Jupyter Notebooks {#jupyter}
=================

Jupyter notebooks are an interactive IDE environment. The environment
allows you to start up notebooks in a variety of languages (Python,
Matlab, Octave, Julia), terminal windows, and a simple text editor. You
can also download and upload files to your home directory through this
interface. This page will describe how to
`start up <#starting-jupyter>`{.interpreted-text role="ref"} and
`use <#about-jupyter>`{.interpreted-text role="ref"} your own Jupyter
instance and how to set
`environment variables for Jupyter <#jupyter-env>`{.interpreted-text
role="ref"}.

Starting Up Jupyter {##starting-jupyter}
-------------------

When you start up Jupyter, the Jupyter instance gets submitted through
the scheduler as a job. To do this, navigate to the following page:

<https://txe1-portal.mit.edu/jupyter/jupyter_notebook.php>

To launch a notebook with default options, you can simply click on the
\"Launch Notebook\" button.

To see what the default options are or to change these options, click
\"Show Advanced Launch Options\". A form will appear where you can
select alternative options.

-   Partitions: The partition the job is submitted to. You do not have
    to change this option, it will adjust based on the resources you
    select.
-   CPU Type: The type of CPU you would like to launch to. For Jupyter
    it doesn\'t make a big difference which you choose, however
    occasionally one CPU type has more resources available than another
    (you can use the LLfree command in a terminal to check).
-   GPU Resource Flag: If you would like to allocate GPUs to your
    Jupyter instance you can select the GPU type here. Note your code or
    packages must take advantage of GPUs, otherwise they will sit idle
    if you request them.
-   GPU Resource Count: If you select a GPU the number of GPUs to
    allocate to your Jupyter instance.
-   ncpu: The number of CPUs or cores allocated to your notebook. The
    number of CPUs your request is limited to the number of CPUs on the
    CPU type you have selected.
-   exclusive: Check this box if you would like exclusive use of a
    compute node. You will be allocated all the CPUs on the node.
-   Time limit: Select how long you would like your notebook to run for.
-   Anaconda/Python Version: Select the Anaconda and/or Python version
    that you want. Note some languages are not available on all Anaconda
    versions. Refer to our How To pages to see which versions you should
    select for the language you want to use.
-   Application: Choose between Jupyter Notebook and Jupyter Lab. They
    both have the same features but with different layouts. Jupyter
    Notebook is the stable production application, Jupyter Lab is a beta
    application. The application you choose is personal preference. In
    this course we will be showing examples using Jupyter Notebook.

Once you click on the \"Launch\" button, the scheduler launches your
job, if the resources are available. When your Jupyter instance is
ready, a link will appear on the page. This can take a minute or so.

One very important thing to note is that once your Jupyter instance has
launched, it will continue to run until you stop the job. This is
particularly important if you are using a lot of resources when
launching your job. Stopping the job can be done by clicking on the
\"Shutdown\" button in the top right corner of the Jupyter interface
page, by navigating back to the inital launch page and clicking the
\"Shutdown\" button, or by using one of the scheduler commands (LLkill).

The Jupyter Environment {##about-jupyter}
-----------------------

When you first enter the Jupyter environment, you will see the files and
directories in your home directory. You can navigate through these by
clicking. By clicking the \"New\" button in the top right, you can:

-   Start a new notebook (Julia, Python, Matlab, Octave, R)
-   Open a terminal
-   Create a new text file with a simple text editor

In addition to these, you can upload and download data, and edit text
files from Jupyter. Click on the checkbox next to the file you want to
edit, and go to the top of the page and click \"Edit\". This will bring
you to a simple text editor. For a bit of an intro to the Jupyter
interface, see this page on [Notebook
Basics](http://jupyter-notebook.readthedocs.io/en/stable/examples/Notebook/Notebook%20Basics.html).

A Note on Environment Variables and Modules {##jupyter-env}
-------------------------------------------

Environment variables you have defined may not be set in the Jupyter
environment, and modules you need may not be loaded. You can add these
to the file `~/.jupyter/llsc_notebook_bashrc`, then shutdown and restart
your Jupyter instance on the Jupyter portal
(<https://txe1-portal.mit.edu/jupyter/jupyter_notebook.php>). You can
check that your environment is set the way you need it by opening a
terminal and running the `env` command or by running the `module list`
command to check which moduels are loaded. Note that the Anaconda module
is automatically loaded when starting up Jupyter, so you do not need to
load it, and loading multiple Ananconda modules may cause unexpected
behavior.
