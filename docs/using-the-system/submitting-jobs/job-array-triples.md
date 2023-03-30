Job Arrays with LLsub Triples in 3 Steps
========================================

If you are currently running a Job Array, you can take advantage of
SuperCloud's triples mode submission by submitting your job array with
LLsub. In most cases, this can be done in a few easy steps.

What is triples mode? It's a different way to submit your job by
specifying three numbers:

- Nodes: The number of nodes you want to use up
- NPPN: The number of processes that should run per node
- NTPP: The number of threads per process

Why would you want to use triples mode to submit your job? Triples mode
provides a few advantages. Since it does whole-node scheduling you
don't have to worry about other jobs impacting yours (or your job
impacting someone else's). We also do task-pinning when you request
resources with a triple, so your processes are arranged in the best way
possible for the layout of the hardware architecture. Finally,
submitting a Job Array with LLsub triples mode the way we describe here
will greatly reduce the startup time for your jobs, over a job array
with many pending tasks.

Step 1: Batch Up your Array
---------------------------

The first thing you want to do is set up your job so that your script
splits up your inputs among each process/task and each process/task
iterates a subset of your inputs. This way you can change your input set
without changing the number of tasks or processes that you schedule.
This step alone will save you on startup time. Anytime you submit more
jobs than your allocation allows, those additional jobs will remaining
pending until some of your first running jobs complete. Then, when one
of your first jobs complete, the scheduler now has to find the pending
job some resources and start it running. If you batch up your job array,
you only have to go through the scheduling process once. If you've set
up a job array following our instructions in the past you may already be
doing this, and you can skip to the next section.

First you want to take a look at your code. Code that can be submitted
as a Job Array usually has one big for loop. If you are iterating over
multiple parameters or files, and have nested for loops, you'll first
want to enumerate all the combinations of what you are iterating over so
you have one big loop.

