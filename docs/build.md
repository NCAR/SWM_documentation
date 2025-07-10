# Build

## CMake Build Options

An overview of the available options is shown in the table below.

| Variable Name | Description                                    | Default | Possible values |
|---------------|---------------------------------------------   |---------|-----------------|
| SWM_DEVICE    | Select device type to build for                | cpu     | cpu or gpu      |
| SWM_C         | Enable the C versions of the mini-app          | ON      | ON or OFF       |
| SWM_FORTRAN   | Enable the Fortran versions of the mini-app    | ON      | ON or OFF       |
| SWM_AMREX     | Enable the AMReX version of the mini-app       | OFF     | ON or OFF       |
| SWM_OPENACC   | Enable the OpenACC versions of the mini-app    | OFF     | ON or OFF       |
| SWM_OPENMP    | Enable the OpenMP versions of the mini-app     | OFF     | ON or OFF       |
| SWM_MPI       | Enable the MPI versions of the mini-app        | OFF     | ON or OFF       |
| SWM_CUDA      | Enable the CUDA versions of the mini-app       | OFF     | ON or OFF       |
| AMReX_ROOT    | Path to AMReX library CMake configuration file. See below for details. | none    | 

The default options are set so that the project should build on any just about system that has CMake, a C compiler, and a Fortran compiler. As you turn more options to ON you may need to install additional dependencies on you system. Refer to the [repository tour](swm_repository_tour.md) page for more details on each version of the mini-app and its requirements.

### SWM_DEVICE
The option `SWM_DEVICE` controls the code will run on a cpu or gpu. This option means slightly different things depending on what version of the mini-app you are running.

The the options `SWM_C`, `SWM_FORTRAN`, and `SWM_AMREX` control building the code 

The AMReX versions of the SWM mini-app are linked against a CMake install of the AMReX library. The variable `AMReX_ROOT` should point to the directory containing the AMReXConfig.cmake file for your AMReX install. If you installed the AMReX library at `AMREX_INSTALL_DIR` then you should typically set `AMReX_ROOT=$AMREX_INSTALL_DIR/lib/cmake/AMReX`

It is possible to configure the options in a way that conflicts. If you do select an incompatible set up options then you should receive a warning or error when generating the cmake build. For example if you set `SWM_DEVICE=cpu` and `SWM_CUDA=ON` then CMake will generate a warning that CUDA cannot be run on the cpu, and will automatically set `SWM_CUDA=OFF`.

You can change the compilers used using the typical cmake variables:

| Variable Name | Description | Default | Possible values |
|---------------|-------------|---------|-----------------|
| CMAKE_C_COMPILER | Explicitly specify which C compiler to use. | Default controlled by CMake. If $CC is defined in your environment it will use that instead of the default. | gcc and nvc have been tested. Although this should work with just about any C compiler. |
| CMAKE_CXX_COMPILER | Explicitly specify which C++ compiler to use. | Default controlled by CMake. If $CXX is defined in your environment it will use that instead of the default. | g++ and nvc++ have been tested. Although this should work with just about any C compiler. |
| CMAKE_Fortran_COMPILER | Explicitly specify which Fortran compiler to use. | Default controlled by CMake. If $FC is defined in your environment it will use that instead of the default. | gfortran and nvfotran have been tested. Although this should work with just about any Fortran compiler. |

## AMReX CMake Build
This is a quick overview on the options we use when building the AMReX library to link with our mini-app. For more information on building AMReX with CMake see their [documentation page](https://amrex-codes.github.io/amrex/docs_html/BuildingAMReX.html#building-with-cmake).

AMReX CMake build options we always-use:

| Variable Name        | Value  |
|----------------------|--------|
| AMReX_SPACEDIM       | 2      |
| AMReX_PRECISION      | DOUBLE |
| AMReX_FORTRAN        | YES    |
| CMAKE_INSTALL_PREFIX | The path to where AMReX library will be installed. |

AMReX CMake build options we sometimes use depending on if you would like to build the mini-app for parallel cpu execution. For example if you plan on building the amrex version of the SWM mini-app with `SWM_MPI=ON` and/or `SWM_CUDA=ON` then you will need to enable those features when building the AMReX library.

| Variable Name | Value |
|---------------|-------|
| AMReX_MPI     | YES   |
| AMReX_OMP     | YES   |

If you are going to build the mini-app to run on NVIDIA GPUs (`SWM_DEVICE=gpu` and `SWM_CUDA=ON`) you will need to build AMReX with the following options: 

| Variable Name            | Value |
|--------------------------|-------|
| AMReX_GPU_BACKEND        | CUDA |
| AMReX_GPU_RDC            | YES (Maybe needs to be NO for the Fortran with OpenACC or OpenMP directives. I am still working on getting that build working) |
| AMReX_CUDA_ARCH          | Depends on the gpu you are using |
| AMReX_DIFFERENT_COMPILER | ON |

For more details on building AMReX for GPUs see their [documentation](https://amrex-codes.github.io/amrex/docs_html/GPU.html#building-with-cmake).

These AMReX options were on by default, however I don't think we are using any of these features so you can turn then off.

| Variable Name               | Value |
|-----------------------------|-------|
| AMReX_LINEAR_SOLVERS        | NO    |
| AMReX_LINEAR_SOLVERS_INCFLO | NO    |
| AMReX_LINEAR_SOLVERS_EM     | NO    |
| AMReX_AMRLEVEL              | NO    |
| AMReX_PARTICLES             | NO    |
| AMReX_TINY_PROFILE          | NO    |
