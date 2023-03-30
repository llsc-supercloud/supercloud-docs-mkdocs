# Transferring Files

There are number of ways to access and transfer data and files between
your computer and the MIT Supercloud System depending on your OS
([Mac/Linux](transferring-files.md#maclinux-and-most-windows) or
[Windows](transferring-files.md#windows)) and your comfort
with Linux/Unix commands. Regardless of OS, files can be downloaded, but
not uploaded, through the
[web portal](transferring-files.md#downloading-files-through-the-web-portal) and
both uploaded and downloaded through the
[Jupyter](transferring-files.md#uploading-and-downloading-files-using-jupyter) web
interface. For Mac/Linux or Windows with Cygwin, you have the additional
options of using `scp` and `rsync` from a terminal window. If you are
using PuTTY on Windows, there is a way to transfer through that program
as well. All of these are described below.

## Mac/Linux and Most Windows

### Via scp

You can use `scp` (secure copy) to copy files to the MIT Supercloud. The
`scp` command expects a path to the file or directory name that you
would like to transfer followed by a space and then the destination.
Note, it is best to copy to or from your local machine. In addition, the
location on the system is the login node followed by a `:` (colon)
followed by the path to the directory where you want to put the file. If
you do not specify the directory location `scp` will place it in your
top level directory, which on the Supercloud system is your home
directory. An example of copying a directory to TX-E1 is shown below.
Note that the `scp` command is followed by `-r`, to recursively copy all
the files in the directory, and the directory name is followed by a
`/` so that `scp` copies all of the files in the directory.

> `myLocalMachine UserName > scp -r testCodes/ <username>@txe1-login.mit.edu:/home/gridsan/<username>/snippets`

If you want to copy data from TX-E1 to your computer, you just swap the
"from" and "to":

> `myLocalMachine UserName > scp -r <username>@txe1-login.mit.edu:/home/gridsan/<username>/results/`

### Via rsync

The `rsync` (remote synchronization) command is similar to the `scp`
command. However, after the first time that you copy files, executing the
`rsync` command will copy only the files that have changed since the
last rsync. This can often save time. Note that if you have the slash at
the end, it means "all contents in the directory". If not, it means
the directory itself and its contents. As shown in case A below, the
directory, `myDir`, and its all contents will be copied over to the
snippets directory on the remote host. However, in case B, only the
contents in the `myDir` directory will be copied to the snippets
directory on the remote host.

> CaseA:
> `rsync -rlug myDir <username>@txe1-login.mit.edu:/home/gridsan/<username>/snippets`
> CaseB:
> `rsync -rlug myDir/ <username>@txe1-login.mit.edu:/home/gridsan/<username>/snippets`

There are a number of options that can be used with rsync. Among the
most commonly used are:

- `r` to recursively copy files in all sub-directories
- `l` to copy and retain symbolic links.
- `u` is needed if you have modified files on the destination and you
    don't want the old file to overwrite over the newer version on the
    destination.
- `g` is used to preserve group attributes associated with files in a
    shared group.
- `h` human readable
- `v` verbose so that you get any error or warning information

Again, if you want to copy data from TX-E1 to your computer, you just
swap the "from" and "to":

> CaseA:
> `rsync -rlug <username>@txe1-login.mit.edu:/home/gridsan/<username>/results myDir`
> CaseB:
> `rsync -rlug <username>@txe1-login.mit.edu:/home/gridsan/<username>/results/ myDir`

## Windows


Depending on which ssh client you use you will transfer data
differently. If you use Bash on Windows 10, Mobaxterm, Cygwin, or other
Linux-like command line environment, you can use scp or rsync directly
and should follow the instructions above for
[Mac/Linux](transferring-files.md#maclinux-and-most-windows). If you use PuTTY,
you can follow the directions below to use pscp. You run these commands
from your own computer, not from the MIT SuperCloud login node (if you
have already logged in with ssh, you've gone too far).

IMPORTANT: MIT SuperCloud runs a Linux operating system. Two big
differences between Windows and Linux are the direction that the folder
separators face (Windows uses "\" and Linux uses "/") and case
sensitivity (Linux file and directory names are case sensitive, while
Windows file and folder names are not). Whether you are using scp,
rsync, or pscp you must use the Linux slashes "/" in your path and the
correct case in your filenames (Documents =/= documents). It will also
be much easier to transfer data if you avoid using spaces in your file
and directory/folder names.

HINT: Windows paths sometimes specify different "drives" (such as the
C: drive, where most local documents are kept). Different ssh clients
treat these drives differently. Using the example of a folder in the C
drive (say C:myFolder), the translations for each are:

- Bash for Windows 10: `/mnt/c/myFolder`
- Mobaxterm: `/drives/c/myFolder`
- Cygwin: `/cygdrive/C/myFolder`

When in doubt, you can navigate to the directory that you want to
transfer and issue the `pwd` command, which should give you the full
path to your current location.

### PuTTY

When transferring files from a Windows machine to the Supercloud system,
which are linux machines, you need to use a version of `scp`, or secure
copy. The software package, PuTTY, which you installed to provide `ssh`,
includes `scp`. To transfer a file between your Windows desktop and the
Supercloud linux system:

- Open a command window by typing `run` in the search box. The `scp`
    command must be run from a command window.
- Set the `PATH` variable so that your system sees the putty command:
  - To do it for this session only, type
        `set PATH=C:\Program Files\PuTTY` at the command prompt
  - To make this change more permanent, click on the start button,
        in the search box type Environment Variables:
    - Select Add/Modify Environment Variables for user
    - Click on Path and then click to edit Path.
    - Add `C:\Program File\PuTTY` (This assumes that you installed
            PuTTY in the default folder.)
    - Confirm that PuTTY is in your path by typing: `%ECHO PATH%`
            at the command line.
- Navigate to the directory (folder) that holds the file you want to
    transfer
- The `pscp` command has the format
    `pscp <Path_to_FileToBeTransfered> <Path_To_NewLocation>`
- For example to use `pscp` to copy the file `myMatlabScript.m` from
    my current directory (folder) to my home directory on the Supercloud
    system I would type

If I wanted to copy `myResults.dat` from my home directory on the
Supercloud system to a folder called `goodData` on my local machine I
would first change directories so that I was in goodDate, using:
`cd goodData`. Then, secure copy the file from SuperCloud to my
local system using:

> `pscp <myUserID>@txe1-login.mit.edu:/home/gridsan/<myUserID>/myResults.dat .`

Note the `.` at the end of the line - that is equivalent to "place
it here". Also note that in Windows you can use `\` or `/` but
Linux only understands `/`.

### Cygwin

Cygwin also supports the secure copy protocol. This works similarly to
scp in a Linux/Mac. When using Cygwin, you will be able to run `scp`,
however it can be tricky to determine to the path to your files. This is
of the form `/cygdrive/C/<your_File_Path>`.

Say you are using scp to copy the file myScript.sh from your current
directory (folder) to your home directory on the Supercloud system.
Type:

> `scp /cygdrive/C/<path_to_Folder>/mScript.sh <myUserID>@txe1-login.mit.edu:/home/gridsan/<myUserID>`

If you wanted to copy `myResults.txt` from your home directory on the
Supercloud system to a folder called `goodData` on your local machine,
first change directories so that you are in `goodData`, using:
`cd goodData.`

Secure copy the file from the Supercloud system to the local system
using:

> `scp <myUserID>@txe1-login.mit.edu:/home/gridsan/<myUserID>/myResults.txt  /cyqdrive/c/<path_to_goodData>/`

Note that in Windows you can use `\` or `/` but Linux only
understands `/`.

## Downloading Files through the Web Portal


The web portal can be accessed at:

> [https://txe1-portal.mit.edu/gridsan/USERNAME/](https://txe1-portal.mit.edu/gridsan/USERNAME/)

Where USERNAME is your username. When you navigate to the portal page,
you will be prompted for your username and password. Through the portal,
you can navigate through your directories and download files. You cannot
upload files this way.

## Uploading and Downloading Files Using Jupyter

You can both upload and download files using the Jupyter interface
through your browser. If you do not already have a Jupyter instance
running, [start one
up](../jupyter-notebooks.md). The Jupyter portal is
located at:

> [https://txe1-portal.mit.edu/jupyter/jupyter_notebook.php](https://txe1-portal.mit.edu/jupyter/jupyter_notebook.php)

Once you have an instance running, follow the "Open Notebook" link to
open Jupyter. You will see the files and directories in your home
directory. Here you can right click on a file and select "Save Link
As" to download a file. The "Upload" button is located in the top
right of the page, right next to the "New" button. You can select
multiple files to upload at a time.
