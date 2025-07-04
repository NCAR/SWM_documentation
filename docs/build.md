# Build

## CMake Build Options

An overview of the available options is shown in the table below.

| Variable Name | Description                                 | Default | Possible values |
|---------------|---------------------------------------------|---------|-----------------|
| SWM_DEVICE    | Select device type to build for             | cpu     | cpu or gpu      |
| SWM_C         | Enable the C versions of the mini-app       | ON      | ON or OFF       |
| SWM_FORTRAN   | Enable the Fortran versions of the mini-app | ON      | ON or OFF       |
| SWM_AMREX     | Enable the AMReX version of the mini-app    | OFF     | ON or OFF       |
| SWM_OPENACC   | Enable the OpenACC versions of the mini-app | OFF     | ON or OFF       |
| SWM_OPENMP    | Enable the OpenMP versions of the mini-app  | OFF     | ON or OFF       |
| SWM_MPI       | Enable the MPI versions of the mini-app     | OFF     | ON or OFF       |
| SWM_CUDA      | Enable the CUDA versions of the mini-app    | OFF     | ON or OFF       |

The default options are set so that the project should build on any just about system that has CMake, a C compiler, and a Fortran compiler. As you turn more options to ON you may need to install additional dependencies on you system. Refer to the [repository tour](swm_repository_tour.md) page for more details on each version of the mini-app and its requirements.

### SWM_DEVICE
The option `SWM_DEVICE` controls the code will run on a cpu or gpu. This option means slightly different things depending on what version of the mini-app you are running.

The the options `SWM_C`, `SWM_FORTRAN`, and `SWM_AMREX` control building the code 



It is possible to configure the options in a way that conflicts. If you do select an incompatible set up options then you should receive a warning or error when generating the cmake build. For example if you set `SWM_DEVICE=cpu` and `SWM_CUDA=ON` then CMake will generate a warning that CUDA cannot be run on the cpu, and will automatically set `SWM_CUDA=OFF`.

| Variable Name | Description | Default | Possible values |
|---------------|-------------|---------|-----------------|
| CMAKE_C_COMPILER | Explicitly specify which C compiler to use. | Default controlled by CMake. If $CC is defined in your environment it will use that instead of the default. | gcc and nvc have been tested. Although this should work with just about any C compiler. |
| CMAKE_CXX_COMPILER | Explicitly specify which C++ compiler to use. | Default controlled by CMake. If $CXX is defined in your environment it will use that instead of the default. | g++ and nvc++ have been tested. Although this should work with just about any C compiler. |
| CMAKE_Fortran_COMPILER | Explicitly specify which Fortran compiler to use. | Default controlled by CMake. If $FC is defined in your environment it will use that instead of the default. | gfortran and nvfotran have been tested. Although this should work with just about any Fortran compiler. |

Another known limitation is that if you want to build swm_amrex_fsubroutine versions (OpenACC and OpenMP) to target the gpu you need to use the nvhpc compilers and have 

Currently the only OpenMP, MPI, and CUDA versions of the mini-app use amrex. 