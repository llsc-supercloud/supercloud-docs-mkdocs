Monitoring System and Job Status
================================

The four actions you may take the most are checking system status and
starting, monitoring, and stopping jobs. Since scheduling jobs is a
longer topic, see this [page](submitting-jobs/submitting-jobs.md) for an in-depth description of how to start your job. Here
we describe how to check the
[status of the system](#checking-system-status)
for available resources,
[monitor a currently running job](#monitoring-jobs)), and [stop a
running job](#stopping-jobs).

Each of these tasks is done through the scheduler, which is Slurm on the
MIT Supercloud system. On this page and the [job submission
page](submitting-jobs/submitting-jobs.md) we describe some
of the basic options for submitting, monitoring, and stopping jobs. More
advanced options are described in Slurm's
[documentation](https://slurm.schedmd.com/man_index.html), and this
handy [two-page guide](https://slurm.schedmd.com/pdfs/summary.pdf) gives
a brief description of the commands and their options.

Checking System Status
----------------------

Our wrapper command, `LLGrid_status`, has a nicely formatted and easy to
read output for checking system status:

```bash
[StudentX@login-0 ~]$ LLGrid_status
LLGrid: txe1 (running slurm 16.05.8)
============================================ 
Online Intel xeon-e5 nodes: 36
Unclaimed nodes: 24
Claimed slots: 172
Claimed slots for exclusive jobs: 80
-------------------------------------------- 
Available slots: 404
```

In the output, you can see the name of the system you are on (e1 here),
the scheduler that's being used (Slurm), the number of unclaimed nodes,
and the number of available slots.

Monitoring Jobs
---------------

You can monitor your jobs using the `LLstat` command:

```bash
[StudentX@login-0 ~]$ LLstat   LLGrid: txe1 (running slurm 16.05.8)
JOBID     ARRAY_J    NAME        USER    START_TIME          PARTITION  CPUS  FEATURES  MIN_MEMORY  ST  NODELIST(REASON)   
40986     40986      myJob      Student  2017-10-19T15:35:46 normal     1     xeon-e5   5G          R   gpu-2  
40980_100 40980      myArrayJob Student  2017-10-19T15:35:37 normal     1     xeon-e5   5G          R   gpu-2  
40980_101 40980      myArrayJob Student  2017-10-19T15:35:37 normal     1     xeon-e5   5G          R   gpu-2  
40980_102 40980      myArrayJob Student  2017-10-19T15:35:37 normal     1     xeon-e5   5G          R   gpu-2
```

The output of the LLstat command lists the job IDs of the jobs running,
their names, the start time, the number of cpus per task, its status,
and the node that it is running on. If it is in error state, it lists
that as well.

Stopping Jobs
-------------

Jobs can be stopped using the `LLkill` command. You specify the list of
job IDs, separated by commas that you would like to stop, for example:

`LLkill 40986,40980`

Stops the jobs with job IDs 40986 and 40980. You can also use the
`LLkill` command to stop all of your currently running jobs:

`LLkill -u USERNAME`
