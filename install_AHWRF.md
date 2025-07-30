# AHWRF Configure on HPC
Configuration of ARW HWRF (AHWRF) on HPC

These are some installation notes taken in the process of installing AHWRF. This tutorial is for **free**

## Module load and edit path

```console
cd $HOME
vi .bashrc
```
Edit in the .bashrc file
```console
. /opt/media-ext1/hwrf/spack/share/spack/setup-env.sh
spack env activate hwrf-test
spack load gcc@9.4.0
spack load openmpi@4.0.7%gcc@9.4.0
spack load cmake@3.18.6%gcc@9.4.0
spack load binutils@2.35.2%gcc@9.4.0
### parallel netcdf
export C_INCLUDE_PATH=$(spack location -i parallel-netcdf)/include:$C_INCLUDE_PATH
export LIBRARY_PATH=$(spack location -i parallel-netcdf)/lib:$LIBRARY_PATH
export LD_LIBRARY_PATH=$(spack location -i parallel-netcdf)/lib:$LD_LIBRARY_PATH
export PNETCDF=$(spack location -i parallel-netcdf)
### netcdf-fortran
export C_INCLUDE_PATH=$(spack location -i netcdf-fortran)/include/:$C_INCLUDE_PATH
export LIBRARY_PATH=$(spack location -i netcdf-fortran)/lib:$LIBRARY_PATH
export LD_LIBRARY_PATH=$(spack location -i netcdf-fortran)/lib:$LD_LIBRARY_PATH
export NETCDF=$(spack location -i netcdf-fortran)
### netcdf-c
export C_INCLUDE_PATH=$(spack location -i netcdf-c)/include/:$C_INCLUDE_PATH
export LIBRARY_PATH=$(spack location -i netcdf-c)/lib:$LIBRARY_PATH
export LD_LIBRARY_PATH=$(spack location -i netcdf-c)/lib:$LD_LIBRARY_PATH
### parallelio
export PIO=$(spack location -i parallelio)
export C_INCLUDE_PATH=$(spack location -i parallelio)/include/:$C_INCLUDE_PATH
export LIBRARY_PATH=$(spack location -i parallelio)/lib:$LIBRARY_PATH
export LD_LIBRARY_PATH=$(spack location -i parallelio)/lib:$LD_LIBRARY_PATH
### zlib
export C_INCLUDE_PATH=$(spack location -i zlib)/include:$C_INCLUDE_PATH
export LIBRARY_PATH=$(spack location -i zlib)/lib:$LIBRARY_PATH
export LD_LIBRARY_PATH=$(spack location -i zlib)/lib:$LD_LIBRARY_PATH
### libpng
export C_INCLUDE_PATH=$(spack location -i libpng)/include:$C_INCLUDE_PATH
export LIBRARY_PATH=$(spack location -i libpng)/lib:$LIBRARY_PATH
export LD_LIBRARY_PATH=$(spack location -i libpng)/lib:$LD_LIBRARY_PATH
## jasper
export JASPERLIB=/opt/media-ext1/mpas/dariTest1/mpas-lib/lib
export JASPERINC=/opt/media-ext1/mpas/dariTest1/mpas-lib/include/jasper
export LD_LIBRARY_PATH=/opt/media-ext1/mpas/dariTest1/mpas-lib/lib/:$LD_LIBRARY_PATH
```

## Building AHWRF

First of all, we need to download the source code from
```console
$ cd $HOME
$ wget https://github.com/wrf-model/WRF/archive/refs/tags/v4.7.0.tar.gz
$ tar -xvzf v4.7.0.tar.gz
$ cd $HOME/WRFV4.7.0
```

Now we are able to run the configure 

```console
$ ./configure
```
You will see various options. Choose the option that lists the compiler you are using and the way you wish to build AHWRF (i.e., seriall or in parallel). Although there are 3 different types of parallel (smpar, dmpar, and dm+sm), it is recommend choosing dmpar option.

