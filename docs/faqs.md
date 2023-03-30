Frequently Asked Questions
==========================

This page contains the answers to a few questions that we receive often.
As more questions are asked, this page may be updated.

How do I get an account?
------------------------

To request an account, follow the instructions and answer the questions
on our [Account Request page](requesting-account.md). We will reach
out to you once your account is created, or if we have any questions for
you.

I would like to log in from a new computer. Can I add a new ssh key?
--------------------------------------------------------------------

If you have a new computer, or want to add keys for additional computers
that you use, you can add your own key on our web portal. Instructions
on how to generate a new ssh key and add it to your account are on our
[Account Request page](requesting-account.md#generating-ssh-keys). In
summary, log in with your credentials (for MIT and other educational
institutions this is the middle option when you go to
<https://txe1-portal.mit.edu>) and then click on the "sshkeys" link.
Scroll to the bottom and paste your key in the box.

How much storage do I have for my account?
------------------------------------------

We have set some guardrails on home and group directory storage. Home directories are set to 10TB and group directories are set to 50TB. It is recommended that users
not use their accounts as primary storage. **Further, we do not back up
the storage on the system, so we strongly recommend transferring your
code, data, and any other important files to another machine for
backup.**

How can I share files/code/data with my colleagues?
---------------------------------------------------

If you would like to share files with others you can request a shared
group directory. Shared group directories are located at
/home/gridsan/groups and we will put a symlink in your home directory to
use as a shortcut to your shared group directory. To request one, send
email to
[supercloud\@mit.edu](mailto:supercloud@mit.edu?subject=New%20Group%20Request&body=Hello%2C%0A%0AI%20would%20like%20to%20request%20a%20new%20group.%20Here%20is%20the%20required%20information%3A%0A%0A1.%20Group%20name%3A%20%0A2.%20Group%20owner%2Fapprover%3A%0A3.%20Group%20members%3A%0A4.%20Any%20non-public%20data%3F%20If%20so%2C%20what%20requirements%2C%20restrictions%2C%20or%20agreements%20are%20associated%20with%20the%20data%3F%0A%0AThank%20you%2C%0A%0A)
and let us know:

1. What the group should be called. Short, descriptive names are best.
2. Who should be the owner/approver for the group. We will ask this
    person for approval whenever we receive a request to join a group.
3. Who should be in the group. Supercloud usernames are helpful, but
    not required.
4. Whether you plan to store any non-public data in the group. If you
    do, let us know what requirements, restrictions, or agreements are
    associated with the data. [See why we ask
    here](requesting-account.md#why-do-we-ask-if-you-are-using-data-that-is-not-publicly-available).

To learn more about Shared Groups and best practices using them, see the
page on [Shared Groups](using-the-system/files-and-data/shared-groups.md).

How do I set/change my password?
--------------------------------

We do not use passwords on SuperCloud. If you have an active MIT
Kerberos or login from another University, you can log in
using your institution's credentials. On the Supercloud Web Portal
Login page, select the middle option "MIT Touchstone/InCommon
Federation". You may have to select your institution from the dropdown
list, which should take you to your institution's login page. After you
log in, you should see the Portal main page. If you have trouble logging
in this way, please [contact us](https://supercloud.mit.edu/contact) and
we can help.

If you cannot log in using "MIT Touchstone/InCommon Federation" or one of the other options on the portal, we
may set you up with a password. If you have not yet reset your password,
or remember your previous password, then follow the instructions on the
[Web Porta](using-the-system/web-portal.md) page. If you have previously
set your password and cannot remember it, [contact
us](https://supercloud.mit.edu/contact) and we will help you reset your
password.

Are there any resource limits?
------------------------------

New accounts are created with a small starting resource allocation. Once
you have completed the [Practical HPC
course](https://learn.llx.edly.io/course/practical-hpc/) you can send your certificate
 to <supercloud@mit.edu> to request to be moved to the standard
allocation. The starting and standard allocations are listed on the
[Systems and Software](systems-and-software.md) page.

If you have a deadline and need additional resources you can request
more by [contacting us](https://supercloud.mit.edu/contact). If you
looking to request more GPUs, please read through this page on
[Optimizing your GPU Jobs](using-the-system/best-practices/gpu-jobs.md) first. Please state the number
of additional processors you need, the length of time for which you need
it, and tell us about the jobs you are running and how you are
submitting them. If you plan to run many independent jobs we will ask
you to convert your job to use [triples](using-the-system/submitting-jobs/submitting-jobs.md#triples-mode)
before giving and increased allocation. Remember this is a shared
system, so during busy times we may not be able to grant your request.
We will also only grant increase requests if you have completed the
[Practical HPC course](https://learn.llx.edly.io/course/practical-hpc/).

It is also important to keep in mind what your fair share of memory is
for each process and [request additional
resources](using-the-system/submitting-jobs/submitting-jobs.md#adding-more-memory-or-cores-per-task) if needed. For
example, if there are 40 cores and 384GB of RAM on the machine you are
using, each processor's fair share would be about 9GB. Check the
[Systems and Software](systems-and-software.md) page to see how
many cores and how much memory each node type has. If you think your
processes will go over this, request additional slots as needed. This
ensures you have sufficient memory without killing your job.

What do I do if my job won't be deleted?
-----------------------------------------

Occasionally this will happen if the node where your job is running goes
down, or your job does not exit gracefully. If this happens, [contact
us](https://supercloud.mit.edu/contact) with the Job ID, and we'll
delete the job and reboot the node if needed.

Why do I get an error when I try to install a package?
------------------------------------------------------

There are two common reasons you get an error when you try to install a
package. If you get a "Permission Denied" or similar error, it is
because you are trying to install the package system-wide, rather than
your own home directory. See the
[Software and Package Management](using-the-system/software-packages.md) page for more
information on how to install packages.

If you get a "Network Error", or similar, this is because we don't
have internet/network connection on the compute nodes, this includes
Jupyter and any interactive jobs. You will have to install the package
on one of the login nodes.

If you get an error like
`Could not install packages due to an EnvironmentError: [Errno 122] Disk quota exceeded`
when installing a package with pip or something like
`ERROR: could not download https://pkg.julialang.org/registry/...`
installing a package with Julia, even though you are on the login node,
this is because it is filling up your quota in the `/tmp` directory. We
have set quotas on this directory to prevent a single person from
inadvertently filling it up, as when this happens it can cause issues
for everyone using the node, including preventing anyone from installing
packages. This can be fixed by setting the `TMPIDR` environment variable
like so:

```bash
mkdir /state/partition1/user/$USER
export TMPDIR=/state/partition1/user/$USER
```

After you have installed your package you can clean up any lingering
files by removing the temporary directory you have created:

> `rm -rf /state/partition1/user/$USER`

How can I set up VSCode to edit files remotely on Supercloud?
-------------------------------------------------------------

You can use VSCode to remotely connect to Supercloud via the Remote-SSH
extension. The default settings in the VSCode Remote - SSH extension
will fail to connect. This is due to it trying to lock files in your
home directory, which is disabled for performance reasons.

The solution is to have it use the local filesystem. To get it to work,
go to your VS Code settings, click "Extensions" and then "Remote - SSH".
Once you're in the settings for Remote - SSH, check the box next to
"Remote.SSH: Lockfiles in Tmp". What this will do is put any lockfiles
in /tmp, rather than your home directory.

A side note: we have seen VS Code clutter up /tmp in the past, which we
keep fairly small. Disconnecting occasionally should clean these up,
however we do not know for sure. If you can check it once in a while and
clean up any files that are yours in /tmp, that would be really helpful.

How can I use Tensorboard on Supercloud?
----------------------------------------

Take a look at this page on [how to run Tensorboard in an interactive
job](tensorboard.md).

I got an Out of Memory error. How can I figure out how much memory my job needs and request more?
-------------------------------------------------------------------------------------------------

This is described on the Submitting Jobs page. If you submit your jobs
with `sbatch`, check out [this
section](using-the-system/submitting-jobs/submitting-jobs.md#adding-more-memory-or-cores-per-task), and if you use
`LLsub` take a look at [this
section](using-the-system/submitting-jobs/submitting-jobs.md#additional-cores-on-the-same-node-with-llsub). As described in
those links, you can check how much memory your job used using the
`sacct` command, then request enough additional cores for the memory you
need. Keep in mind that if your job was killed due to high memory use,
your job may not have gotten to the point of highest memory use. To get
an accurate measurement you can run your job on an exclusive node long
enough to reach the part of the job that would consume the most memory,
then stop the job and check the memory use with `sacct`.

My Python/Julia job is running, but I don't see any output in the log files. What is going on?
-----------------------------------------------------------------------------------------------

Julia and Python will buffer output in batch jobs. This means they will
hold on to the output and print it out all at once, sometimes this
isn't until the end of a loop or the end of the program. You can force
both to print the output when it is produced. In Python you can do this
by using the `-u` flag when you call Python in your submission script
(ex: `python -u myscript.py`). In Julia you can do this by adding
`flush(stdout)` after the print statements in your Julia script that
you'd like to print immediately (ex:
`println("Hello World!"); flush(stdout)`).

What does the Underutilizing/Oversubscribing the node warning message mean?
---------------------------------------------------------------------------

When you launch a job in [triples mode](using-the-system/submitting-jobs/submitting-jobs.md#triples-mode), the second and third
numbers you provide are the number of processes per node (NPPN) and the
number of threads per process (NT). You can multiply these two numbers
to get the total number of threads you will have running on the node.
You will usually get the best performance if you have the same total
number of threads as cores on the node. See the
[Systems and Software](systems-and-software.md) page for a list of
how many cores are on each node type.

**Oversubscribing:** When you have more threads than the number of cores
on the node. Oversubscribing can overwhelm the node, which can slow your
job down or even cause it to fail. It can also be harmful for the node.
Reduce the number of threads per process or number of processes per node
so that the total number of threads is less than or equal to the number
of cores.

**Underutilizing:** When you have fewer threads than the number of cores
on the node you may be undersubscribing. This means you may not be
taking full advantage of the node. Many underlying packages and
libraries can take advantage of multithreading. You might try increasing
the number of threads per process, while avoiding oversubscribing, to
see if you get improved performance.

For more information on how to pick the best triple for your job, take a
look at the [tuning](using-the-system/submitting-jobs/submitting-jobs.md#triples-mode-tuning) process and
recommendations.

How can I get more help?
------------------------

If you have a question that is not answered here, send email to
<supercloud@mit.edu> for more help.
