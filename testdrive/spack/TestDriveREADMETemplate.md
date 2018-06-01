# Test Driving the Spack Package Manager

This document will walk you through preparing and using the [Spack](https://spack.io/) package manager to
install a basic software stack for HEP.

Spack is a multi-platform package manager that builds and installs multiple versions and configurations of software. It works on Linux, macOS, and many supercomputers. Spack is non-destructive: installing a new version of a package does not break existing installations, so many configurations of the same package can coexist.

Spack offers a simple "spec" syntax that allows users to specify versions and configuration options. Package files are written in pure Python, and specs allow package authors to write a single script for many different builds of the same package. With Spack, you can build your software all the ways you want to.

[Full documentation](http://spack.readthedocs.io/) for Spack.

## Base Operating System Install

_**AUTHORS:**_ _Assume a base system of macOS High Sierra with Xcode 9, or a Docker Image based on centos:centos7 or
ubuntu:xenial. In the Docker case, supply (a) Dockerfile(s) for each system, adding any additional system packages
required, and (b) an `hsf` user with `sudo` privileges that the container will run as. There’s no requirement
to build and host the images. Bonus points: Singularity! If macOS needs additional packages, document them below:_

Test driving Spack requires either a CentOS6/7, Ubuntu 16.04LTS, ~or macOS High Sierra system~. For macOS, only the base system
plus Xcode 9 from the App Store (_**AUTHORS:**_ _add any additional requirements here_) is required. For convenience and
reproducibility, Docker images are available for Linux, and can be obtained and run as follows:

_**AUTHORS:**_ _Add Docker pull/build/run instructions here_

**Build your own image**

```
docker build {c7,ubuntu16}-spack
```

Optionally, you may use an existing Linux installation, but you may encounter errors in subsequent steps if it is missing
(or has incompatible) packages, or your environment has custom settings.

## Installing Spack

To install Spack, please follow the [Getting Started](http://spack.readthedocs.io/en/latest/getting_started.html) documentation.

**Prerequisites**

Spack has the following minimum requirements, which must be installed before Spack is run:

- Python 2 (2.6 or 2.7) or 3 (3.3 - 3.6)
- A C/C++ compiler
- The `git` and `curl` commands.
- If using the `gpg` subcommand, `gnupg2` is required.

**Installation**

Getting Spack is easy. You can clone it from the [github repository](https://github.com/spack/spack) using this command:

```
$ git clone https://github.com/spack/spack.git
```

**Check installation**

With Spack installed, you should be able to run some basic Spack commands. For example:

```
$ spack spec netcdf
Input spec
--------------------------------
netcdf

Concretized
--------------------------------
netcdf@4.6.1%gcc@5.4.0~dap~hdf4 maxdims=1024 maxvars=8192 +mpi~parallel-netcdf+shared arch=linux-ubuntu16.04-x86_64
    ^hdf5@1.10.1%gcc@5.4.0~cxx~debug~fortran+hl+mpi+pic+shared~szip~threadsafe arch=linux-ubuntu16.04-x86_64
        ^openmpi@3.1.0%gcc@5.4.0~cuda fabrics= ~java~memchecker~pmi schedulers= ~sqlite3~thread_multiple+vt arch=linux-ubuntu16.04-x86_64
            ^hwloc@1.11.9%gcc@5.4.0~cairo~cuda+libxml2+pci+shared arch=linux-ubuntu16.04-x86_64
                ^libpciaccess@0.13.5%gcc@5.4.0 arch=linux-ubuntu16.04-x86_64
                    ^libtool@2.4.6%gcc@5.4.0 arch=linux-ubuntu16.04-x86_64
                        ^m4@1.4.18%gcc@5.4.0 patches=3877ab548f88597ab2327a2230ee048d2d07ace1062efe81fc92e91b7f39cd00 +sigsegv arch=linux-ubuntu16.04-x86_64
                            ^libsigsegv@2.11%gcc@5.4.0 arch=linux-ubuntu16.04-x86_64
                    ^pkgconf@1.4.0%gcc@5.4.0 arch=linux-ubuntu16.04-x86_64
                    ^util-macros@1.19.1%gcc@5.4.0 arch=linux-ubuntu16.04-x86_64
                ^libxml2@2.9.8%gcc@5.4.0~python arch=linux-ubuntu16.04-x86_64
                    ^xz@5.2.4%gcc@5.4.0 arch=linux-ubuntu16.04-x86_64
                    ^zlib@1.2.11%gcc@5.4.0+optimize+pic+shared arch=linux-ubuntu16.04-x86_64
                ^numactl@2.0.11%gcc@5.4.0 arch=linux-ubuntu16.04-x86_64
                    ^autoconf@2.69%gcc@5.4.0 arch=linux-ubuntu16.04-x86_64
                        ^perl@5.26.2%gcc@5.4.0+cpanm+shared+threads arch=linux-ubuntu16.04-x86_64
                            ^gdbm@1.14.1%gcc@5.4.0 arch=linux-ubuntu16.04-x86_64
                                ^readline@7.0%gcc@5.4.0 arch=linux-ubuntu16.04-x86_64
                                    ^ncurses@6.0%gcc@5.4.0 patches=4110a40613b800da2b2888c352b64c75a82809d48341061e4de5861e8b28423f,f84b2708a42777aadcc7f502a261afe10ca5646a51c1ef8b5e60d2070d926b57 ~symlinks~termlib arch=linux-ubuntu16.04-x86_64
                    ^automake@1.16.1%gcc@5.4.0 arch=linux-ubuntu16.04-x86_64

```

Try installing a simple package:

```
$ spack install zlib
```

## Installing the HEP Test Stack
The HSF Test Stack packages are as follows:

- **Toolchain**
  - GCC 6.4
    - With c, c++, fortran languages
  - Python 2.7.14
- **Core HEP Stack**
  - Boost 1.65
  - ROOT 6.12.06
    - Including PyROOT, MathMore
  - GSL 2.4
  - Qt5 5.10 (`qtbase` only)
  - Xerces-C 3.1.4
  - CLHEP (_Version to be compatible with Geant4_)
  - Geant4 10.3

To install this stack, ...

_**AUTHORS:**_ _Document the steps needed to install these packages from source, and separately from
binary if the tool provides these_

```
$ pkgs="boost@1.65.0      \
        gsl@2.4           \
        qt@5.10           \
        xerces-c@3.1.4    \
        root@6.06.08      \
        geant4@10.03.p03"
        # CLHEP will be installed by Geant4

$ spack install $pkgs %gcc@6.3.1
```

Optionally, ...

- Dummy package

```
$ spack create -n spack-test-drive
```

This will create an empty package for us with a default template

```
##############################################################################
# Copyright (c) 2013-2018, Lawrence Livermore National Security, LLC.
# Produced at the Lawrence Livermore National Laboratory.
#
# This file is part of Spack.
# Created by Todd Gamblin, tgamblin@llnl.gov, All rights reserved.
# LLNL-CODE-647188
#
# For details, see https://github.com/spack/spack
# Please also see the NOTICE and LICENSE files for our notice and the LGPL.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU Lesser General Public License (as
# published by the Free Software Foundation) version 2.1, February 1999.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY; without even the IMPLIED WARRANTY OF
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the terms and
# conditions of the GNU Lesser General Public License for more details.
#
# You should have received a copy of the GNU Lesser General Public
# License along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA 02111-1307 USA
##############################################################################
#
# This is a template package file for Spack.  We've put "FIXME"
# next to all the things you'll want to change. Once you've handled
# them, you can save this file and test your package like this:
#
#     spack install spack-test-drive
#
# You can edit this file again by typing:
#
#     spack edit spack-test-drive
#
# See the Spack documentation for more information on packaging.
# If you submit this package back to Spack as a pull request,
# please first remove this boilerplate and all FIXME comments.
#
from spack import *


class SpackTestDrive(Package):
    """FIXME: Put a proper description of your package here."""

    # FIXME: Add a proper url for your package's homepage here.
    homepage = "http://www.example.com"
    url      = "http://www.example.com/example-1.2.3.tar.gz"

    # FIXME: Add proper versions and checksums here.
    # version('1.2.3', '0123456789abcdef0123456789abcdef')

    # FIXME: Add dependencies if required.
    # depends_on('foo')

    def install(self, spec, prefix):
        # FIXME: Unknown build system
        make()
        make('install')
```

Let's modify it to create a dummy package which contains the desired packages as dependencies:

```
##############################################################################
# Copyright (c) 2013-2018, Lawrence Livermore National Security, LLC.
# ...
##############################################################################
from spack import *


class SpackTestDrive(Package):

    version('1.0.0')

    # Toolchain
    depends_on('gcc@6.4.0')
    depends_on('Python@2.7.14')

    # Required Core HEP Stack
    depends_on('boost@1.65.0)
    depends_on('gsl@2.4')         
    depends_on('qt@5.10.0' )        
    depends_on('xerces-c@3.1.4')  
    depends_on('root@6.06.08' )   
    depends_on('geant4@10.03.p03')

```

This dummy package can now be 'installed' by:

```
$ spack install spack-test-drive
```

which will not install a proper `spack-test-drive` but the specified dependencies.



_**AUTHORS:**_ _Show any other features of the tool you think are useful here, e.g. build using different C++ Standards,
optional components of packages, different package versions_

# Using the HEP Test Stack
To use the freshly installed test stack, ...

_**AUTHORS:**_ _Document the steps needed to setup a runtime environment for using
the stack listed above, including optional parts if the tools allows this_

# Adding a New Package to the Stack
To add a new package to the stack, ...

_**AUTHORS:**_ _Document the steps needed to create a package for a simple C/C++
library or application (your choice) and walkthrough the steps to build and install it. Reuse/link to the tool's
own documentation on this if you think it's sufficient_
