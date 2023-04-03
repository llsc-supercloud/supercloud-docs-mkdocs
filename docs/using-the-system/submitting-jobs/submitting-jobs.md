# Submitting Jobs

For most job types, there are two ways to start the job: using the
commands provided by the scheduler, Slurm, or using wrapper command,
LLsub, that we have provided. LLsub creates a scheduler command based on
the arguments you feed it, and will output that command to show you what
it is running. The scheduler commands may provide more flexibility, and
the wrapper commands may be easier to use in some cases and are
scheduler agnostic. We show some of the more commonly used options. More
Slurm options can be seen on the Slurm
[documentation](http://slurm.schedmd.com/man_index.html) page, and more
LLsub options can be seen by running `LLsub -h` at the command line.

There are two main types of jobs that you can run: interactive and batch
jobs. Interactive jobs allow you to run interactively on a compute node
in a shell. Batch jobs, on the other hand, are for running a pre-written
script or executable. Interactive jobs are mainly used for testing,
debugging, and interactive data analysis. Batch jobs are the traditional
jobs you see on an HPC system and should be used when you want to run a
script that doesn't require that you interact with it.

On this page we will go over:

- [How to start an Interactive Job with LLsub](submitting-jobs.md#how-to-start-an-interactive-job-with-llsub)
- [How to submit a Basic Serial job with LLsub and sbatch](submitting-jobs.md#submitting-a-simple-serial-batch-job)
- [How to request more resources with sbatch](submitting-jobs.md#requesting-additional-resources-with-sbatch)
- [How to request more resources with LLsub](submitting-jobs.md#requesting-additional-resources-with-llsub-llsub)
- [How to submit an LLMapReduce Job](submitting-jobs.md#llmapreduce)
- [How to submit a job with pMatlab, sbatch, or LaunchFunctionOnGrid](submitting-jobs.md#matlaboctave-tools)
- [How to get the most performance out of LLsub, LLMapReduce, and pMatlab using Triples Mode](submitting-jobs.md#triples-mode)

You can find examples of several job types in the [Teaching
Examples](https://github.com/llsc-supercloud/teaching-examples) github
repository. They are also in the `bwedx`shared group directory and
anyone with a Supercloud account can copy them to their home directory
and use them as a starting point.

## How to start an Interactive Job with LLsub

Interactive jobs allow you to run interactively on a compute node in a
shell. Interactive jobs are mainly used for testing, debugging, and
interactive data analysis.

Starting an interactive job with LLsub is very simple. To request a
single core, run at the command line:

> `LLsub -i`

As mentioned earlier on this page, when you run an LLsub command,
you'll see the Slurm command that is being run in the background when
you submit the job. Once your interactive job has started, you'll see
the command line prompt has changed. It'll say something like:

> `USERNAME@d-14-13-1:~$`

Where `USERNAME` is your username, and `d-14-13-1` is the hostname of
the machine you are on. This is how you know you are now on a compute
node in an interactive job.

By default you will be allocated a single CPU core. We have a number of
options that allow you to request additional resources. You can always
view these options and more by running `LLsub -h`. We'll go over a few
of those here. Note that these can (and often should) be combined.

- **Full Exclusive Node:** Add the word `full` to request an exclusive
    node. No one else will be on the machine with you:

> `LLsub -i full`

- **A number of cores:** Use the `-s` option to request a certain
    number of CPU cores, or slots. Here, for example, we are requesting
    4 cores:

> `LLsub -i -s 4`

- **GPUs:** Use the `-g` option to request a GPU. You need to specify
    the GPU type and the number of GPUs you want. You can request up to
    the number of GPUs on a single node. Refer to the
    [Systems and Software](../../systems-and-software.md) page to see how
    many GPUs are available per node. Remember you may want to also
    allocate some number of CPUs in addition to your GPUs. To get 20
    CPUs and 1 Volta GPU (half the resources on our Xeon-G6 nodes), you
    would run:

> `LLsub -i -s 20 -g volta:1`

## Submitting a Simple Serial Batch Job

Submitting a batch job to the scheduler is the same for most languages.
This starts by writing a submission script. This script should be a bash
script (it should start with `#!/bin/bash`) and contain the command(s)
you need to run your code from the command line. It can also contain
scheduler flags at the beginning of the script, or load modules or set
environment variables you need to run your code.

A job submission script for a simple, serial, batch job (for example,
running a python script) looks like this:

```bash
#!/bin/bash

# Loading the required module
module load anaconda/2023a

# Run the script
python myScript.py
```

The first line is the `#!/bin/bash` mentioned earlier. It looks like a
comment, but it isn't. This tells the machine how to interpret the
script, that it is a bash script. Lines 3 and 4 demonstrate how to load
a module in a submission script. The final line of the script runs your
code. This should be the command you use to run your code from the
command line, including any input arguments. This example is running a
python script, therefore we have `python myScript.py`.

### Submitting with LLsub

To submit a simple batch job, you can use the LLsub command:

> `LLsub myScript.sh`

Here `myScript.sh` can be a job submission script, or could be replaced
by a compiled executable. The `LLsub` command, with no arguments,
creates a scheduler command with some default options. If your
submission script is `myScript.sh`, your output file will be
`myScript.sh.log-%j`, where `%j` is a unique numeric identifier, the
JobID for your job. The output file is where all the output for your job
gets written. Anything that normally is written to the screen when you
run your code, including any errors or print statements, will be printed
to this file.

When you run this command, the scheduler will find available resources
to launch your job to. Then `myScript.sh` will run to completion, and
the job will finish when the script is complete.

### Submitting with Slurm Scheduler Commands

To submit a simple batch job with the same default behavior as LLsub
above, you would run:

> `sbatch -o myScript.sh.log-%j myScript.sh`

Here `myScript.sh` can be a job submission script, or could be replaced
by a compiled executable. The `-o` flag states the name of the file
where any output will be written, the `%j` portion indicates job ID. If
you do not include this flag, any output will be written to
`slurm-JOBID.out`, which may make it difficult differentiate between job
outputs.

You can also incorporate this flag into your job submission script by
adding lines starting with `#SBATCH` followed by the flag right after
the first `#!/bin/bash` line:

```bash
#!/bin/bash

# Slurm sbatch options
#SBATCH -o myScript.sh.log-%j

# Loading the required module(s)
module load anaconda/2023a

# Run the script
python myScript.py
```

Like `#!/bin/bash`, these lines starting with `#SBATCH` look like
comments, but they are not. As you add more flags to specify what
resources your job needs, it becomes easier to specify them in your
submission script, rather than having to type them out at the command
line. If you incorporate Slurm flags in your script like this, you can
submit it by running:

> `sbatch myScript.sh`

When you run these commands, the scheduler will find available resources
to launch your job to. Then `myScript.sh` will run to completion, and
the job will finish when the script is complete.

Note that when you start adding additional resources you need to make a
choice between using `LLsub` and `sbatch`. If you have `sbatch` options
in your submission script and submit it with `LLsub`, `LLsub` will
ignore any additional command line arguments you give it and use those
described in the script.

## Requesting Additional Resources with sbatch


By default you will be allocated a single core for your job. This is
fine for testing, but usually you'll want more than that. For example
you may want:

- [Additional cores on multiple nodes (distributed)](submitting-jobs.md#additional-cores-on-multiple-nodes)
- [Additional cores on the same node (shared memory or threading)](#additional-cores-on-the-same-node)
- [Multiple independent tasks (job array/throughput)](#job-arrays)
- [Exclusive node(s)](#exclusive-nodes)
- [More memory or cores per process/task/worker](#adding-more-memory-or-cores-per-task)
- [GPUs](#requesting-gpus)

Here we have listed and will go over some of the more common resource
requests. Most of these you can combine to get what you want. We will
show the lines that you would add to your submission script, but note
that you can also include these options at the command line if you want.

How do you know what you should request? An in-depth discussion on this
is outside the scope of this documentation, but we can provide some
basic guidance. Generally, parallel programs are either implemented to
be distributed or not. Distributed programs can communicate across
different nodes, and so can scale beyond a single node. Programs written
with MPI, for example, would be distributed. Non-Distributed programs
you may see referred to as shared memory or multithreaded. Python's
multiprocessing package is a good example of a shared memory library.
Whether your program is Distributed or Shared Memory dictates how you
request additional cores: do they need to be all on the same node, or
can they be on different nodes? You also want to think about what you
are running: if you are running a series of identical independent tasks,
say you are running the same code over a number of files or parameters,
this is referred to as Throughput and can be run in parallel using a Job
Array. (If you are iterating over files like this, and have some
reduction step at the end, take a look at
[LLMapReduce](#llmapreduce)). Finally,
you may want to think about whether your job could use more than the
default amount of memory, or RAM, and whether it can make use of a GPU.

### Additional Cores on Multiple Nodes

The flag to request a certain number of cores that can be on more than
one node is `--ntasks`, or `-n` for short. A task is Slurm's
terminology for an individual process or worker. For example, to request
4 tasks you can add the following to your submission script:

> `#SBATCH -n 4`

You can control how many nodes these tasks are split onto using the
`--nodes`, or `-N`. Your tasks will be split evenly across the nodes you
request. For example, if I were to have the following in my script:

> `#SBATCH -n 4 #SBATCH -N 2`

I would have four tasks on two nodes, two tasks on each node. Specify
the number of nodes like this does not ensure that you have exclusive
access to those nodes. It will by default allocate one core for each
task, so in this case you'd get a total of four cores, two on each
node. If you need more than one core for each task, take a look at the
[cpus-per-task](#adding-more-memory-or-cores-per-task) option,
and if you need exclusive access to those nodes see the
[exclusive](#exclusive-nodes) option.

### Additional Cores on the Same Node

There are technically two ways to do this. You can use the same options
as requesting tasks on multiple nodes and setting the number of Nodes to
1, say we want four cores:

> `#SBATCH -n 4 #SBATCH -N 1`

Or you can use `-c`, or the `--cpus-per-task` option by itself:

> `#SBATCH -c 4`

As far as the number of cores you get, the result will be the same.
You'll get the four cores on a single node. There is a bit of a nuance
on how Slurm sees it. The first allocates four tasks all on one node.
The second allocates a single task with four CPUs or cores. You don't
need to worry too much about this, choose whichever makes the most sense
to you.

### Job Arrays

**NOTE:** We encourage everyone who runs a job array to use LLsub with
Triples mode. See the page [LLsub Job Array Triples](job-array-triples.md) to see how to set this up.

A simple way to run the same script or command with different parameters
or on different files in parallel is by using a Job Array. With a Job
Array, the parallelism happens at the Scheduler level and is completely
language agnostic. The best way to use a Job Array is to batch up your
parameters so you have a finite number of tasks each running a set of
parameters, rather than one task for each parameter. In your submission
script you specify numeric indices, corresponding to the number of tasks
that you want running at once. Those indices, or Task IDs are captured
in environment variables, along with the total number of tasks, and
passed into your script. Your script then has the information it needs
to split up the work among tasks. This process is described in the
[Teaching
Examples](https://github.com/llsc-supercloud/teaching-examples) github
repository, with examples in
[Julia](https://github.com/llsc-supercloud/teaching-examples/tree/master/Julia/word_count/JobArray)
and
[Python](https://github.com/llsc-supercloud/teaching-examples/tree/master/Python/word_count/JobArray).

First you want to take a look at your code. Code that can be submitted
as a Job Array usually has one big for loop. If you are iterating over
multiple parameters or files, and have nested for loops, you'll first
want to enumerate all the combinations of what you are iterating over so
you have one big loop. Then you want to add a few lines to your code to
take in two arguments, the Task ID and the number of tasks, use those
numbers to split up the thing you are iterating over. For example, I
might have a list of filenames, `fnames`. In python I would add:

```python
# Grab the arguments that are passed in
my_task_id = int(sys.argv[1])
num_tasks = int(sys.argv[2])`

# Assign indices to this process/task
my_fnames = fnames[my_task_id-1:len(fnames):num_tasks]

for f in my_fnames: ...
```

Notice that I am iterating over `my_fnames`, which is a subset of the
full list of filenames determined by the task ID and number of tasks.
This subset will be different for each task in the array. Note that the
third line of code will be different for languages with arrays that
start at index 1 (see the [Julia Job
Array](https://github.com/llsc-supercloud/teaching-examples/tree/master/Julia/word_count/JobArray)
code for an example of this).

The submission script will look like this:

```bash
#!/bin/bash

#SBATCH -o myScript.sh.log-%j-%a
#SBATCH -a 1-4

# Loading the required module(s)
module load anaconda/2023a

python top5each.py $SLURM_ARRAY_TASK_ID $SLURM_ARRAY_TASK_COUNT
```

The `-a` (or `--array`) option is where you specify your array indices,
or task IDs. Here I am creating an array with four tasks by specifying 1
"through" 4. When the scheduler starts your job, it will start up four
independent tasks, each will run this script, and each will have
`#SLURM_ARRAY_TASK_ID` set to its task ID. Similarly,
`$SLURM_ARRAY_TASK_COUNT` will be set to the total number of tasks, in
this case 4.

You may have noticed that there is an additional `%a` in the output file
name. There will be one output file for each task in the array, and the
`%a` puts the task ID on at the end of the filename, so you know which
file goes with which task.

By default you will get one core for each task in the array. If you need
more than one core for each task, take a look at the
[cpus-per-task](#adding-more-memory-or-cores-per-task) option,
and if you need to add a GPU to each task, check out the the
[GPUs](#requesting-gpus) section.

### Exclusive Nodes

Requesting an exclusive node ensures that there will be no other users
on the node with you. You might want to do this when you know you need
to make use of the full node, when you are running performance tests, or
when you think your program might affect other users. There is some
software that have not been designed for a shared HPC environment, and
so use all the cores on the node, whether you have allocated them or
not. You can look through their documentation to see if there is a way
to limit the number of cores it uses, or you can request an exclusive
node. Another situation where you might affect other users is when you
don't yet know what resources your code requires. For these first few
runs it makes sense to request an exclusive node, and then look at the
resources that your job used, and request those resources in the future.

To request an exclusive node or nodes, you can add the following option:

> `#SBATCH --exclusive`

This will ensure that wherever the tasks in your job land, those nodes
will be exclusive. If you have four tasks, for example, specified with
either `-n` (`--ntasks`) or in a job array, and those four tasks fall on
the same node, you will get that one node exclusively. It will not force
each task onto its own exclusive node without adding other options.

### Adding More Memory or Cores per Task

You can ensure that each task has more than one core or the default
amount of memory the same way. By default, each core gets its fair share
of the RAM on the node, calculated by the total amount of memory on the
node divided by the number of cores. See the
[Systems and Software](../../systems-and-software.md) page for a list of
the amount of RAM, number of cores, and RAM per core for each resource
type. For example, with the Xeon-P8 nodes, they have 192 GB of RAM and
48 cores, so each core gets 4 GB of RAM. Therefore, the way to request
more memory is to request more cores. Even if you are not using the
additional core(s), you are using their memory. The way to do this is
using the `--cpus-per-task`, or `-c` option. Say I know each task in my
job will use about 20 GB of memory, with the Xeon-P8 nodes above, I'd
want to request five cores for each task:

> `#SBATCH -c 5`

This works nicely with both the `-n` (`--ntasks`) and `-a` (`--array`)
options. As the flag name implies, you will get 5 cpu cores for every
task in your job. If you are already using the `-c` option for a shared
memory or threaded job, you can either use the `-n` and `-N 1`
alternative and save `-c` for adding additional memory, or you can
increase what you put for `-c`. For example, if I know I'm going to use
4 cores in my code, but each will need 20 GB of RAM, I can request a
total of 4*5 = 20 cores.

How do you know how much memory your job needs? You can find out how
much memory a job used after the job is completed. You can run your job
long enough to get an idea of the memory requirement first in [exclusive](#exclusive-nodes)
 mode so your job can have access to
the maximum amount of memory. Then you can use the `sacct` slurm command
to get the memory used:

> `sacct -j JOBID -o JobID,JobName,State,AllocCPUS,MaxRSS --units=G`

where JOBID is your job ID. State shows the job status, keep in mind
that the memory numbers are only accurate for jobs that are no longer
running, and AllocCPUS is the number of CPU cores that were allocated to
the job. MaxRSS is the maximum resident memory (maximum memory
footprint) used by each job.

If the MaxRSS value is larger than the per-slot/core memory limit for
the compute node (again, check the
[Systems and Software](../../systems-and-software.md) page to get this
for the resource type you are requesting), you will have to request
additional memory for your job.

This formatting for the accounting data prints out a number of memory
datapoints for the job. They are all described in the [sacct man
page](https://slurm.schedmd.com/sacct.html).

### Requesting GPUs

Some code can be accelerated by adding a GPU, or Graphical Processing
Unit. GPUs are specialized hardware originally developed for rendering
the graphics you see on your computer screen, but have been found to be
very fast at doing certain operations and have therefore been adopted as
an accelerator. They are frequently used in Machine Learning libraries,
but are increasingly used in other software. You can also write your own
GPU code using CUDA.

Before requesting a GPU, you should verify that the software, libraries,
or code that you are using can make use of a GPU, or multiple GPUs. The
Machine Learning packages available in our anaconda modules should all
be able to take advantage of GPUs. To request a single GPU, add the
following line to your submission script:

> `#SBATCH --gres=gpu:volta:1`

This flag will give you a single GPU. For multi-node jobs, it'll give
you a single GPU for every node you end up on, and will give you a
single GPU for every task in a Job Array. If your code can make use of
multiple GPUs, you can set this to 2 instead of 1, and that will give
you 2 GPUs for each node or Job Array task.

Note that only certain operations are being done on the GPU, your job
will still most likely run best given a number of CPU cores as well. If
you are not sure how many to request, if you request 1 GPU, ask for 20
CPUs (half of the CPUs), if you request 2 GPUs, you can ask for all of
the CPUs. You can check the current CPU and GPU counts for each node on
our [Systems and Software](../../systems-and-software.md) page.

## Requesting Additional Resources with LLsub

By default you will be allocated a single core for your job. This is
fine for testing, but usually you'll want more than that. For example
you may want:

- [Additional cores on the same node (shared memory or threading)](#additional-cores-on-the-same-node-with-llsub)
- [Multiple independent tasks (job array/throughput)](job-array-triples.md)
- [More memory or cores per process/task/worker](#adding-more-memory-or-cores)
- [GPUs](#requesting-gpus-with-llsub)

Here we have listed and will go over some of the more common resource
requests. Most of these you can combine to get what you want. We will
show the lines that you would add to your submission script, but note
that you can also include these options at the command line if you want.

How do you know what you should request? An in-depth discussion on this
is outside the scope of this documentation, but we can provide some
basic guidance. Generally, parallel programs are either implemented to
be distributed or not. Distributed programs can communicate across
different nodes, and so can scale beyond a single node. Programs written
with MPI, for example, would be distributed. Non-Distributed programs
you may see referred to as shared memory or multithreaded. Python's
multiprocessing package is a good example of a shared memory library.
Whether your program is Distributed or Shared Memory dictates how you
request additional cores: do they need to be all on the same node, or
can they be on different nodes? You also want to think about what you
are running: if you are running a series of identical independent tasks,
say you are running the same code over a number of files or parameters,
this is referred to as Throughput and can be run in parallel using a Job
Array. (If you are iterating over files like this, and have some
reduction step at the end, take a look at
[LLMapReduce](#llmapreduce)). Finally,
you may want to think about whether your job could use more than the
default amount of memory, or RAM, and whether it can make use of a GPU.

If you are submitting your job with LLsub, you should be aware of its
behavior. If you have any Slurm options in your submission script (any
lines starting with `#SBATCH`) LLsub will ignore any command line
arguments you give it and only use those you specify in your script. You
can still submit this script with LLsub, but it won't add any extra
command line arguments you pass it.

### Additional Cores on the Same Node with LLsub

Libraries that use shared memory or threading to handle parallelism
require that all cores be on the same node. In this case you are
constrained to the number of cores on a single machine. Check the
[Systems and Software](../../systems-and-software.md) page to see the
number of cores available on the current hardware.

To request multiple cores on the same node for your job you can use the
`-s` option in `LLsub`. This stands for "slots". For example, if I am
running a job and I'd like to allocate 4 cores to it, I would run:

> `LLsub myScript.sh -s 4`

### Job Array

See [LLsub Job Array Triples](job-array-triples.md).

### Adding More Memory or Cores

If you anticipate that your job will use more than ~4 GB of RAM, you
may need to allocate more resources for your job. You can be sure your
job has enough memory to run by allocating more slots, or cores, to each
task or process in your job. Each core gets its fair share of the RAM on
the node, calculated by the total amount of memory on the node divided
by the number of cores. See the [Systems and Software](../../systems-and-software.md)
 page for a list of the amount of RAM, number of cores, and
RAM per core for each resource type. For example, the Xeon-P8 nodes have
192 GB of RAM and 48 cores, so each core gets 4 GB of RAM. Therefore,
the way to request more memory is to request more cores. Even if you are
not using the additional core(s), you are using their memory. The way to
do with LLsub is the `-s` (for slots) option. Say I know each task in my
job will use about 20 GB of memory, with the Xeon-P8 nodes above, I'd
want to request five cores for each task:

> `LLsub myScript.sh -s 5`

If you are already using the `-s` option for a shared memory or threaded
job, you should increase what you put for `-s`. For example, if I know
I'm going to use 4 cores in my code, but each will need 20 GB of RAM, I
can request a total of 4*5 = 20 cores:

> `LLsub myScript.sh -s 20`

How do you know how much memory your job needs? You can find out how
much memory a job used after the job is completed. You can run your job
long enough to get an idea of the memory requirement first (you can
request the maximum number of cores per node for this step). Then you
can use the `sacct` slurm command to get the memory used:

> `sacct -j JOBID -oJobID,JobName,State,AllocCPUS,MaxRSS --units=G`

where JOBID is your job ID. State shows the job status, keep in mind
that the memory numbers are only accurate for jobs that are no longer
running, and AllocCPUS is the number of CPU cores that were allocated to
the job. MaxRSS is the maximum resident memory (maximum memory
footprint) used by each job.

If the MaxRSS value is larger than the per-slot/core memory limit for
the compute node (again, check the
[Systems and Software](../../systems-and-software.md) page to get this
for the resource type you are requesting), you will have to request
additional memory for your job.

This formatting for the accounting data prints out a number of memory
data points for the job. They are all described in the [sacct man
page](https://slurm.schedmd.com/sacct.html).

### Requesting GPUs with LLsub

Some code can be accelerated by adding a GPU, or Graphical Processing
Unit. GPUs are specialized hardware originally developed for rendering
the graphics you see on your computer screen, but have been found to be
very fast at doing certain operations and have therefore been adopted as
an accelerator. They are frequently used in Machine Learning libraries,
but are increasingly used in other software. You can also write your own
GPU code using CUDA.

Before requesting a GPU, you should verify that the software, libraries,
or code that you are using can make use of a GPU, or multiple GPUs. The
Machine Learning packages available in our anaconda modules should all
be able to take advantage of GPUs. To request a single GPU, use the
following command:

> `LLsub myScript.sh -g volta:1`

This flag will give you a single GPU. For multi-node jobs, it'll give
you a single GPU for every node you end up on, and will give you a
single GPU for every task in a Job Array. If your code can make use of
multiple GPUs, you can set this to 2 instead of 1, and that will give
you 2 GPUs for each node or Job Array task.

Note that only certain operations are being done on the GPU, your job
will still most likely run best given a number of CPU cores as well. If
you are not sure how many to request, if you request 1 GPU, ask for 20
CPUs (half of the CPUs), if you request 2 GPUs, you can ask for all of
the CPUs. You can check the current CPU and GPU counts for each node on
our [Systems and Software](../../systems-and-software.md) page. To
request 20 cores and 1 GPU, run:

> `LLsub myScript.sh -s 20 -g volta:1`

LLMapReduce
-----------

The LLMapReduce command scans the user-specified input directory and
translates each individual file as a computing task for the
user-specified application. Then, the computing tasks will be submitted
to scheduler for processing. If needed, the results can be
post-processed by setting up a user-specified reduce task, which is
dependent on the mapping task results. The reduce task will wait until
all the results become available.

You can view the most up-to-date options for the LLMapReduce command by
running the command LLMapReduce -h. You can see examples of how to use
LLMapReduce jobs in /usr/local/examples directory on the Supercloud
system nodes. Some of these may be in the `examples` directory in your
home directory. You can copy any that are missing from
`/usr/local/examples` to your home directory. We also have an example in
the [Teaching
Examples](https://github.com/llsc-supercloud/teaching-examples) github
repository, with examples in
[Julia](https://github.com/llsc-supercloud/teaching-examples/tree/master/Julia/word_count/LLMapReduce)
and
[Python](https://github.com/llsc-supercloud/teaching-examples/tree/master/Python/word_count/LLMapReduce).
These examples are also available in the bwedx shared group directory
and can be copied to your home directory from there.

LLMapReduce can work with any programs and we have a couple of examples
for Java, Matlab, Julia, and Python. By default, it cleans up the
temporary directory, `MAPRED.PID`. However, there is an option to keep
(`--keep=true`) the temporary directory if you want it for debugging. The
current version also supports a nested LLMapReduce call.

Matlab/Octave Tools
-------------------

### pMatlab

pMatlab was created at MIT Lincoln Laboratory to provide easy access to
parallel computing for engineers and scientists using the MATLAB(R)
language. pMatlab provides the interfaces to the communication libraries
necessary for distributed computation. In addition to MATLAB(R), pMatlab
works seamlessly with Octave, and open source Matlab toolkit.

MATLAB(R) is the primary development language used by Laboratory staff,
and thus the place to start when developing an infrastructure aimed at
removing the traditional hurdles associated with parallel computing. In
an effort to develop a tool that will enable the researcher to
seamlessly move from desktop (serial) to parallel computing, pMatlab has
adopted the use of Global Array Semantics. Global Array Semantics is a
parallel programming model in which the programmer views an array as a
single global array rather than multiple subarrays located on different
processors. The ability to access and manipulate related data
distributed across processors as a single array more closely matches the
serial programming model than the traditional parallel approach, which
requires keeping track of which data resides on any given individual
processor.

Along with global array semantics, pMatlab uses the message-passing
capabilities of MatlabMPI to provide a global array interface to
MATLAB(R)) programmers. The ultimate goal of pMatlab is to move beyond
basic messaging (and its inherent programming complexity) towards higher
level parallel data structures and functions, allowing MATLAB(R)) users
to parallelize their existing programs by simply changing and adding a
few lines.

Any pMatlab code can be run on the MIT Supercloud using standard pMatlab
submission commands. The Practical High Performance Computing course on
our online course platform provides a very good introduction for how to
use pMatlab. There is also an examples directory in your home directory
that provides several examples. The Param_Sweep example is a good place
to start. There is an in-depth explanation of this example in the
[Teaching
Examples](https://github.com/llsc-supercloud/teaching-examples/tree/master/Matlab-Octave/Param_Sweep)
github repository.

If you anticipate that your job will use more than 4 GB of RAM, you may
need to allocated more resources for your job. You can be sure your job
has enough memory to run by allocating more slots, or cores, to each
task or process in your job. For example, our xeon-p8 nodes have 48 cores and
192 GB of RAM, therefore each core represents about 4 GB. So if your
job needs ~8 GB, allocate two cores or slots per process. Doing so
will ensure your job will not fail due running out of memory, and not
interfere with someone else's job.

To do this with pMatlab, you can add the following line to your run
script, before you the `eval(pRUN(...))` command:

> `setenv('GRIDMATLAB_MT_SLOTS','2')`

### Submitting with LLsub or Sbatch

You can always submit a Matlab(R) script with a submission script
through sbatch or LLsub. The basic submission script looks like this:

```bash
#!/bin/bash

# Run the script
matlab -nodisplay -r "myScript; exit"
```

Where `myScript` is the name of the Matlab script that you want to run.
When running a Matlab script through a submission script, you do need to
specify that Matlab should exit after it runs your code. Otherwise it
will continue to run, waiting for you to give it the next command.

### LaunchFunctionOnGrid and LaunchParforOnGrid

If you want to launch your serial MATLAB scripts or functions on LLSC
systems, you can use the `LaunchFunctionOnGrid()` function. You can
execute your code without any modification (if it is written for a Linux
environment) as a batch job. Its usage, in Matlab, is as follows:

> `launch_status = LaunchFunctionOnGrid(m_file) launch_status = LaunchFunctionOnGrid(m_file,variables)`

Where `m_file` is a string that specifies the script or function to be
run, and variables is the list of variables that are being passed in.
Note that variables must be variables, not constants.

If you want to launch your MATLAB scripts or functions that call the
`parfor()` function on LLSC systems, you can use the
`LaunchParforOnGrid()` function. You can execute your code without any
modification (if it is written for a Linux environment) as a batch job.
While `LaunchParforOnGrid()` will work functionally, it has significant
limitations in performance, both at the node level and the cluster
level; it might be better to use pMatlab instead. To use the
`LaunchParforOnGrid()` function in MATLAB:

> `launch_status = LaunchParforOnGrid(m_file) launch_status = LaunchParforOnGrid(m_file,variables)`

Where `m_file` is a string that specifies the script or function to be
run, and variables is the list of variables that are being passed in.
Note that variables must be variables, not constants.

If you anticipate that your job will use more than 4 GB of RAM, you may
need to allocated more resources for your job. You can be sure your job
has enough memory to run by allocating more slots, or cores, to each
task or process in your job. For example, our xeon-p8 nodes have 48 cores and
192 GB of RAM, therefore each core represents about 4 GB. So if your
job needs ~8 GB, allocate two cores or slots per process. Doing so
will ensure your job will not fail due running out of memory, and not
interfere with someone else's job.

To do this with LaunchFunctionOnGrid or LaunchParforOnGrid, you can add
the following line to your run script, before you use the
`LaunchFunctionOnGrid()` or `LaunchParforOnGrid()` command:

> `setenv('GRIDMATLAB_MT_SLOTS','2')`

## Triples Mode

Triples mode is a way to launch pMatlab,
[LLsub Job Array](job-array-triples.md), and
LLMapReduce jobs that gives you better performance and more flexibility
to manage memory and threads. Unless you are requesting a small number
of cores for your job, we highly encourage you to migrate to this model.

With triples mode, you specify the resources for your job by providing 3
parameters:

> `[Nodes NPPN NTPP]`

where

- `Nodes`is number of compute nodes
- `NPPN`is number of processes per node
- `NTPP`is number of threads per process (default is 1)

With triples mode your job will have exclusive use of each of the nodes
that you request.

### LLsub

A brief introduction to LLsub is provided
[above](#requesting-additional-resources-with-llsub). To use triples
mode to launch LLsub job on Supercloud, run as follows:

> `LLsub ./submit.sh [Nodes,NPPN,NTPP]`

A more in-depth guide on how to convert an existing Job Array to an
LLsub Triples submission is provided on the page
[LLsub Job Array](job-array-triples.md).

### LLMapReduce with Triples

A brief introduction to LLMapReduce is provided
[above](#llmapreduce). To use triples
mode to launch your LLMapReduce job on Supercloud, use the `--np` option
with the triple as its parameter, as follows:

> `--np=[Nodes,NPPN,NTPP]`

### pMatlab with Triples

A brief introduction to pMatlab is provided
[above](#pmatlab). To use triples mode to
launch your pMatlab job on Supercloud, you use the pRUN() function. Its
usage, in Matlab, is as follows:

> `eval(pRUN('mfile', [Nodes NPPN NTPP], 'grid'))`

### Triples Mode Tuning

Triples mode tuning provides greater efficiency by allowing you to
better tune your resource requests to your application. This one-time
tuning process typically takes ~1 hour:

1. Instrument your code to print a rate (work/time) giving a sense of
    the speed from a ~1 minute run.
2. Determine best number of threads (`NTPPBest`) by examining rate
    from runs with varying numbers of threads:
    `[1,1,1], [1,1,2], [1,1,4]`, \...  
3. Determine best number of processes per node (`NPPNbest`) by
    examining rate from runs with varying numbers of processes:
    `[1,1,NTPPBest], [1,2,NTPPBest], [1,4,NTPPBest]`, \...  
4. Determine best number of nodes (`NodesBest`) by examining rate from
    runs of with varying numbers of nodes:
    `[1,NPPNbest,NTPPBest], [2,NPPNbest,NTPPBest], [4,NPPNbest,NTPPBest]`,
    \...  
5. Run your production jobs using `[NodesBest,NPPNbest,NTPPBest]`

You could tune `NPPN` first, then `NTPP`. This would be a better
approach if you are memory bound. You can find the max `NPPN` that will
fit, then keep increasing `NTPP` until you stop getting more
performance.

"Good" `NPPN` values for Xeon-P8: 1, 2, 4, 8, 16, 24, 32, 48

"Good" `NPPN` values for Xeon-G6: 1, 2, 4, 8, 16, 20, 32, 40

Triples mode tuning results in a ~2x increase efficiency for many
users.

Once the best settings have been found, they can be reused as long as
the code remains roughly similar. Recording the rates from the above
process can often result in a publishable [IEEE
HPEC](http://www.ieee-hpec.org/) paper. We are happy to work with you to
guide you through this tuning process.
