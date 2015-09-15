# TDDFT with Quantum Espresso
This project uses Quantum Espresso to do research in Time-Dependent Density Functional Theory (TDDFT). We use Quantum Espresso version 5.2.0 and ce-tddft (made compatible for QE 5.1.X) written by Xiaofeng Qian and Davide Ceresoli (see the [ce-tddft github](https://github.com/dceresoli/ce-tddft) page).

This code adds to the time-dependent code provided in ce-tddft integration schemes, support for multiple k-points, capabilities for simulating bilayers, and more. The code is being developed by the [Materials Computation and Theory Group](http://web.stanford.edu/group/evanreed/index.html) at Stanford.

#### 0. Overview of this repo
Most of the code is copied directly from Quantum Espresso v 5.2.0. One exception is this README.md file. The Espresso README file is appended to this README.md file and the original README file has been deleted.

Folders that have been added include the following:

```
MCTG/
    This is a directory containing all extra information provided by the
    Materials Computation and Theory Group (MCTG) at Stanford, e.g.
    installation instructions (see below)
ce-tddft/
    This is the ce-tddft package originally written by the authors listed above.
    It is highly likely that this folder as is has been modified (by us) from
    the copy provided by the original authors on github.
reed-tddft/
    This folder contains new work done by the MCTG in the investigation of
    integration schemes.
TDDFPT/
    This folder is provided by default in Quantum Espresso, but it is likely
    that certain files inside have been modified from their original form.
```

#### 1. Installation
We are primarily using two clusters for our development:
  1) centurion
  2) sherlock.
Because centurion has an outdated version of gcc, we use the Intel Fortran compiler (ifort) on centurion. On sherlock, we are using gfortran, which is the default (as of Sep 2015, gcc 4.4.7 is the default, but a gcc 4.8.x module is available on the cluster).

For information on the installation of this package on each of these clusters, see the ```MCTG/``` folder in the top-level directory of this repo.

#### 2. 2H-MoS2 and 1T'-MoS2 examples
These examples use an E-field in the z-direction, include spin-degeneracy, and use a (16 x 16 x 1) k-point mesh with a 26 Ry (350 eV) cutoff energy (ecut). Atomic positions are relaxed with spin-orbital coupling, however norm-conserving pseudo-potentials are used instead of PAW which is required for spin-orbital coupling but not supported in ce-tddft yet.

Examples can be run in the same way as the graphene case

### Different time schemes in reed-tddft
This code (for now) cannot be compiled on centurion because a higher version of autoconf (which I used on the sherlock cluster) is required and I don't have the time yet to translate it to fit the lower version on centurion.

However, different time integration schemes can be found in the file
molecule_optical_absorption.yuanchange starting from line 194 and the attached codes are
in the folder src/attached.

The default method in for molecule_optical_absorption.yuanchange is Crank-Nicolson + Iterative Method. It needs to be commented out when other schemes are chosen.

# README (copied from Quantum Espresso)
This is the distribution of the Quantum ESPRESSO suite of codes (ESPRESSO: opEn-Source Package for Research in Electronic Structure, Simulation, and Optimization), promoted by the IOM-DEMOCRITOS National Simulation Center of the Italian CNR (http://www.democritos.it).

Quick installation instructions for the impatient:

```
$ ./configure [options]
$ make all
```
("make" alone prints a list of acceptable targets). Binaries go in bin/.
For more information, see the general documentation in directory Doc/,
package-specific documentation in */Doc/, and the web site
http://www.quantum-espresso.org/

All the material included in this distribution is free software;
you can redistribute it and/or modify it under the terms of the GNU
General Public License as published by the Free Software Foundation;
either version 2 of the License, or (at your option) any later version.

These programs are distributed in the hope that they will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY
or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License
for more details.

You should have received a copy of the GNU General Public License along
with this program; if not, write to the Free Software Foundation, Inc.,
675 Mass Ave, Cambridge, MA 02139, USA.
