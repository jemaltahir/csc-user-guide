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

Installing a package is done  relatively simple using the `install` command:
```
spack install <package_name>
``` 
This process involves 3 steps. 


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
At the moment of wrirting the document the output of the above command is:
``` 
$ spack find
==> 609 installed packages
-- linux-rhel8-x86_64 / aocc@3.2.0 ------------------------------
openssl@1.1.1k

-- linux-rhel8-x86_64_v3 / gcc@7.5.0 ----------------------------
autoconf@2.69    berkeley-db@18.1.40  diffutils@3.8  hwloc@2.2.0     libiconv@1.16    libtool@2.4.6  m4@1.4.19    numactl@2.0.14  perl@5.34.0    pmix@3.2.3    slurm@20.02.6  zlib@1.2.11
automake@1.16.3  bzip2@1.0.8          gdbm@1.19      libevent@2.1.8  libsigsegv@2.13  lustre@2.12.3  ncurses@6.2  openmpi@4.1.2   pkgconf@1.8.0  readline@8.1  ucx@1.12.0

-- linux-rhel8-x86_64_v3 / gcc@8.5.0 ----------------------------
aocc@3.2.0           gcc@10.3.0                           libbsd@0.11.3         libxpm@3.5.12      patchelf@0.13                py-kombu@5.0.2             randrproto@1.5.0
apr@1.6.2            gcc@11.2.0                           libcerf@1.3           libxrandr@1.5.0    pcre@8.44                    py-maestrowf@1.1.7dev0     readline@8.1
apr-util@1.6.1       gdbm@1.19                            libedit@3.1-20210216  libxrender@0.9.10  pcre2@10.36                  py-merlin@1.7.5            redis@6.2.5
autoconf@2.69        gettext@0.21                         libelf@0.8.13         libxt@1.1.5        pdsh@2.31                    py-numpy@1.21.3            renderproto@0.11.1
automake@1.16.3      git@2.31.1                           libevent@2.1.8        libyaml@0.2.5      perl@5.34.0                  py-packaging@21.0          rust@1.51.0
berkeley-db@18.1.40  git-lfs@2.11.0                       libffi@3.3            libzmq@4.3.3       perl-alien-svn@1.8.11.0      py-parse@1.18.0            screen@4.8.0
binutils@2.33.1      glib@2.70.0                          libgd@2.2.4           llvm@12.0.1        perl-module-build@0.4224     py-pip@21.1.2              serf@1.3.9
binutils@2.37        glproto@1.4.17                       libgit2@1.1.1         lua@5.3.4          perl-termreadkey@2.38        py-prompt-toolkit@3.0.17   sqlite@3.36.0
binutils@2.37        gmake@4.3                            libice@1.0.9          lua-luaposix@35.0  pixman@0.40.0                py-psutil@5.8.0            subversion@1.14.0
bzip2@1.0.8          gmp@6.2.1                            libiconv@1.16         lz4@1.9.3          pkgconf@1.8.0                py-pycparser@2.20          swig@2.0.12
cairo@1.16.0         gnuplot@5.4.2                        libidn2@2.2.0         m4@1.4.19          py-amqp@5.0.1                py-pyparsing@2.4.7         swig@4.0.2
cmake@3.21.4         gnutls@3.6.16                        libjpeg-turbo@2.1.0   mesa@21.2.3        py-attrs@21.2.0              py-pyrsistent@0.15.7       tar@1.34
cuda@11.5.0          go@1.17.2                            libmd@1.0.3           mesa-glu@9.0.1     py-billiard@3.6.3.0          py-pytz@2021.1             texinfo@6.5
curl@7.79.0          go-bootstrap@1.4-bootstrap-20171003  libpng@1.6.37         molden@6.7         py-cached-property@1.5.2     py-pyyaml@5.3.1            tmux@3.2a
curl@7.79.0          gobject-introspection@1.56.1         libpthread-stubs@0.4  mpc@1.1.0          py-celery@5.0.0              py-redis@3.5.3             unzip@6.0
czmq@4.1.1           harfbuzz@2.6.8                       libsigsegv@2.13       mpc@1.1.0          py-cffi@1.15.0               py-semantic-version@2.8.2  utf8proc@2.4.0
diffutils@3.8        htop@3.1.1                           libsm@1.2.3           mpfr@3.1.6         py-click@7.1.2               py-setuptools@58.2.0       util-linux-uuid@2.36.2
emacs@27.2           hwloc@2.2.0                          libsodium@1.0.18      mpfr@4.1.0         py-click-didyoumean@0.0.3    py-setuptools-rust@0.12.1  vim@8.2.2541
expat@2.4.1          icu4c@67.1                           libssh2@1.8.0         ncurses@6.2        py-click-repl@0.1.6          py-setuptools-scm@6.3.2    xcb-proto@1.14.1
fio@3.26             inputproto@2.3.2                     libtiff@4.3.0         nghttp2@1.44.0     py-coloredlogs@14.0          py-six@1.16.0              xextproto@7.3.0
fish@3.3.1           intel-oneapi-compilers@2021.4.0      libtool@2.4.6         ninja@1.10.2       py-cryptography@3.4.7        py-sqlalchemy@1.4.20       xproto@7.0.31
flux-core@0.30.0     intel-oneapi-mkl@2021.4.0            libx11@1.7.0          nvhpc@21.11        py-cython@0.29.24            py-tabulate@0.8.9          xrandr@1.5.0
font-util@1.3.2      intel-oneapi-mpi@2021.4.0            libxau@1.0.8          nvhpc@22.3         py-dill@0.3.4                py-toml@0.10.2             xtrans@1.3.5
fontconfig@2.13.94   intel-oneapi-tbb@2021.4.0            libxcb@1.14           openblas@0.3.18    py-filelock@3.0.12           py-tomli@1.2.1             xz@5.2.5
freetype@2.11.0      isl@0.20                             libxdmcp@1.1.2        openssh@8.0p1      py-greenlet@1.1.0            py-vine@5.0.0              yaml-cpp@0.7.0
fribidi@1.0.5        isl@0.21                             libxext@1.3.3         openssl@1.1.1k     py-humanfriendly@8.2         py-wcwidth@0.1.7           z3@4.8.9
gcc@7.5.0            jansson@2.13.1                       libxml2@2.9.12        p7zip@16.02        py-importlib-metadata@4.8.1  py-zipp@3.6.0              zlib@1.2.11
gcc@9.4.0            kbproto@1.0.7                        libxmu@1.1.2          pango@1.42.0       py-jsonschema@3.2.0          python@3.8.12              zstd@1.5.0

-- linux-rhel8-x86_64_v3 / gcc@11.2.0 ---------------------------
cmake@3.21.4  metis@5.1.0  ncurses@6.2  openssl@1.1.1k  pkgconf@1.8.0

-- linux-rhel8-zen2 / aocc@3.2.0 --------------------------------
amdblis@3.1                  berkeley-db@18.1.40  expat@2.4.1    hwloc@2.2.0        libidn2@2.2.0        libunwind@1.5.0  netcdf-c@4.8.1          pkgconf@1.8.0  tar@1.34
amdfftw@3.1                  boost@1.77.0         gdbm@1.19      icu4c@67.1         libjpeg-turbo@2.1.0  libxml2@2.9.12   netcdf-fortran@4.5.3    pmix@3.2.3     texinfo@6.5
amdlibflame@3.1              bzip2@1.0.8          gettext@0.21   jasper@2.0.32      libmd@1.0.3          m4@1.4.19        nghttp2@1.44.0          python@3.6.2   ucx@1.12.0
amdlibm@3.1                  cmake@3.22.2         gmp@6.2.1      libaec@1.0.5       libpciaccess@0.16    metis@5.1.0      numactl@2.0.14          python@3.8.12  util-linux-uuid@2.36.2
amdscalapack@3.0             curl@7.79.0          gnutls@3.6.16  libbsd@0.11.3      libpng@1.6.37        mpfr@4.1.0       openssl@1.1.1k          readline@8.1   xz@5.2.5
autoconf@2.69                diffutils@3.8        hdf@4.2.15     libdwarf@20180129  libsigsegv@2.13      mpich@4.0.1      papi@6.0.0.1            scotch@6.1.1   zlib@1.2.11
autoconf-archive@2019.01.06  eccodes@2.24.2       hdf5@1.10.7    libffi@3.3         libssh2@1.8.0        mumps@5.4.0      parallel-netcdf@1.12.2  slurm@20.02.6
automake@1.16.3              elfutils@0.186       hdf5@1.10.7    libiconv@1.16      libtool@2.4.6        ncurses@6.2      perl@5.34.0             sqlite@3.36.0

-- linux-rhel8-zen2 / gcc@9.4.0 ---------------------------------
amber@20             charmpp@6.10.2    extrae@3.8.3       gromacs@2021.3    libdwarf@20180129    libunwind@1.5.0  netcdf-c@4.8.1          plumed@2.7.2          ucx@1.11.2
amber@20             cmake@3.21.4      extrae@3.8.3       gromacs@2021.5    libevent@2.1.8       libxc@5.1.5      netcdf-fortran@4.5.3    pmix@3.2.2            ucx@1.12.0
amdblis@3.1          cp2k@8.2          fftw@3.3.10        gsl@2.7           libffi@3.3           libxml2@2.9.12   netlib-scalapack@2.1.0  pmix@3.2.3            ucx@1.12.1
amdscalapack@3.0     cp2k@8.2          fftw@3.3.10        gsl@2.7           libflame@5.2.0       libxsmm@1.16.3   nghttp2@1.44.0          proj@7.2.1            udunits@2.2.28
antlr@2.7.7          cp2k@9.1          fftw@3.3.10        hdf@4.2.15        libiconv@1.16        libxsmm@1.17     numactl@2.0.14          proj@8.1.0            util-linux-uuid@2.36.2
autoconf@2.69        cubelib@4.6       fftw@3.3.10        hdf5@1.10.7       libidn2@2.2.0        lustre@2.12.3    opari2@2.0.6            py-cython@0.29.24     util-macros@1.19.3
automake@1.16.3      cubew@4.6         findutils@4.8.0    hdf5@1.10.7       libint@2.6.0         m4@1.4.19        openblas@0.3.18         py-setuptools@58.2.0  xz@5.2.5
berkeley-db@18.1.40  cuda@11.1.1       flex@2.6.3         help2man@1.47.16  libint@2.6.0         metis@5.1.0      openmpi@4.1.2           python@3.8.12         zlib@1.2.11
binutils@2.33.1      curl@7.79.0       flex@2.6.4         hwloc@1.11.8      libjpeg-turbo@2.1.0  mpfr@4.1.0       openmpi@4.1.2           readline@8.1
bison@3.8.2          diffutils@3.8     gdbm@1.19          hwloc@2.2.0       libmd@1.0.3          mpich@4.0.1      openssl@1.1.1k          scalasca@2.6
boost@1.77.0         eccodes@2.24.2    gettext@0.21       hwloc@2.2.0       libpciaccess@0.16    mumps@5.4.0      otf2@2.3                scorep@7.0
boost@1.77.0         eigen@3.4.0       gmp@6.2.1          icu4c@67.1        libpng@1.6.37        namd@2.14        papi@6.0.0.1            scotch@6.1.1
boost@1.77.0         elfutils@0.186    gnutls@3.6.16      jasper@2.0.32     libsigsegv@2.13      namd@2.14        parallel-netcdf@1.12.2  slurm@20.02.6
bzip2@1.0.8          elpa@2020.05.001  googletest@1.10.0  jemalloc@5.2.1    libssh2@1.8.0        nasm@2.15.05     perl@5.34.0             sqlite@3.36.0
cdo@1.9.10           elpa@2021.11.001  gromacs@2020.4     libaec@1.0.5      libtiff@4.3.0        nco@5.0.1        pkgconf@1.8.0           tar@1.34
cgal@5.0.3           expat@2.4.1       gromacs@2020.5     libbsd@0.11.3     libtool@2.4.6        ncurses@6.2      plumed@2.6.3            tcl@8.6.11

-- linux-rhel8-zen2 / gcc@11.2.0 --------------------------------
antlr@2.7.7          diffutils@3.8      googletest@1.10.0  libiberty@2.33.1      libxscrnsaver@1.2.2     openblas@0.3.18         py-pillow@8.0.0           scotch@6.1.1
autoconf@2.69        eccodes@2.24.2     gperf@3.1          libiconv@1.16         lustre@2.12.3           openjpeg@1.5.2          py-pip@21.1.2             scrnsaverproto@1.2.2
automake@1.16.3      eccodes@2.24.2     grib-api@1.24.0    libidn2@2.2.0         m4@1.4.19               openmpi@4.1.2           py-ply@3.11               slurm@20.02.6
bdftopcf@1.0.5       eigen@3.4.0        gsl@2.7            libjpeg-turbo@2.1.0   mkfontdir@1.0.7         openssl@1.1.1k          py-pybind11@2.6.2         sqlite@3.36.0
berkeley-db@18.1.40  elfutils@0.186     gsl@2.7            libmd@1.0.3           mkfontscale@1.1.2       otf2@2.3                py-pyparsing@2.4.7        tar@1.34
binutils@2.33.1      elpa@2021.05.001   hdf@4.2.15         libpciaccess@0.16     mpfr@4.1.0              papi@6.0.0.1            py-pytest-runner@5.1      tcl@8.6.11
bison@3.8.2          expat@2.4.1        hdf5@1.10.7        libpng@1.6.37         mpich@4.0.1             parallel-netcdf@1.12.2  py-python-dateutil@2.8.2  tk@8.6.11
blitz@1.0.2          extrae@3.8.3       hdf5@1.10.7        libpthread-stubs@0.4  mumps@5.4.0             parallel-netcdf@1.12.2  py-pythran@0.9.12         ucx@1.12.0
boost@1.69.0         extrae@3.8.3       hdf5@1.10.7        libsigsegv@2.13       mumps@5.4.0             perl@5.34.0             py-scipy@1.7.1            udunits@2.2.28
boost@1.77.0         fftw@3.3.10        help2man@1.47.16   libssh2@1.8.0         namd@2.14               pkgconf@1.8.0           py-setuptools@57.4.0      util-linux-uuid@2.36.2
boost@1.77.0         fftw@3.3.10        hpl@2.3            libtiff@4.3.0         namd@2.14               pmix@3.2.3              py-setuptools-scm@6.3.2   util-macros@1.19.3
boost@1.77.0         fftw@3.3.10        hwloc@2.2.0        libtool@2.4.6         nasm@2.15.05            proj@7.2.1              py-six@1.16.0             xcb-proto@1.14.1
bzip2@1.0.8          fftw@3.3.10        icu4c@67.1         libunwind@1.5.0       nco@5.0.1               proj@8.1.0              py-tomli@1.2.1            xextproto@7.3.0
cdo@1.9.10           findutils@4.8.0    inputproto@2.3.2   libx11@1.7.0          ncurses@6.2             py-beniget@0.4.1        python@3.8.12             xproto@7.0.31
cdo@1.9.10           flex@2.6.4         jasper@2.0.32      libxau@1.0.8          netcdf-c@4.8.1          py-certifi@2021.10.8    python@3.9.7              xtrans@1.3.5
cgal@5.0.3           font-util@1.3.2    jemalloc@5.2.1     libxc@5.1.5           netcdf-c@4.8.1          py-cppy@1.1.0           qhull@2020.2              xz@5.2.5
charmpp@6.10.2       fontconfig@2.13.1  kbproto@1.0.7      libxcb@1.14           netcdf-fortran@4.5.3    py-cycler@0.10.0        readline@8.1              zlib@1.2.11
cmake@3.21.4         fontsproto@2.1.3   libaec@1.0.5       libxdmcp@1.1.2        netcdf-fortran@4.5.3    py-cython@0.29.24       renderproto@0.11.1
cubelib@4.6          freetype@2.11.0    libbsd@0.11.3      libxext@1.3.3         netlib-scalapack@2.1.0  py-gast@0.5.2           scalasca@2.6
cubew@4.6            gdbm@1.19          libdwarf@20180129  libxfont@1.5.2        netlib-scalapack@2.1.0  py-kiwisolver@1.3.2     scalasca@2.6
cuda@11.5.0          gettext@0.21       libevent@2.1.8     libxft@2.3.2          nghttp2@1.44.0          py-matplotlib@3.4.3     scorep@7.0
cudnn@8.3.3.40-11.5  gmp@6.2.1          libffi@3.3         libxml2@2.9.12        numactl@2.0.14          py-numpy@1.21.3         scorep@7.0
curl@7.79.0          gnutls@3.6.16      libfontenc@1.1.3   libxrender@0.9.10     opari2@2.0.6            py-packaging@21.0       scotch@6.1.1
``` 

## Dependencies and Variants

(https://github.com/LLNL/mpiP).
