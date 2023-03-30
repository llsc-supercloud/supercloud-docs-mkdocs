# Shared Groups

## Overview

This page summarizes Shared Groups, including how to request to [join or
create a group](shared-groups.md#joining-or-creating-a-group), some
[best practices for working with
shared groups](shared-groups.md#using-shared-groups-effectively), and how to
[inspect and change file
permissions and group ownership](shared-groups.md#linux-file-permissions).

Supercloud users are welcome to either join an existing group and
receive the benefit of access to a shared file directory, or propose the
creation of a group if there is some common interest amongst certain
Supercloud account holders, perhaps they are members of the same lab.

You can look at the current list of groups by listing the groups
directory:

> `studentx@login-3:~$ ls /home/gridsan/groups/`

You can see what groups you are currently in by running the "groups"
command:

> `studentx@login-3:~$ groups`

## Joining or Creating a Group

If you would like to join a group, send an e-mail to
[supercloud@mit.edu](mailto:supercloud@mit.edu) with that request and CC the group owner for
approval. The group owner must give approval before we can add you to
the group. If you are not sure who the group owner/approver is, you can
send in your request and we will reach out to the approver.

If you would like to create a group, please email
[supercloud@mit.edu](mailto:supercloud@mit.edu?subject=New%20Group%20Request&body=I%20would%20like%20to%20request%20a%20shared%20group.%0A%0AGroup%20name%3A%20%0AGroup%20owner%2Fapprover%3A%0AGroup%20members%3A%0ADo%20you%20plan%20to%20store%20any%20non-public%20data%20in%20the%20group%3F%20Yes%20or%20no%3A%20%0AIf%20yes%2C%20please%20list%20any%20requirements%2C%20restrictions%2C%20or%20agreements%20associated%20with%20the%20data%3A)
with the following info.

- What should the group be called?
- Who should the group owner/approver be? We will ask this person for
    approval if anyone asks to be added.
- Who should be in the group, listing usernames is most helpful to us,
    but not required.
- Whether you plan to store any non-public data in the group. If so,
    please list any requirements, restrictions, or agreements associated
    with the data. The more information you give us, the better.

## Using Shared Groups Effectively

Once you have been added to a group you will be able to access that
group's shared directory. All group directories are located in the
`/home/gridsan/groups` directory on the filesystem. Since this is part
of the central filesystem along with your home directory, all nodes in
Supercloud can access the group directories. We will also add a symlink
in your home directory to your group shared directory, this symlink will
have the suffix "_shared" to indicate it is linking to a group
directory. If you are sharing code with other members of your team that
includes paths to a shared group, it is good practice to use a path that
does not include your home directory, otherwise your team members will
get a permission denied error when they try to run your code. Instead,
it is best to use the absolute path through `/home/gridsan/groups`.

All of our [Best Practices for using the
Filesystem](../best-practices/filesystem.md)
apply to the group directories. Additionally, NEVER use a GUI to drag
and drop files into a group directory. Doing so can alter the
permissions in the group directory, preventing others in your group from
accessing the files you've moved into the shared group directory. When
using `rsync` to transfer files into a group directory, be sure to use
the `-g` flag, which will also help keep the group ownership set
properly.

### Linux File Permissions

Sometimes, despite your best efforts, the permissions on a group can be
altered such that you or others in your group can't interact with a file
the way they need to. If that happens, you can always contact us at
<supercloud@mit.edu> and we can fix it. However, you may find it more
convenient to fix it on your own. Here is a brief introduction to Linux
File Permissions to help you learn what is going on and how to fix it.

#### Inspecting File Permissions

If you do a long form listing of the files in a directory using `ls -l`:

```bash
drwxrwx---    2 studentz studentz     4096 Jun 15 14:51  mydirectory
lrwxrwxrwx    1 root     root          26 Jun 15 17:24  files_shared -> ../groups/fileshare
-rw-------    1 studenty studenty    4096 Jun 30 09:02  logfile1
-rw-rw---     1 studentx Alpha       4096 Jun 30 09:02  logfile2
```

You will see the file permissions of your various directories, symlinks,
and files in the leftmost columns. The first column indicates whether
the file is a directory (d), symlink (l), or a regular file. Columns 2
through 10 can be viewed as triplets that define access permissions for
the file or folder. To explicitly define permissions you will need to
reference the Permission Group and Permission Types:

- The Permission Groups are:  u -- Owner   g -- Group  o -- Others  
- The Permission Types are:  r -- Read  w -- Write   x -- Execute

The first of these triplets represent the Owner's permissions, the
second the Group's, and the third Others'. An r,w, or x represent the
ability to perform that action, and a "-" means that action is not
permitted. For a file like `logfile1` above you can see that it is owned
by user `studenty` (from group `studenty`) and only the owner has read
and write permissions. The file named `logfile2` currently has the
permissions set to `-rw-rw----`, which means that the owner and group
have read and write permission. SuperCloud does not allow you to add
read, write, or execute permissions for others, or all users. One
important thing to note: in order to go into a directory you must have
execute permissions on that directory. So if you get a "Permission
denied" error when trying to enter or look at the files in a directory,
check whether the directory has read and execute permissions.

#### Changing File Permissions

Now say we want to change permissions for a file. One of the easiest
ways is to use the Assignment Operators, + (plus) and -- (minus). These
are used to tell the system whether to add or remove the specific
permissions.

For example, to add group read and write permission for `logfile1`, you
would invoke the command:

> `chmod g+rw logfile1`

Now say you want your group to be able to read `logfile2`, but don't
want anyone to accidentally modify it. To remove group write permissions
you would invoke the command:

> `chmod g-w logfile2`

It's very important to know that if you want to apply these changes
recursively that you use the `-R` (with a capital R) flag. Using a
lowercase `-r` flag like you do for other Linux commands like `cp` will
remove write permissions for everyone, including yourself. If you make
this mistake, it is not the end of the world, but you will need to send
us an email and have us fix it.

Alternately you can define the full permissions options with binary
references like `chmod 750 logfile1` which would grant full privileges
(7) to the owner, and rw privileges (5) to the group and nothing (0) to
others in a single command. You can learn more options and about chmod
either from an online tutorial or from your local man pages
(`man chmod`, typing `q` will exit) or with the quick cheat sheet you
can display with `chmod --help`. The following is also pretty good
tutorial, but be aware it talks about permissions in general, and not
everything will be relevant to shared groups or Supercloud: [How to use
the chmod Command on
Linux](https://www.howtogeek.com/437958/how-to-use-the-chmod-command-on-linux/).

#### Linux File Ownership

If we take another look at the example directory above:

```bash
drwxrwx---    2 studentz studentz     4096 Jun 15 14:51  mydirectory
lrwxrwxrwx    1 root     root          26 Jun 15 17:24  files_shared -> ../groups/fileshare
-rw-rw----    1 studenty studenty    4096 Jun 30 09:02  logfile1
-rw-r----     1 studentx Alpha       4096 Jun 30 09:02  logfile2
-rw-r-x---    1 studenty studenty    4096 Jun 30 09:02  myscript.py
```

the 12th and 13th column of the `ls -l` output is the owner of the file,
listed first, and the group for the file, listed second. For example,
`logfile2` is owned by `studentx` and its group is `Alpha`. Based on the
permissions above, `studentx` can read and write to the file, and anyone
in the `Alpha` group can read the file, but cannot write to it.

In a group directory the group owner for a file should usually be the
group associated with that directory. Sometimes it unintentionally gets
set to the username of the person who created or put the file there.
This can easily be remedied by using the `chgrp` command. For example,
let's say we'd like everyone in the `Alpha` group to be able to read and
run (execute) the file `myscript.py`, but not have write permissions.
The group permissions are set properly, but the group is set to
`studenty` instead of `Alpha`. To fix this, we can run:

> `chgrp Alpha myscript.py`

Again, if you would like to apply this change recursively, the flag is
`-R` (with a capital R).
