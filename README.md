# TDDFT with Quantum Espresso
This project is aimed at using Quantum Espresso to do new research in Time-Dependent Density Functional Theory (TDDFT). We use Quantum Espresso version 5.2.0 and ce-tddft written by Xiaofeng Qian and Davide Ceresoli (see the [ce-tddft github](https://github.com/dceresoli/ce-tddft) page).

This code adds to the time-dependent code provided in ce-tddft by extending to bilayers, providing new integration schemes, and more. The code is being developed by the [Materials Computation and Theory Group](http://web.stanford.edu/group/evanreed/index.html) at Stanford.

##Installation instructions (Centurion)

####1 Clone this github repo
```
$ git clone https://github.com/rehnd/qe-tddft.git
```
Note: this may fail due to firewall on centurion. In this case, use

```
$ git clone git://github.com/rehnd/qe-tddft.git
```

####2 Edit your .bashrc file to use the intel compiler and openmpi. Add the following lines to ~/.bashrc
```
source /opt/intel/fce/11/bin/ifortvars.sh intel 64
source /opt/open-mpi/tcp-intel11/mpivars.sh
source /opt/intel/mkl/10.1/tools/environment/mklvarsem64t.sh
```

####3 Source your .bashrc file
```
$ source ~/.bashrc
```

####4 Enter the espresso folder and configure
```
$ cd espresso
$ ./configure --prefix=`pwd`
```
####5 Edit make.sys
Change line 67 in make.sys

```
F77			= ifort
```

####6 Install epsresso pw code
In the main top-level directory of this repo, run

```
$ make pw
```
Alternatively, it is fine to run

```
$ make all
```
which will build all of the packages in espresso (not just the pw package).


####7 Install ce-tddft (the original one with no modifications)

```
$ cd ce-tddft
$ autoconf
$ ./configure --with-qe-source=..
```

The executable tddft.x is in the folder ce-tddft/bin

####8 Configure the runscript on the PBS on centurion
Change to the bin directory in the top-level espresso directory
```
$ cd espresso/bin/
```

Open the file titled ```run_espresso``` and edit line 43 to say

```
echo "mpirun -bynode -np $nproc $your_dir/espresso/bin/pw.x  < $input >& `pwd`/$output" >> $script
```
where $your_dir is the directory that contains the top-level espresso directory.

Now change to the ce-tddft bin directory

```
$ cd ce-tddft/bin/
```

Open the ```run_cetddft``` shell script and change line 43 to

```
echo "mpirun -bynode -np $nproc $your_dir/espresso/ce-tddft/bin/tddft.x  < $input >& `pwd`/$output" >> $script
```
That concludes the installation.

##Running Examples
The following describes how to run examples to ensure that the installation worked properly

####1 Graphene
The graphene example uses an E-field in the z-direction, includes spin-degeneracy, and uses a (16 x 16 x 1) k-point mesh with a 25 Ry cutoff energy.

```
$ cd ce-tddft/examples/graphene
```

Please read the run_espresso file carefully to make sure you understand how to run jobs on the PBS. 

To run the example, execute the following


```
$ ../../../bin/run_espresso 1 16 graphene.pw-in graphene.pw-out
```

For now, the ce-tddft code is still serial but you can nevertheless run the following command when the PW is finished. Please use a reasonable number of time steps for time propagation.


```
$ ../../bin/run_cetddft 1 16 graphene.tddft-in graphene.tddft-out
$ grep ^DIP graphene.tddft-out > dip_z.dat
$ python ../../tools/plot_optical_absorption.py z
```

Compare your results with the ones in ```examples/graphene/compare_results```

####2 2H-MoS2 and 1T'-MoS2 examples
These examples use an E-field in the z-direction, include spin-degeneracy, and use a (16 x 16 x 1) k-point mesh with a 26 Ry (350 eV) cutoff energy (ecut). Atomic positions are relaxed with spin-orbital coupling, however norm-conserving pseudo-potentials are used instead of PAW which is required for spin-orbital coupling but not supported in ce-tddft yet. 

Examples can be run in the same way as the graphene case

###Different Time-Schemes in reed-tddft
This code (for now) cannot be compiled on centurion because a higher version of autoconf 
(which I used on the sherlock cluster) is required and I don't have the time yet to translate it to 
fit the lower version on centurion.

However, different time integration schemes can be found in the file 
molecule_optical_absorption.yuanchange starting from line 194 and the attached codes are
in the folder src/attached.

The default method in for molecule_optical_absorption.yuanchange is Crank-Nicolson + 
Iterative Method. It needs to be commented out when other schemes are chosen.


## Quantum Espresso README (copied from Quantum Espresso)
This is the distribution of the Quantum ESPRESSO suite of codes (ESPRESSO: 
opEn-Source Package for Research in Electronic Structure, Simulation, 
and Optimization), promoted by the IOM-DEMOCRITOS National Simulation Center 
of the Italian CNR (http://www.democritos.it). 

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


