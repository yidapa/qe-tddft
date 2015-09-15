# QE and TDDFT Installation instructions on Sherlock
This file explains how to install Quantum Espresso, ce-tddft, and reed-tddft on sherlock. To request an account on sherlock and for general information about the cluster, refer to the [Sherlock homepage](http://sherlock.stanford.edu/mediawiki/index.php/Main_Page). In particular, read about submitting jobs on the cluster and please do not run jobs on the master nodes.

#### 0 gcc versions
Note that installation on sherlock should be somewhat simpler than on centurion, since we are using the default gcc compiler (gcc 4.4.7, released 2010).

If you would like to compile with a more recent version of gcc, take a look at the modules available:

```
$ module avail
$ module load gcc/4.8.1     # or some other version of gcc
```
Since most of the code is written in Fortran, there should be no issues with  the older version of gcc since it implements the Fortran 2008 standard.

#### 1 Clone this github repo
```
$ git clone https://github.com/rehnd/qe-tddft.git
```

#### 2 Configure the code
Run the following

```
$ cd qe-tddft
$ ./configure
```

#### 3 Compile the code
Run the following:
```
$ make pw
```
or alternatively,

```
$ make all
```

which will build all packages in QE, not just the PW package.

#### 4 Build the ce-tddft code
Run the following:

```
$ cd ce-tddft
$ autoconf
$ ./configure --with-qe-source=..
$ mkdir bin
$ make all
```
That should conclude the build process. Check to make sure no errors appear.

## Running examples
Will write this section later.