```console
checking for perl5... no
checking for perl... found /usr/bin/perl (perl)
Will use NETCDF in dir: /home/wrf/wrf_io
HDF5 not set in environment. Will configure WRF for use without.
Will use PHDF5 in dir: /home/wrf/wrf_io
Will use 'time' to report timing information
Configuring to use jasper library to build Grib2 I/O...
  $JASPERLIB = /home/wrf/wrf_io/lib
  $JASPERINC = /home/wrf/wrf_io/include
------------------------------------------------------------------------
Please select from among the following Linux x86_64 options:

  1. (serial)   2. (smpar)   3. (dmpar)   4. (dm+sm)   PGI (pgf90/gcc)
  5. (serial)   6. (smpar)   7. (dmpar)   8. (dm+sm)   PGI (pgf90/pgcc): SGI MPT
  9. (serial)  10. (smpar)  11. (dmpar)  12. (dm+sm)   PGI (pgf90/gcc): PGI accelerator
 13. (serial)  14. (smpar)  15. (dmpar)  16. (dm+sm)   INTEL (ifort/icc)
                                         17. (dm+sm)   INTEL (ifort/icc): Xeon Phi (MIC architecture)
 18. (serial)  19. (smpar)  20. (dmpar)  21. (dm+sm)   INTEL (ifort/icc): Xeon (SNB with AVX mods)
 22. (serial)  23. (smpar)  24. (dmpar)  25. (dm+sm)   INTEL (ifort/icc): SGI MPT
 26. (serial)  27. (smpar)  28. (dmpar)  29. (dm+sm)   INTEL (ifort/icc): IBM POE
 30. (serial)               31. (dmpar)                PATHSCALE (pathf90/pathcc)
 32. (serial)  33. (smpar)  34. (dmpar)  35. (dm+sm)   GNU (gfortran/gcc)
 36. (serial)  37. (smpar)  38. (dmpar)  39. (dm+sm)   IBM (xlf90_r/cc_r)
 40. (serial)  41. (smpar)  42. (dmpar)  43. (dm+sm)   PGI (ftn/gcc): Cray XC CLE
 44. (serial)  45. (smpar)  46. (dmpar)  47. (dm+sm)   CRAY CCE (ftn $(NOOMP)/cc): Cray XE and XC
 48. (serial)  49. (smpar)  50. (dmpar)  51. (dm+sm)   INTEL (ftn/icc): Cray XC
 52. (serial)  53. (smpar)  54. (dmpar)  55. (dm+sm)   PGI (pgf90/pgcc)
 56. (serial)  57. (smpar)  58. (dmpar)  59. (dm+sm)   PGI (pgf90/gcc): -f90=pgf90
 60. (serial)  61. (smpar)  62. (dmpar)  63. (dm+sm)   PGI (pgf90/pgcc): -f90=pgf90
 64. (serial)  65. (smpar)  66. (dmpar)  67. (dm+sm)   INTEL (ifort/icc): HSW/BDW
 68. (serial)  69. (smpar)  70. (dmpar)  71. (dm+sm)   INTEL (ifort/icc): KNL MIC
 72. (serial)  73. (smpar)  74. (dmpar)  75. (dm+sm)   FUJITSU (frtpx/fccpx): FX10/FX100 SPARC64 IXfx/Xlfx

Enter selection [1-75] : 34
------------------------------------------------------------------------
Compile for nesting? (1=basic, 2=preset moves, 3=vortex following) [default 1]: 3

Configuration successful! 
------------------------------------------------------------------------
testing for fseeko and fseeko64
fseeko64 is supported
------------------------------------------------------------------------
Settings listed above are written to configure.wrf.
If you wish to change settings, please edit that file.
If you wish to change the default options, edit the file:
     arch/configure.defaults
NetCDF users note:
 This installation of NetCDF supports large file support.  To DISABLE large file
 support in NetCDF, set the environment variable WRFIO_NCD_NO_LARGE_FILE_SUPPORT
 to 1 and run configure again. Set to any other value to avoid this message.
  

Testing for NetCDF, C and Fortran compiler

This installation of NetCDF is 64-bit
                 C compiler is 64-bit
           Fortran compiler is 64-bit
              It will build in 64-bit

*****************************************************************************
This build of WRF will use classic (non-compressed) NETCDF format
*****************************************************************************

```

