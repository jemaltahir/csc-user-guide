# Spack

Spack is a package management tool designed to support multiple versions and configurations of software on a wide variety of platforms and environments. 

## Available
    Mahti: 0.17
##  Basic Usage 
In order to use `spack` one has first to unload all modules  and set the user´s folder where the programs are going to be installed. 
```
module purge
export  USER_SPACK_ROOT=/pathto/my_spack
```
Then the module can be load 
```
module load spack/v0.17-user
```
First time when it is used the user´s spack needs to be initialized:
```
user-spack-init 
```
In addition one can set the group which will part of this installation via the environment variable `USER_SPACK_GROUP`.

The Mahti base installation already comes with several compilers configured:
```
$ spack compilers
==> Available compilers
-- aocc rhel8-x86_64 --------------------------------------------
aocc@3.2.0

-- gcc rhel8-x86_64 ---------------------------------------------
gcc@11.2.0  gcc@9.4.0  gcc@8.5.0  gcc@7.5.0

-- intel rhel8-x86_64 -------------------------------------------
intel@2021.4.0

-- nvhpc rhel8-x86_64 -------------------------------------------
nvhpc@22.3  nvhpc@21.11

-- oneapi rhel8-x86_64 ------------------------------------------
oneapi@2021.4.0
``` 
The installed packages can be sees using the `find` command:
```
spack find
```
New packages are installed using the `install` command:
```
spack install <package_name>
``` 

## Dependencies and Variants

(https://github.com/LLNL/mpiP).
