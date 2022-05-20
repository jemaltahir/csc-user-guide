# mpiP: Lightweight MPI Profiler

## Available
    Puhti: 3.4.1
    Mahti: 3.4.1
## Usage    
[mpiP](http://mpip.sourceforge.net/) is a lightweight MPI profiler which uses statistical sampling to record profiling data. It generates less overhead and much less data than tracing tools. In order to use no code changes are required, only a re-link is needed. For both Mahti and Puhti, in addition to the compiler and MPI modules, one has to load the mpiP module: 
```
module load mpip
```
Next, the code is build as usual, but with the addition of the `-g` flag in compilation and the following linker flags. For Mahti one should use:
```
-lmpiP -lm -lbfd -liberty -L<path-to-unwind-lib> -lunwind 
```
The path to the *unwind* library on mahti is `/appl/spack/v014/install-tree/gcc-9.3.0/libunwind-1.3.1-otflii/lib`. 
Similarly, for Puhti the relinking is done with:
```
-lmpiP -lm -L<path-to-iberty-lib>  -liberty -L<path-to-unwind-lib> -lunwind
```
The path to the *iberty* library is `/appl/spack/install-tree/intel-19.0.4/libiberty-2.31.1-o4es74/lib/`, while the path to *unwind* library is `/appl/spack/install-tree/intel-19.0.4/libunwind-1.2.1-45uplb/lib/`. 
The above re-link will only work if it appears the last in the compiling line. 
Note that the compile options `-lmpiP ...`  have to be the last in the compiling list. For example in a `Makefile` we would have:

```
FC=mpif90
FCFLAGS=-O2 -g -fopenmp -march=native -fdefault-real-8 -fdefault-double-8 -lfftw3 -lfftw3_omp  -lmpiP -lm -lbfd -liberty -L/appl/spack/v014/install-tree/gcc-9.3.0/libunwind-1.3.1-otflii/lib -lunwind

all : TwoDMPIPFC.f
	$(FC) -o TwoDMPIPFC TwoDMPIPFC.f ${FCFLAGS} 

```
Next the code is ran as a usual batch job. The following additions are needed to the job script:
```
module load mpip
export LD_LIBRARY_PATH=<path-to-unwind-lib>:<path-to-iberty-lib>:$LD_LIBRARY_PATH
export MPIP="-t 10.0 -k 2 -c"
```
This will create profiling for all code in a file which is indicated before the programs's own output in the standard output. Here is an example of a output at the beginning of the execution of a code:

```
mpiP: Set the report print threshold to [10.00%].
mpiP: Set the callsite stack traceback depth to [2].
mpiP: 
mpiP: mpiP: mpiP V3.4.1 (Build Aug 28 2020/11:57:54)
mpiP: Direct questions and errors to mpip-help@lists.sourceforge.net
mpiP: 
mpiP: Storing mpiP output in [./TwoDMPIPFC.256.181196.1.mpiP].
mpiP:
``` 
The profiling data is stored in the `.mpiP`file indicated above. Below is an example: 



```
   
[ ... ]        
``` 