For this purpose we are going to compile AHWRF. Compilation should take about 20-30 minutes. The ongoing compilation can be checked.
```console
$ ./compile em_real &> log &
ls main/*exe  (real.exe, wrf.exe)
```

If we see this message, you done it right ;)

```console
==========================================================================
build started:   lun nov 18 21:48:48 -03 2019
build completed: lun nov 18 21:56:41 -03 2019
 
--->                  Executables successfully built                  <---
 
-rwxrwxr-x. 1 wrf wrf 57309864 nov 18 21:56 main/real.exe
-rwxrwxr-x. 1 wrf wrf 61189576 nov 18 21:56 main/wrf.exe
 
==========================================================================
```

Once the compilation completes, to check whether it was successful, you need to look for executables in the `WRFV4.7.0/main` directory.
```console
$ ls -las main/*.exe
real.exe (real data initialization)
wrf.exe (model executable)
```

These executables are linked to 2 different directories. You can choose to run AHWRF from either directory.
```console
WRFV4.7.0/run
WRFV4.7.0/test/em_real
```

Now we need to download and compile WPS

## Building WPS

NOTE: If you choosed in AHWRF to run on shared memory architecture (smpar), you need to add the flag of OpenMP (-lgomp) to the WRF_LIB variable in file configure.wps (just append it after -lnetcdf).

After the AHWRF model is built, the next step is building the WPS program (if you plan to run real cases, as opposed to idealized cases). The AHWRF model MUST be properly built prior to trying to build the WPS programs. If you do not already have the WPS source code, move to your `Build_WRF` directory, download that file and unpack it. Then go into the HWPSdev directory and make sure the HWPSdev directory is clean.

```console
$ cd $HOME
$ git clone https://github.com/wrf-model/WPS.git
$ cd $HOME/WPS
```

The next step is to configure WPS, however, you first need to set some paths for the ungrib libraries and then you can configure.
```console
$ ./configure
```
You should be given a list of various options for compiler types, whether to compile in serial or parallel, and whether to compile ungrib with GRIB2 capability. Unless you plan to create extremely large domains, it is recommended to compile WPS in serial mode, regardless of whether you compiled WRFV3 in parallel. It is also recommended that you choose a GRIB2 option (make sure you do not choose one that states "NO_GRIB2"). You may choose a non-grib2 option, but most data is now in grib2 format, so it is best to choose this option. You can still run grib1 data when you have built with grib2.

Choose the option that lists a compiler to match what you used to compile WRF, serial, and grib2. Note: The option number will likely be different than the number you chose to compile HWRFdev.

