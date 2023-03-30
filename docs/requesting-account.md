Requesting an Account
=====================

The MIT SuperCloud is intended to support research and collaboration
between MIT Lincoln Laboratory and students, faculty and researchers at
MIT and other academic institutions. It is our practice to allow access
from within the United States.

Account Request Process
-----------------------

The account request, approval, and creation process is:

1. Request: There are two steps to the request:
    1. Fill out **all fields** of our [Account Request
        Form](https://txe1-portal.mit.edu/login/request.php). In this
        form we ask if you are using non-public data, see why
        [below](requesting-account.md#why-do-we-ask-if-you-are-using-data-that-is-not-publicly-available). If you
        are not part of an MGHPCC institution, list your MIT or Lincoln
        Laboratory collaborator. Do not submit this form without
        answering these question, it will cause significant delay in the
        process. If you have any questions about the form, ask us by
        sending email to <supercloud@mit.edu>.
    2. Ask your faculty advisor or PI to send us a short confirmation
        email for your account verifying that you will be using your
        Supercloud account for your work. This email should be sent to
        [supercloud@mit.edu](mailto:supercloud@mit.edu?subject=Confirming%20Supercloud%20Account). **We
        will not email your advisor for you. We will not proceed with
        the account creation process until we receive an email from your
        advisor/PI.**
2. Faculty Advisor/PI Confirmation: Once we receive an email from your
    faculty advisor/PI we can continue the next step. This confirmation
    must come from a faculty member or PI on the project that you are
    using Supercloud for.
3. Approval: This usually happens behind the scenes. You may receive an
    email with additional questions before you are approved. While you
    are waiting you can start learning to use your account by working
    through the [Practical
    HPC](https://learn.llx.edly.io/course/practical-hpc/) course.
4. Creation: When your account is created, you will receive an email
    with your username and further instructions to set up your account.
    When your account is first created you will have a small startup
    allocation. Once you complete steps 5 and 6 you can request your
    account be updated to the standard allocation.
5. Set up your account:
    [Create an ssh key](requesting-account.md#generating-ssh-keys) and
    [add it to
    your account](requesting-account.md#adding-your-ssh-keys-to-your-account), then
    make sure you can log in through ssh. The [Practical
    HPC](https://learn.llx.edly.io/course/practical-hpc/) course also
    has a section with videos that walks you through this process.
6. Learn to use your account: Work through the [Practical
    HPC](https://learn.llx.edly.io/course/practical-hpc/) course. This
    course:
    1. Includes an Introduction to HPC, canonical HPC Workflows, and
        the SuperCloud system.
    2. Walks you through setting up your account, installing software,
        running your first test job, submitting your first batch job.
    3. Describes how to scale up efficiently and measure your
        performance.

The account creation process is manual and can take approximately **two
weeks**. You can make this process smoother by making sure you have
fully filled out your request form before submitting it and making sure
your advisor has sent us an email confirmation. While you are waiting
for your account you can get a head start learning how to use the
account by reviewing the [Practical
HPC](https://learn.llx.edly.io/course/practical-hpc/) course.

Why do we ask if you are using data that is not publicly available?
-------------------------------------------------------------------

Not all data is appropriate for Supercloud. If your data is not publicly
available, we ask for any agreements or requirements you have for your
data to make sure Supercloud is the right place to be putting the data.
Please be as detailed as you can. To get a general idea of the sorts of
data that may or may not be appropriate for Supercloud, take a look at
[MIT IS&T's guidance for storing data in Dropbox, OneDrive, and Google
Drive
here](http://kb.mit.edu/confluence/pages/viewpage.action?pageId=152598887).

Generating ssh Keys
-------------------

If you have any issues or questions regarding the generation of ssh
keys, please contact the team at <supercloud@mit.edu>. To access the
system you will need ssh keys. For additional security you can create a
passphrase when you generate your key, which you must enter every time
you log in. Since you set this yourself on your own computer, we cannot
help you reset it if you forget it. If you can't remember your
passphrase you'll have to generate a new key and re-add it using the
Web Portal.

If you cannot generate ssh keys on your system, let us know and we can
help you.

If you have no existing ssh keys, from the command line in a terminal
window, follow the steps below. On Mac and Linux, open your standard
terminal window. On Windows 10 and higher, you can use the Windows
command prompt. If the command prompt does not recognize the ssh-keygen
command, you can install OpenSSH by [following the instructions on this
page](https://linoxide.com/how-use-ssh-commands-windows-10-command-prompt/).
If your Windows operating system is older than Windows 10, see the note
[below](#pre-windows-10).

If you already have ssh keys then you can use those. You will need your
public key, `id_rsa.pub.`

`[user1234@yourMachine]$ ssh-keygen -t rsa`

You will see the following:

`Generating public/private rsa key pair.`

When answering the 3 prompts (first 3 lines) hit return to create
passwordless keys and save them in the default location. Alternatively,
for extra security you can create a passphrase for your key that you'll
have to enter every time you log in. To do this, instead of pressing
"enter" or "return", enter the passphrase you've chosen when
prompted.

```
Enter file in which to save the key (/home/user1234/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/user1234/.ssh/id_rsa.
Your public key has been saved in /home/user1234/.ssh/id_rsa.pub.
The key fingerprint is:
88:90:6a:dc:f1:bd:ed:fb:b1:aa:46:14:34:5e:b9:70 user1234@yourMachine
The key's randomart image is:
+--[ RSA 2048]----+
|      .o ..      |
|   .  .ooE       |
|  o.   .+ .      |
|....o..o .       |
|.o ...o.S        |
|.      .o        |
|      .. . .     |
|       ..   o    |
|      ...++o     |
+—————------------+`
```

To view your public ssh key, go to your .ssh directory.

`[user1234@yourMachine]$ cd .ssh`

In `~/.ssh` you would see two files `id_rsa` and `id_rsa.pub`. The
`id_rsa.pub` file contains your public key.

`[user1234@yourMachine]$ ls   id_rsa  id_rsa.pub`

This is the `id_rsa.pub` file content after generating a public SSH
key that we would require. To view it, type `cat id_rsa.pub` at the
command line.

```bash
[user1234@yourMachine]$ cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEA1NAD8v4nFzQ6G7KIEzkDLOnlH7t/4zmw0vVXlJjjFW4kLBgLJa0tkk61jHCxO2CurDr4zdEs2NeHG9agZJgMKMJZdIVaxtPcEBVVaNutvn/ZDRe3VsrRjToKEoR0xlAUdoef++AwiwI6K6vBOGIq6whLIlY5L9tZJfaLF3xMwmQRRhf4C+al/yZ5hX7BfGba2fqZmugTPpeSbLnFMVPKK/wy6XZasBSAKgLBA141EMXIKuGrpXpxLMECPBN5GDd/xmjmD0pC2o2z5OdfdYJj/FRWL2sC8hWTZSPa4p/n7Qc9ErFW5wM7FkynwguN4t/A+QOCa+p8C/nrOcTQKugrtw user1234@yourMachine
```

Copy the entire output, including the ssh-rsa at the beginning.

Adding your SSH Keys to your Account
------------------------------------

Once you have created your ssh keys and copied your public key, you can
add your key to your account using the Web Portal:

1. Go to <https://txe1-portal.mit.edu>.
2. Log in. If you are an MIT affiliate or an affiliate at another
    university or institution you can log in with your MIT or
    institutional credentials. Click on MIT Touchstone/InCommon.
    1. Select your institution (note these are spelled out, MIT is
        listed as Massachusetts Institute of Technology, for example).
    2. Click the checkbox next to "Remember my Choice" and click the
        "Select" button.
    3. Log in with your institutional credentials.
3. Click on the "sshkeys" link.
4. Paste your public ssh key in the box at the bottom of the page,
    click "Update Keys".
5. Verify you can log in by running `ssh USERNAME@txe1-login.mit.edu`
    in the terminal where you created your ssh keys, where `USERNAME` is
    the username we sent you in your new account email.

## Pre-Windows 10

**NOTE:** For other Windows users there are a number of ssh clients you
can use. Some ssh clients like Moba Xterm and Cygwin give you a
Linux-like environment, and so once you start the program (which should
look include a command line window), you can follow the instructions for
creating an ssh key in [above](#generating-ssh-keys)
once you install the client.

To install Moba Xterm, follow the instructions
[here](https://www.poftut.com/mobaxterm-installation-and-usage-for-windows-personal-edition/)
through the section "Create Local Shell". Anytime you are instructed
to open a terminal window, you can follow the instructions to create a
local shell. Once you have installed, follow the instructions
[above](#generating-ssh-keys) for creating an ssh
key.

Instructions for installing PuTTY
are [here](http://www.chiark.greenend.org.uk/~sgtatham/putty/). (Please
note, the link will open in a new window.) Once PuTTY is installed
please follow the instructions at
this [link](https://docs.joyent.com/public-cloud/getting-started/ssh-keys/generating-an-ssh-key-manually/manually-generating-your-ssh-key-in-windows) to
manually generate your ssh-keys, only follow the instructions in the
"Generating an SSH Key" section.

Current Approver List
---------------------

- Boston University: Wayne Gilmore
- Harvard: Scott Yockel
- MIT: Jeremy Kepner, Vijay Gadepally, Chris Hill, Lauren Milechin
- Lincoln Laboratory: Jeremy Kepner, Vijay Gadepally, Albert Reuther
- Northeastern: David Kaeli
- UMass Amherst: John Griffin
- UMass Dartmouth: Geoffrey Cowles
- UMass Lowell: Anne Maglia
- UMass Medical: Paul Langlois
- University of Rhode Island: Gaurav Khanna
