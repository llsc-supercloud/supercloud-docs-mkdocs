Best Practices for Using the Filesystem
=======================================

Installing Python Packages
--------------------------

- Use our anaconda modules whenever possible. The newest anaconda
    module should have the most up to date versions. Our anaconda
    modules are on the local disk of all the nodes and so does not
    affect the shared filesystem.
- If you need to install additional Python packages, install with pip
    using the `--user` flag as described in [our
    documentation](../software-packages.md#python-packages). Python
    will then only go to your home directory for these installed
    packages, and so should be less load on the shared filesystem. Only
    use conda environments as a last resort, as this puts ALL packages
    you use in your home directory, and creates many small files.
- DO NOT install anaconda or miniconda in your home directory. There
    is no reason to do this and will slow your Supercloud experience
    down significantly, as these installations contain many, many small
    files. If you absolutely need to use conda to install a package
    create a conda environment using our anaconda modules. If you have
    previously installed anaconda or miniconda in your home directory,
    delete it now.

Submitting Jobs
---------------

- Use [Triples Mode](../submitting-jobs/submitting-jobs.md#triples-mode-triples) for
    submitting [Job
    Arrays](../submitting-jobs/job-array-triples.md) and
    LLMapReduce jobs. These create fewer log files and group them by
    node, reducing the number of files per directory. It is also lighter
    weight on the scheduler, as it creates fewer tasks/jobs that the
    scheduler has to keep track of.
- DO NOT create very large Job Arrays. Each task in a Job Array
    creates a log file, the more tasks in your array, the more files.
    The best practice is to use Triples to submit your job arrays (see
    bullet above).
- DO NOT submit many small jobs, most likely a Job Array or
    LLMapReduce with Triples would be appropriate.
- Avoid doing things that actively stress the filesystem, for example
    checking whether a file exists repeatedly over a long period of time
    or across many tasks.

File Organization
-----------------

- Aim for less than ~1000 files per directory.
- Fewer, larger files are better than many small files (file size
    should be a minimum of 1MB, target ~100MB).
- Within a job, you can use $TMPDIR for temporary or intermediate
    files you don't need after the job. $TMPDIR points to a temporary
    directory on the local filesystem that is set up at the start of
    your job and removed at the end of your job. If your job requires a
    lot of I/O you may see significant performance improvement by
    copying the files you need to $TMPDIR at the start of your job and
    copy any new files you need to your home directory at the end of
    your job.
- Use [shared directories](../files-and-data/shared-groups.md) to share data among team members rather than having a
    separate copy in everyone's home directory.
- Check /home/gridsan/groups/datasets before downloading large public
    datasets. If there is a public dataset that you are considering
    downloading that you think others may also want to use, send us a
    email to <supercloud@mit.edu> to suggest that we add it.
    - If you are using ImageNet, we have a special setup that will
    stress the filesystem significantly less and should be much
    faster. First load its modulefile:
    `module load /home/gridsan/groups/datasets/ImageNet/modulefile`.
    This will set up the `$IMAGENET_PATH` environment variable,
    which you can use in your code to point to ImageNet.
