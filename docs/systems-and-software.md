Systems and Software {#systems_and_software}
====================

This page lists information about the system and available
[software](#software),
[languages](#available-languages),
[compilers](#compilers),
[modules](#modules), etc. This is
only a partial list, so if there is anything you are interested in that
isn't listed here, please [contact
us](https://supercloud.mit.edu/contact).

MGHPCC TX-E1 Specifications
---------------------------

  -------------------------------
  Summary               
  --------------------- ---------
  Number of Nodes       704

  Total CPU Cores       32000

  Total GPUs            448

  Distributed Storage   873 TB
  -------------------------------

  -------------------------------------------------------------------------------------
  Individual   s                                                               
  Node                                                                         
  ------------ ------- ------- ------- ---------- --------- ------ ----------- --------
  Processor    Nodes   CPU     Node    RAM/core   GPU Type  GPU    GPUs/node   Local
                       Cores   RAM                          RAM                Disk

  Intel Xeon   480     48      192 GB  4 GB       NA        NA     NA          4.4 TB
  Platinum                                                                     
  8260                                                                         

  Intel Xeon   224     40      384 GB  9 GB       NVidia    32 GB  2           3.8 TB
  Gold 6248                                       Volta                        
                                                  V100                         
  -------------------------------------------------------------------------------------

  ----------------------------------------------------------
  Resource Alloca tions                        
  --------------- --------------- ------------ -------------
  Processor       Partition       Starting     Standard

  Intel Xeon      xeon-p8         2 Nodes (96  16 Nodes (768
  Platinum 8260                   Cores)       Cores)

  Intel Xeon Gold xeon-g6-volta   1 Node (40   6 Nodes (240
  6248                            Cores 2      Cores 12
                                  GPUs)        GPUs)
  ----------------------------------------------------------

Available Languages
-------------------

- Julia
- Python (Anaconda available through modules)
- Matlab(R)/Octave
- R
- C/C++
- Fortran
- Java
- Perl 5
- Ruby

Modules
-------

To see the most up-to-date list of currently available modules, run the
command `module avail`. For more information about modules, see the
[module section](using-the-system/software-packages.md#modules) on the
Software and Package Management page.

Software
--------

### Machine Learning Tools

- Tensorflow
- Pytorch

### Big Data Software Stack

- Hadoop
- Zookeeper
- Accumulo

### Middleware Software Stack

- ARPACK
- ATLAS
- Boost
- BLAS
- FFTW
- LAPAC
- OpenMPI
- OpenBLAS

### Lincoln Laboratory Developed Software

- [pMatlab](https://www.ll.mit.edu/research-and-development/cyber-security-and-information-sciences/pmatlab)
- [D4M](http://www.mit.edu/~kepner/D4M/)
- [Graphulo](http://graphulo.mit.edu/)
- LLMapReduce

### Compilers

- gcc
- g++
- gfortran
- icc