```console
Found Jasper environment variables for GRIB2 support...
  $JASPERLIB = /home/HWRFdev/wrf_io/lib
  $JASPERINC = /home/HWRFdev/wrf_io/include
------------------------------------------------------------------------
Please select from among the following supported platforms.

   1.  Linux x86_64, gfortran    (serial)
   2.  Linux x86_64, gfortran    (serial_NO_GRIB2)
   3.  Linux x86_64, gfortran    (dmpar)
   4.  Linux x86_64, gfortran    (dmpar_NO_GRIB2)
   5.  Linux x86_64, PGI compiler   (serial)
   6.  Linux x86_64, PGI compiler   (serial_NO_GRIB2)
   7.  Linux x86_64, PGI compiler   (dmpar)
   8.  Linux x86_64, PGI compiler   (dmpar_NO_GRIB2)
   9.  Linux x86_64, PGI compiler, SGI MPT   (serial)
  10.  Linux x86_64, PGI compiler, SGI MPT   (serial_NO_GRIB2)
  11.  Linux x86_64, PGI compiler, SGI MPT   (dmpar)
  12.  Linux x86_64, PGI compiler, SGI MPT   (dmpar_NO_GRIB2)
  13.  Linux x86_64, IA64 and Opteron    (serial)
  14.  Linux x86_64, IA64 and Opteron    (serial_NO_GRIB2)
  15.  Linux x86_64, IA64 and Opteron    (dmpar)
  16.  Linux x86_64, IA64 and Opteron    (dmpar_NO_GRIB2)
  17.  Linux x86_64, Intel compiler    (serial)
  18.  Linux x86_64, Intel compiler    (serial_NO_GRIB2)
  19.  Linux x86_64, Intel compiler    (dmpar)
  20.  Linux x86_64, Intel compiler    (dmpar_NO_GRIB2)
  21.  Linux x86_64, Intel compiler, SGI MPT    (serial)
  22.  Linux x86_64, Intel compiler, SGI MPT    (serial_NO_GRIB2)
  23.  Linux x86_64, Intel compiler, SGI MPT    (dmpar)
  24.  Linux x86_64, Intel compiler, SGI MPT    (dmpar_NO_GRIB2)
  25.  Linux x86_64, Intel compiler, IBM POE    (serial)
  26.  Linux x86_64, Intel compiler, IBM POE    (serial_NO_GRIB2)
  27.  Linux x86_64, Intel compiler, IBM POE    (dmpar)
  28.  Linux x86_64, Intel compiler, IBM POE    (dmpar_NO_GRIB2)
  29.  Linux x86_64 g95 compiler     (serial)
  30.  Linux x86_64 g95 compiler     (serial_NO_GRIB2)
  31.  Linux x86_64 g95 compiler     (dmpar)
  32.  Linux x86_64 g95 compiler     (dmpar_NO_GRIB2)
  33.  Cray XE/XC CLE/Linux x86_64, Cray compiler   (serial)
  34.  Cray XE/XC CLE/Linux x86_64, Cray compiler   (serial_NO_GRIB2)
  35.  Cray XE/XC CLE/Linux x86_64, Cray compiler   (dmpar)
  36.  Cray XE/XC CLE/Linux x86_64, Cray compiler   (dmpar_NO_GRIB2)
  37.  Cray XC CLE/Linux x86_64, Intel compiler   (serial)
  38.  Cray XC CLE/Linux x86_64, Intel compiler   (serial_NO_GRIB2)
  39.  Cray XC CLE/Linux x86_64, Intel compiler   (dmpar)
  40.  Cray XC CLE/Linux x86_64, Intel compiler   (dmpar_NO_GRIB2)

Enter selection [1-40] : 3
------------------------------------------------------------------------
Configuration successful. To build the WPS, type: compile
------------------------------------------------------------------------

Testing for NetCDF, C and Fortran compiler

This installation NetCDF is 64-bit
C compiler is 64-bit
Fortran compiler is 64-bit


```
The metgrid.exe and geogrid.exe programs rely on the AHWRF model's I/O libraries. There is a line in the configure.wps file that directs the WPS build system to the location of the I/O libraries from the AHWRF model.

Above is the default setting. As long as the name of the AHWRF model's top-level directory is "WRFV4.7.0" and the WPS and WRFV4.7.0 directories are at the same level (which they should be if you have followed exactly as instructed on this page so far), then the existing default setting is correct and there is no need to change it. If it is not correct, you must modify the configure file and then save the changes before compiling.

Once your configuration is complete, you should have a `configure.wps` file, and you are ready to compile. Edit first configure.wps as following:
```console
WRF_DIR	= ../WRFV4.7.0

COMPRESSION_INC     = -I/opt/media-ext1/mpas/dariTest1/mpas-lib/include
```

You can now compile WPS. Compilation should take a few minutes. The ongoing compilation can be checked.
```console
$ ./compile >& compile.log &
$ tail -f compile.log
```
Once the compilation completes, to check whether it was successful, you need to look for 3 main executables in the WPS top-level directory. Then verify that they are not zero-sized.
```console
$ ls -ls *.exe
geogrid.exe
metgrid.exe
ungrib.exe
```

## Static geography data

The AHWRF modeling system is able to create idealized simulations, though most users are interested in the real-data cases. To initiate a real-data case, the domain's physical location on the globe and the static information for that location must be created. This requires a data set that includes such fields as topography and land use categories. Move to your HOME directory, download the file and unpack it.
```console
$ cd $HOME
$ wget http://www2.mmm.ucar.edu/wrf/src/wps_files/geog_high_res_mandatory.tar.gz
$ tar -xvzf geog_high_res_mandatory.tar.gz

```
The directory infomation is given to the geogrid program in the namelist.wps file in the &geogrid section. The data expands to approximately 29 GB. This data allows a user to run the geogrid.exe program.
