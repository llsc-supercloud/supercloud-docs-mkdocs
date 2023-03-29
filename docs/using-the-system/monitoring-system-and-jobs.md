Monitoring System and Job Status {#monitoring_system_and_jobs}
================================

The four actions you may take the most are checking system status and
starting, monitoring, and stopping jobs. Since scheduling jobs is a
longer topic, see this `page <submitting_jobs>`{.interpreted-text
role="ref"} for an in-depth description of how to start your job. Here
we describe how to check the
`status of the system <#system-status>`{.interpreted-text role="ref"}
for available resources,
`monitor a currently running job <#monitoring-jobs>`{.interpreted-text
role="ref"}, and `stop a
running job <#stopping-jobs>`{.interpreted-text role="ref"}.

Each of these tasks is done through the scheduler, which is Slurm on the
MIT Supercloud system. On this page and the `job submission
page <submitting_jobs>`{.interpreted-text role="ref"} we describe some
of the basic options for submitting, monitoring, and stopping jobs. More
advanced options are described in Slurm\'s
[documentation](https://slurm.schedmd.com/man_index.html), and this
handy [two-page guide](https://slurm.schedmd.com/pdfs/summary.pdf) gives
a breif description of the commands and their options.

Checking System Status {##system-status}
----------------------

Our wrapper command, `LLGrid_status`, has a nicely formatted and easy to
read output for checking system status:

> | `[StudentX@login-0 ~]$ LLGrid_status`
> | `LLGrid: txe1 (running slurm 16.05.8)   ============================================   Online Intel xeon-e5 nodes: 36      Unclaimed nodes: 24      Claimed slots: 172      Claimed slots for exclusive jobs: 80   --------------------------------------------      Available slots: 404`

In the output, you can see the name of the system you are on (e1 here),
the scheduler that\'s being used (Slurm), the number of unclaimed nodes,
and the number of available slots.

You can also get information about the system status using the scheduler
command `sinfo`:

> \[`StudentX@login-0 ~]$ sinfo   PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST   normal*      up   infinite     15  down* node-[039,049,051,058,063-072],raid-2   normal*      up   infinite      2    mix gpu-2,node-050   normal*      up   infinite     10  alloc gpu-1,node-[037-038,062,099,109-113]   normal*      up   infinite     24   idle node-[040-048,055-057,059-061,115-119,124,129-130],raid-1   normal*      up   infinite      1   down node-114   ...   gpu          up   infinite      1  alloc gpu-1   dgx          up 1-00:00:00      1   idle txdgx`

This command will list the nodes that are down (unavailable), fully
allocated, partially allocated, and idle. Note that the same node can
appear in multiple partitions.

Monitoring Jobs {##monitoring-jobs}
---------------

You can monitor your jobs using the `LLstat` command:

> `[StudentX@login-0 ~]$ LLstat   LLGrid: txe1 (running slurm 16.05.8)   JOBID     ARRAY_J    NAME        USER     START_TIME          PARTITION  CPUS  FEATURES  MIN_MEMORY  ST  NODELIST(REASON)   40986     40986      myJob       Student  2017-10-19T15:35:46 normal     1     xeon-e5   5G          R   gpu-2   40980_100 40980      myArrayJob  Student  2017-10-19T15:35:37 normal     1     xeon-e5   5G          R   gpu-2   40980_101 40980      myArrayJob  Student  2017-10-19T15:35:37 normal     1     xeon-e5   5G          R   gpu-2   40980_102 40980      myArrayJob  Student  2017-10-19T15:35:37 normal     1     xeon-e5   5G          R   gpu-2`

The output of the LLstat command lists the job IDs of the jobs running,
their names, the start time, the number of cpus per task, its status,
and the node that it is running on. If it is in error state, it lists
that as well.

Stopping Jobs {##stopping-jobs}
-------------

Jobs can be stopped using the `LLkill` command. You specify the list of
job ID\'s, separated by commas that you would like to stop, for example:

`LLkill 40986,40980`

Stops the jobs with job IDs 40986 and 40980. You can also use the
`LLkill` command to stop all of your currently running jobs:

`LLkill -u USERNAME`
