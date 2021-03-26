candi (Compile &amp; Install)
=====

The ``candi.sh`` shell script downloads, configures, builds, and installs
[deal.II](https://github.com/dealii/dealii) with common dependencies on
linux-based systems.

Important Notice
----

This forked version of deal.ii/candi is **for internal use only**. It is a simple
way to install deal.ii and its dependencies for parallel computation. 
The branch "dealii-8.5" has been updated according to the change of repository in
Hypre and PETSc 3.7.6.

To download and execute the script, run in the terminal

```bash
 git clone --single-branch --branch dealii-8.5 https://github.com/javieralmonacid/candi.git
 cd candi
 ./candi.sh --PROCS=<nprocs> --prefix=/path/to/dealii
```
where ``<nprocs>`` corresponds to the number of threads (cores) available in your machine.
When executing ./candi.sh, pay close attention to the first screen: it indicates **all**
the dependencies needed to install deal.ii and its parallel framework. These must
be fulfilled before installing deal.ii.

To avoid problems when updgrading or downgrading deal.ii, do not execute the script
using administrator (i.e. ``sudo``) privileges.


Quickstart
----

The following commands download the current version of the installer and
then install the latest deal.II release and common dependencies:

```bash
 git clone https://github.com/dealii/candi
 cd candi
 ./candi.sh
```

Follow the instructions on the screen
(you can abort the process by pressing < CTRL > + C)

### Examples

#### Install deal.II on RHEL 7, CentOS 7 or Fedora 24,25,26:
```bash
  module load mpi/openmpi-`uname -i`
  ./candi.sh
```

#### Install deal.II on ubuntu 12.04, 14.xx, 15.xx, 16.xx, 17.xx:
```bash
  ./candi.sh
```

#### Install deal.II on macOS (10.11), 10.12, 10.13:
```bash
   ./candi.sh
```

#### Install deal.II on Windows 10 (1709):
Since the Creators Update in fall 2017 (Windows 10 (1709)) the
Windows Subsystem for Linux (WSL) is an official part.

Install ubuntu from the Store.
Then enable the WSL feature,
e.g. by opening a PowerShell as Administrator and run:
```bash
   Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```
You need to reboot your system afterwards.

Within the ubuntu terminal application clone this repository and run candi

```bash
   sudo apt-get update
   sudo apt-get upgrade
   git clone https://github.com/dealii/candi
   cd candi
   ./candi.sh
```

#### Install deal.II on a generic Linux system or cluster:
```bash
  ./candi.sh --platform=./deal.II-toolchain/platforms/supported/linux_cluster.platform
```

Note that you probably also want to change the prefix path, or 
the path to ``BLAS`` and ``LAPACK`` in the configuration file
(see documentation below).

#### Install deal.II on a system without pre-installed git:

```bash
 wget https://github.com/dealii/candi/archive/master.tar.gz
 tar -xzf master.tar.gz
 cd candi-master
 ./candi.sh
```

Note that in this case you will need to activate the installation of git by
uncommenting the line `#PACKAGES="${PACKAGES} once:git"` in
[candi.cfg](candi.cfg).


Advanced Configuration
----

### Command line options

#### Help: `[-h]`
You can get a list of all command line options by running
```bash
  ./candi.sh -h
```

You can combine the command line options given below.

#### Prefix path: ``[-p=<PATH>]``, ``[--prefix=<PATH>]``
```bash
  ./candi.sh --prefix=Your/Prefix/Path
```

#### Multiple build processes: ``[-j <N>]``, ``[--PROCS=<N>]``
```bash
  ./candi.sh -j <N>
```

* Example: to use 2 build processes type ``./candi.sh -j 2``.
* Be careful with this option! You need to have enough system memory (e.g. at least 8GB for 2 or more processes).

### Configuration file options

If you want to change the set of packages to be installed,
you can enable or disable a package in the configuration file
[candi.cfg](candi.cfg).
This file is a simple text file and can be changed with any text editor.

Currently, we provide the packages

* trilinos
* petsc, slepc
* superlu_dist (to be used with trilinos)
* p4est
* hdf5
* opencascade

and others. For a complete list see [deal.II-toolchain/packages](deal.II-toolchain/packages).

There are several other options within the configuration file, e.g.

* the ``DOWNLOAD_PATH`` folder (can be safely removed after installation),
* the ``UNPACK_PATH`` folder of the downloaded packages (can be safely removed after installation),
* the ``BUILD_PATH`` folder (can be safely removed after installation),
* the ``INSTALL_PATH`` destination folder,

and more.

### Single package installation mode

If you prefer to install only a single package, you can do so by
```bash
  ./candi.sh --packages="dealii"
```
for instance, or a set of packages by
```bash
  ./candi.sh --packages="opencascade petsc"
```

### Developer mode

Our installer provides a software developer mode by setting
``DEVELOPER_MODE=ON``
within [candi.cfg](candi.cfg).

More precisely, the developer mode skips the package ``fetch`` and ``unpack``,
everything else (package configuration, building and installation) is done
as before.

Note that you need to have a previous run of candi and
you must not remove the ``UNPACK_PATH`` directory.
Then you can modify source files in ``UNPACK_PATH`` of a package and
run candi again.