If your code is written so it uses the `$SLURM_ARRAY_TASK_ID` and uses
that to run a single thing, first add a for loop that iterates over the
full set of things you want to run your code on. If you can't rewrite
your code in such a way that it iterates over multiple inputs, you can
use LLMapReduce to submit your job with triples mode, see [this
example](https://github.com/llsc-supercloud/teaching-examples/tree/master/C-Cpp/Fibonacci/LLMapReduce).

Then you add a few lines to your code to take in two arguments, a
process/task ID and the number of processes/tasks, and use those numbers
to split up the thing you are iterating over. For example, I might have
a list of filenames, `fnames`. In python I would add:

```python
# Grab the arguments that are passed in
my_task_id = int(sys.argv[1])
num_tasks = int(sys.argv[2])

# Assign indices to this process/task
my_fnames = fnames[my_task_id:len(fnames):num_tasks]

for f in my_fnames: ...
```

Notice that I am iterating over `my_fnames`, which is a subset of the
full list of filenames determined by the task ID and number of tasks.
This subset will be different for each task in the array. Note that the
third line of code will be different for languages with arrays that
start at index 1 (see the [Julia Job
Array](https://github.com/llsc-supercloud/teaching-examples/tree/master/Julia/word_count/JobArray)
code for an example of this).

Step 2: Changing Your Submission Script
---------------------------------------

If you have been running your Job Arrays with sbatch, you most likely
have a few environment variables in your submission script. To submit
with LLsub triples, you can just replace these:

- `$SLURM_ARRAY_TASK_ID` -\> `$LLSUB_RANK`
- `$SLURM_ARRAY_TASK_COUNT` -\> `$LLSUB_SIZE`

**If you have any SBATCH flags in your submission script, remove those
as well** (LLsub will see these and submit with sbatch, ignoring any
command line arguments you give it). For example, if you are running a
python script, your final submission script will look something like
this:


```bash
#!/bin/bash

# Load Module(s)
module load anaconda/2023a

echo "My task ID: " $LLSUB_RANK
echo "Number of Tasks: " $LLSUB_SIZE

python top5each.py $LLSUB_RANK $LLSUB_SIZE
```

You will also need to make your script executable. You can do that with
this simple command line command:

> `chmod u+x submit_LLsub.sh`

Step 3: Submit your Job with LLsub Triples
------------------------------------------

Now when you submit your job, you can run:

> `LLsub ./submit.sh [NODES,NPPN,NTPP]`

where

- `NODES` is the number of nodes you want to use up
- `NPPN` is the number of processes that should run per node
- `NTPP` is the number of threads per process

The total number of threads per node, or `NTPP*NPPN`, should not be more
than the number of cores on the node, otherwise you may overwhelm the
node with too many running processes and/or threads. For example, if you
are running on the 48-core Xeon-P8 nodes, if you are running with
`NTPP`=1, you should not set `NPPN` more than 48. If `NTPP`=2, `NPPN` should
be at most 24, etc. The numbers you choose depend on your application,
if it is multithreaded it may be worth increasing `NTPP` and decreasing
`NPPN`. If your application consumes a lot of memory, you may need to
decrease `NPPN` so each process has the memory it needs to proceed. The
best way to determine what numbers to choose is to
[tune your triples](submitting-jobs.md#triples-mode-tuning), which is a
relatively quick exercise and can improve your speedup in the long run
by helping you select the ideal numbers for your triple.

So if you want to run on 2 nodes, 10 processes per node, and 4 threads
per process, you would run:

> `LLsub ./submit.sh [2,10,4]`

If you were to run LLstat after running this command, you would see what
looks like a 2 task job array, for example:

```bash
$ LLstat
LLGrid: txe1 (running slurm-wlm 20.11.3)
JOBID ARRAY_JOB_ NAME USER START_TIME PARTITION CPUS FEATURES MIN_MEMORY ST NODELIST(REASON)
9651412_1 9651412 submit_LLsub.sh studentx 2021-03-19T10:32:26 normal 40 xeon-g6 8500M R d-13-8-2
9651412_2 9651412 submit_LLsub.sh studentx 2021-03-19T10:32:26 normal 40 xeon-g6 8500M R d-13-8-1
```

However, you are still running 2*10 = 20 total processes. Triples mode
uses slurm to request full nodes, then takes care of launching the
processes on each node. This is why you'll always see one task for each
node in the `LLstat` output, rather than each process. One advantage of
this is it makes for a more compact `LLstat` output that is easier to
read, instead of having to scroll through tons of tasks.

You can check on your individual processes by looking at the log files.
LLsub with Triples mode will create a directory with the prefix
"LLSUB" followed by a unique number to hold all the log files. Within
this directory will be one launch log file, which will capture any
errors that occur during launch ,and one directory per node and put the
log files for each process in its node's directory. These
subdirectories are labeled with the process ID range and the node name.

For example, here are the log files from a triples run using [2,4,10]:

```bash
$ ls LLSUB.23004
README.md helpers.py submit_LLsub.sh submit_sbatch.sh top5each.py

$ ls LLSUB.23004/
llsub-triple-mode-launch.log-10006591 p0-p3_d-19-4-1 p4-p7_d-19-3-4

$ ls LLSUB.23004/p0-p3_d-19-4-1/
submit_LLsub.sh.log.4 submit_LLsub.sh.log.5 submit_LLsub.sh.log.6 submit_LLsub.sh.log.7

$ ls LLSUB.23004/p4-p7_d-19-3-4/
submit_LLsub.sh.log.4 submit_LLsub.sh.log.5 submit_LLsub.sh.log.6 submit_LLsub.sh.log.7
```

In this example, `LLSUB.23004` is the directory containing the log
files, `llsub-triple-mode-launch.log-10006591` is the log file for the
job launch. `p0-p3_d-19-4-1` and `p4-p7_d-19-3-4` are the directories
for each node, and `submit_LLsub.sh.log.0`, `submit_LLsub.sh.log.1`,
..., `submit_LLsub.sh.log.7` are the log files for each process.
