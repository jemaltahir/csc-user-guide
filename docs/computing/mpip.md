# Spack

Spack is a package management tool designed to support multiple versions and configurations of software on a wide variety of platforms and environments. 

## Available
    Mahti: 0.17
## Usage 
In order to use `spack` one has first to unload all modules  and set the user´s folder where the programs are going to be installed. 
```
module purge
export  USER_SPACK_ROOT=/projappl/project_2001659/cristian/my_spack
```
Then the module can be load 
```
module load spack/v0.17-user
```
and initialize the spack with the command
```
user-spack-init 
```
In addition one can set the group which will part of this installation via the environment variable `USER_SPACK_GROUP`.



pIn the first part of the file is shown basic information about the performance experiment, followed by various statistics per invidual task and as aggregated. 
If one wants to focus only on specific regions the profilling can be switched on/off. For example in a Fortran code one could do:

```
call MPI_INIT( ierr )
call MPI_PCONTROL( 0 )      
[ ... ]                    
call MPI_PCONTROL( 1 )      
               
[ ... ]       ! Region of interest              

call MPI_PCONTROL( 0 )      
[ ... ]                     
call MPI_FINALIZE( ierr )
```
In the above code the debugging is first disabled after the MPI initialization (with `call MPI_CONTROL(0)`)  and later it is switched back for the region of interest.  At the end of the region the profiling is disabled again. In this case the profiler will only collect information for the region between the lines  `call MPI_CONTROL(1)` and  `call MPI_CONTROL(0)`.
Finally, mpiP has several configurable parameters  can set via the environment variable MPIP. For more details, please consult the related documentation at (https://github.com/LLNL/mpiP).
