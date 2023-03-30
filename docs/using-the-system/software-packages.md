Software and Package Management
===============================

The standard environment on the MIT Supercloud System is sufficient for
most. If it is not, first check to see if the tool you need is included
in a module. [Modules](#modules) contain
environment variables that set you up to use other software, packages,
or compilers not included in the standard stack. If there is no module
with what you need, you can often
[install your package or software in your home
directory](#installing-software-or-packages-in-your-home-directory). Below we have
instructions on how to install [Julia](#julia-packages), [Python](#python-packages), and
[R](#r-libraries) packages. If you have explored
both these options or are having trouble, [contact
us](https://supercloud.mit.edu/contact).

Modules
-------

Modules are a handy way to set up environment variables for particular
work, especially in a shared environment. They provide an easy way to
load a particular version of a language or compiler.

To see what modules are available, type the command:

> `module avail`

To load a module, use the command:

> `module load moduleName`

Where `moduleName` can be any of the modules listed by the module avail
command.

If you want to list the modules you currently have loaded, you can use
the module list command:

> `module list`

If you want to change to a different version of the module you have
loaded, you can switch the module you have loaded. This is important to
do when loading a different version of a module you already have loaded,
as environment variables from one version could interfere with those of
another. To switch modules:

> `module switch oldModuleName newModuleName`

Where `oldModuleName` is the name of the module you currently have
loaded, and `newModuleName` is the new module that you would like to
load.

If you would like to unload the module, or remove the changes the module
has made to your environment, use the following command:

> `module unload moduleName`

Finally, in order to use the module command inside a script, you will
need to initialize it first.

The following shows a bash shell script example:

```bash
#!/bin/bash

# Initialize the module command first
source /etc/profile

# Then use the module command to load the module needed for your work
module load anaconda/2023a
```

Installing Software or Packages in your Home Directory
------------------------------------------------------

Many packages and software can be installed in user space, meaning they
are installed just for the user installing the package or software. The
way to do this for Julia, Python, and R packages is described below. For
other software, look at their installation documentation and see if they
have instructions on how to install in your home directory. Sometimes
this is described changing the installation location. Often you will
have to download the source and build the software in your home
directory to do this. Any dependencies can usually be installed in a
similar way. If you run into trouble installing software you can [reach out to us for help](mailto:supercolud@mit.edu?subject=Help%20Installing%20Software).
Let us know what you have tried so far and we can often point you in the
right direction.

### Julia Packages

Adding new packages in Julia doesn't require doing anything special. On
the login node, load a julia module and start Julia. You can enter
package mode by pressing the `]` key and entering `add packagename`,
where `packagename` is the name of your package. Or you can load `Pkg`
and run `Pkg.add("packagename")`.

The easiest way to check if a package already exists is to try to load
it by running `using packagename`. The `Pkg.status()` command will only
show packages that you have added to your home environment. If you would
like a list of the packages we have installed, the following lines
should do the trick (where v1.# is your version number, for example
v1.3):

```julia
using Pkg
Pkg.activate(DEPOT_PATH[2]*"/environments/v1.3")
installed_pkgs = Pkg.installed()
Pkg.activate(DEPOT_PATH[1]*"/environments/v1.3")
installed_pkgs
```

If you get an error trying to install a Julia package, first check to
make sure you are on the login node, as the compute nodes don't have
internet access. If you are already on the login node, it is possible
that the installation is filling up the `/tmp` directory. The errors for
this can be vague and differ between the different Julia versions. You
can try changing the temporary directory that Julia uses to download
its packages for installation by setting the `$TMPDIR` environment
variable. You can create the new temporary directory and set the
environment variable like this:

```bash
mkdir /state/partition1/user/$USER
export TMPDIR=/state/partition1/user/$USER
```

Once you have done this you can start up Julia and install packages as
you normally would. Once you are done it is good practice to delete
these temporary files.

**Note:** If you are using Jupyter there is an additional step you can
optionally do so that Jupyter can find both our installed packages and
your own. You can also run this if you are missing a Julia Kernel. First
load a Julia module. Then, in a Julia shell, run:

```julia
using IJulia
installkernel("Julia MyKernel", env=Dict("JULIA_LOAD_PATH"=>ENV["JULIA_LOAD_PATH"]))
```

The first part `Julia MyKernel` is what you want to call your kernel,
so feel free to change this. The second part makes sure both our
packages and any you've installed in your home directory show up on the
load path when you use a Jupyter Notebook with this kernel.

### Python Packages

Many python packages are included in the Anaconda distribution. The
quickest way to check if the package you want is in our module is to
load the anaconda module, start python, and try to import the package
you are interested in. Importing the packages in our Anaconda modules
will also be much faster than importing packages that are installed in
your home directory. This is because the packages in our Anaconda
modules are installed on the local disk of every node, which is faster
to access than packages installed in your home directory.

If the package you are looking to install is not included in Anaconda,
then you can install it in user space in your home directory- this
allows you to install the package for you to use without affecting other
users. We recommend that you use pip to do this, as pip allows you to
only install the additional packages that you need in your home
directory. Conda environments will result in installing all packages in
your home directory, which can slow down the import process quite a bit.

#### Installing Packages in your Home Directory with pip

First, load the Anaconda module that you want to use if you haven't
already:

> `module load anaconda/2023a`

Here we are loading the 2021a module, the newer modules will have newer
packages. Then, install the package with pip as you normally would, but
with the `--user` flag:

> `pip install --user packageName`

Where `packageName` is the name of the package that you are installing.

If you get an error trying to install a package with pip, first check to
make sure you are on the login node, as the compute nodes don't have
internet access. If you are already on the login node, it is possible
that the installation is filling up the `/tmp` directory, you may get a
"Disk quota exceeded" error. You can change the temporary directory
that pip uses to download its packages for installation by setting the
`$TMPDIR` environment variable. You can create the new temporary
directory, set the environment variable, and install your package like
this:

```bash
mkdir /state/partition1/user/$USER
export TMPDIR=/state/partition1/user/$USER
```

Once you are done it is good practice to delete these temporary files.

#### Installing Packages with Conda

As mentioned above, if at all possible we recommend you install packages
in your home directory with pip rather than create a conda environment,
as it'll be much faster. However, if you need to use a conda
environment (usually this is because a package isn't available through
pip or to help manage complex dependencies), you can do so by loading
our anaconda module (this will give you access to the "conda" command)
and then creating an environment the same way you would anywhere else.
For example:

```bash
module load anaconda/2023a
conda create -n my_env python=3.8 pkg1 pkg2 pkg3
```

In this example I am loading the `anaconda/2023a` module, then creating
a conda environment named `my_env` with Python 3.8 and installing
packages pkg1, pkg2, pkg3. We have found that conda creates more robust
environments when you include all the packages you need when you create
the environment. Then, whenever you want to activate the environment,
first load the anaconda module, then activate with
`source activate my_env`. Using `source activate` instead of
`conda activate` allows you to use your conda environment at the command
line and in submission scripts without additional steps.

If you would like to use your conda environment in Jupyter, simply
install the "jupyter" package into your environment. Once you have
done that, you should see your conda environment listed in the available
kernels.

**Note:** If you are using a conda environment and would like to install
the package with pip in that environment rather than in the standard
home directory location, you should *not* include the `--user` flag.
Further, if you are using a conda environment and want Python to use
packages in your environment first, you can run the following two
command:

> `export PYTHONNOUSERSITE=True`

This will make sure your conda environment packages will be chosen
before those that may be installed in your home directory. If you are
using Jupyter, you will need to add this line to
the `.jupyter/llsc_notebook_bashrc` file. See the section on the bottom
of the [Jupyter](jupyter-notebooks.md) page for more
information.

### R Libraries

There are two different ways we recommend that you use R. First, is
using a preset R environment that comes with the anaconda module, second
would be to create your own R conda environment. This first way works
best if you don't need to install any additional packages than what we
already have.

To use our R conda environment, log in and load an anaconda module. Then
you can activate the R environment with `source activate`. You can see
what packages are installed with the `conda list` command. Any packages
that start with `r-` are R libraries.

```bash
module load anaconda/2023a
source activate R
conda list
```

Then you can use R as you did before.

If you need to install additional packages, it's best to do it in a new
conda environment.

First thing to know is that many R packages are available through conda,
and some are not. What you want to do is include as many R libraries
that you'll need as you can when you create your conda environment- this
helps avoid version conflicts. Conda r libraries are all prefixed with
`r-`, so for example if you need rjava, you'd search for `r-rjava`. You
can check if conda has a library with the command:
`conda search r-LIBNAME`, where `LIBNAME` is the name of the library
you're looking for. You'll see a lot of versions, but as long as you see
something you should be good to add it.

Once you have a list of all the libraries available through conda,
create your conda environment (I'm calling the environment myR, feel
free to change that):

> `conda create -n myR -c conda-forge r-essentials r-LIB1 r-LIB2…`

Where LIB1, LIB2, etc are the additional R libraries you'd like to
include. Sometimes this step takes a while. It'll tell you which new
packages are going to be installed, and then you can confirm by typing
`y`.

If you have any other libraries that weren't available through conda,
install them now. First activate your new environment and then start R:

```bash
source activate myR
R
```

Then you can install your remaining libraries. You can do some test
loads here as well, to make sure the libraries installed properly.

```r
install.packages(“PKGNAME”)
library(PKGNAME)
```

In Jupyter, you should see your environment show up as a kernel. For a
batch job, you'll have to activate the environment either in your
submission script or before you submit the job.
