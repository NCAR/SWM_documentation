# SWM Repository Tour

## High Level Overview
The [SWM](https://github.com/NCAR/SWM) repository is organized on the top level by the language that the mini-app is written in. There are versions of the mini-app written in C, Fortran, Python, AMRex, and Julia (NOTE: WE MAY DROP THE JULIA VERSION SOON). 

In most cases there are multiple versions of the mini-app written in that language. If so the language directory, SWM/swm_{c, fortran, python, amrex, etc...}, contains subdirectories for the different versions of the mini-app written in that language. If there are any files used by multiple versions of the mini-app written in the same language then they are contained in a common directory.

For example, there are two versions of the mini-app written in the C programming language. One version only uses C and the other uses C with OpenACC directives for loop parallelization. There are also some files shared among both versions. In this case the directory structure is as follows:

| Directory                 | Contents                                                   |
|---------------------------|------------------------------------------------------------|
| $SWM_ROOT/swm_c/common    | Files used by both C versions of the mini-app              |
| $SWM_ROOT/swm_c/c         | Files only used by the plain C version of the mini-app     |
| $SWM_ROOT/swm_c/c_OpenACC | Files only used by the C + OpenACC version of the mini-app |

>Throughout this document `SWM_ROOT` will refer to the directory were you pulled the SWM repository.

Build instructions for the for the compiled versions of the mini-app (C, Fortran, and AMReX) are [here](build.md). Some of the following sections mention build flags covered on that page.

## C

### C
**Summary:**
A serial version of the mini-app written in C. 

**Build:**
To build this version of the mini-app your CMake build should use:

| Variable Name | Possible Values |
|---------------|-----------------|
| SWM_C         | ON              |
| SWM_DEVICE    | cpu             |

The CMake build will build an executable at: 
```bash
$SWM_BUILD_DIR/swm_c/c/swm_c
```

**Run:**
No command line arguments or any other configuration required. Just run the executable produced in the previous step. The problem inputs (mesh size, number of iterations, etc...) are hardcoded at compile time in the file `$SWM_ROOT/swm_c/c/shallow_swap.c`.


### C with OpenACC
**Summary:**
A parallel version of the mini-app written in C with OpenACC directives to parallelize several key loops. 

**Build:**
To build this version of the mini-app your CMake build should use:

| Variable Name | Possible Values |
|---------------|-----------------|
| SWM_C         | ON              |
| SWM_OPENACC   | ON              |
| SWM_DEVICE    | cpu or gpu      |

The CMake build will produce two executables at: 
```bash
$SWM_BUILD_DIR/swm_c/c_OpenACC/swm_c_acc
$SWM_BUILD_DIR/swm_c/c_OpenACC/swm_c_acc_tile
```

**Additional Dependencies and/or Restrictions:**
The CMake build currently only works with the GNU and Nvidia compilers. 
>TODO: Adding additional compiler support is as simple as adding the OpenACC flags for your compiler to the CMake build.

**Run:**
No command line arguments or any other configuration required. Just run the executables produced in the previous step. The problem inputs (mesh size, number of iterations, etc...) for the executables are hardcoded at compile time in the files `$SWM_ROOT/swm_c/c_OpenACC/shallow.swap.acc.c` and `$SWM_ROOT/swm_c/c_OpenACC/shallow.swap.acc.Tile.c`.

## Fortran

### Fortran
**Summary:**
A serial version of the mini-app written in Fortran. 

**Build:**
To build this version of the mini-app your CMake build should use:

| Variable Name | Possible Values |
|---------------|-----------------|
| SWM_FORTRAN   | ON              |
| SWM_DEVICE    | cpu             |

The CMake build will produce an executable at: 
```bash
$SWM_BUILD_DIR/swm_fortran/fortran/swm_fortran
```

**Run:**
No command line arguments or any other configuration required. Just run the executable produced in the previous step. The problem inputs (mesh size, number of iterations, etc...) for the executables are hardcoded at compile time and defined the file `$SWM_ROOT/swm_fotran/common/params.F90`.

### Fortran with OpenACC
**Summary:**
A parallel version of the mini-app written in Fortran with OpenACC directives to parallelize the compute heavy loops.

**Build:**
To build this version of the mini-app your CMake build should use:

| Variable Name | Possible Values |
|---------------|-----------------|
| SWM_FORTRAN   | ON              |
| SWM_OPENACC   | ON              |
| SWM_DEVICE    | cpu or gpu      |

The CMake build will produce an executable at: 
```bash
$SWM_BUILD_DIR/swm_fortran/fortran_OpenACC/swm_fortran_acc
```

**Additional Dependencies and/or Restrictions:**
The CMake build currently only works with the GNU and Nvidia compilers. 
>TODO: Adding additional compiler support is as simple as adding the OpenACC flags for your compiler to the CMake build. 

**Run:**
No command line arguments or any other configuration required. Just run the executable produced in the previous step. The problem inputs (mesh size, number of iterations, etc...) for the executables are hardcoded at compile time and defined the file `$SWM_ROOT/swm_fotran/common/params.F90`.


## AMReX
The AMReX versions of the mini-app are really written in C++ but they use the AMReX library extensively. So we have decided to group all of the AMReX versions together in their own "language" directory.


### AMReX
**Summary:**
A version of the mini-app written in C++ and uses the AMReX library. This version has a large number of configuration options. It is able to run on in serial or parallel on cpus and gpus using a number of different programming model backends (MPI, OpenMP, CUDA) independently or or in combination with each other. 

**Build:**
To build this version of the mini-app your CMake build should use:

| Variable Name | Possible Values |
|---------------|-----------------|
| SWM_AMREX     | ON              |
| SWM_OPENMP    | ON or OFF       |
| SWM_MPI       | ON or OFF       |
| SWM_CUDA      | ON or OFF       |
| SWM_DEVICE    | cpu or gpu      |

The CMake build will produce an executable whose name depends on the options you used: 
```bash
$SWM_BUILD_DIR/swm_amrex/swm_AMReX/swm_amrex[_OMP][_MPI][_CUDA]
```
where the text inside of the square brackets will only appear if the corresponding option was set to ON.

To run on the GPU you need to set SWM_DEVICE=gpu and SWM_CUDA=ON. If you want to run on multiple GPUs then you must also set SWM_MPI=ON. Note that setting SWM_OPENMP=ON only applies to spawning treads on the cpu and does not offload onto the gpu.


**Additional Dependencies and/or Restrictions:**
TODO: Go into details about AMReX build options and how they must be enabled to correspond to these features. 
OPENMP
MPI
CUDA - Mention only works with nvidia cuda compiler
TODO: Mention conflict between cpu and cuda. 

Give example of AMReX Cmake Build that we can link against depending on the options you choose.

**Run:**
The executable expects an input file to be passed as the first commandline argument. The input file defines the mesh size, number of iterations, and several other key parameters. An example input file is provided at `$SWM_ROOT/swm_amrex/common/inputs`. For example to run the executable:
```bash
$SWM_BUILD_DIR/swm_amrex/swm_AMReX/swm_amrex[_OMP][_MPI][_CUDA] $SWM_ROOT/swm_amrex/common/inputs
```

If you enabled the option for OpenMP you can control the number of threads used just as you would for any other OpenMP executable, via the OMP_NUM_THREADS environment variable. For example to run using four threads:
```bash
env OMP_NUM_THREADS=4 $SWM_BUILD_DIR/swm_amrex/swm_AMReX/swm_amrex_OMP $SWM_ROOT/swm_amrex/common/inputs
```

If you enabled MPI then you can launch the executable just as you would any other MPI executable on your system. For example to run on 4 ranks:
```bash
mpirun -np 4 $SWM_BUILD_DIR/swm_amrex/swm_AMReX/swm_amrex_MPI $SWM_ROOT/swm_amrex/common/inputs
```

If you are using both OpenMP and MPI you can combine the previous two commands to control how many OpenMP treads each MPI rank uses. For example to run on 4 ranks, where each rank uses 2 treads:
```bash
env OMP_NUM_THREADS=2 mpirun -np 4 $SWM_BUILD_DIR/swm_amrex/swm_AMReX/swm_amrex_MPI $SWM_ROOT/swm_amrex/common/inputs
```

If you want to run on multiple GPUs you must set SWM_MPI=ON, SWM_CUDA=ON, and set the number of ranks to correspond the the number of gpus you want to use. For example to run on 4 gpus:
```bash
mpirun -np 4 $SWM_BUILD_DIR/swm_amrex/swm_AMReX/swm_amrex_MPI_CUDA $SWM_ROOT/swm_amrex/common/inputs
```

### AMReX Driver + Fortran Kernels
**Summary:**
This version of the code uses the same AMReX driver described in the previous section but the kernels inside of the loops are written in fortran instead of C++. The build and run instructions are the same as the previous section.

TODO: THIS VERSION OF THE CODE NEEDS TO BE UPDATED. ITS IS STILL USING THE THE C++ KERNELS INSTEAD OF THE FORTRAN KERNELS. COME BACK AND UPDATE THIS SECTION AS NEEDED AFTER DOING THAT.

### AMReX Driver + Fortran Subroutines with OpenACC
**Summary:**
This version of the mini-app uses an AMReX driver written in C++ and calls Fortran subroutines that are offloaded the compute heavy loop onto the GPUs using OpenACC directives.

**Build:**
To build this version of the mini-app your CMake build should use:

| Variable Name | Possible Values |
|---------------|-----------------|
| SWM_AMREX     | ON              |
| SWM_OPENACC   | ON              |
| SWM_CUDA      | ON              |
| SWM_DEVICE    | gpu             |

You will also need to build using the nvidia compiler stack. Typically by setting:

| Variable Name          | Possible Values |
|------------------------|-----------------|
| CMAKE_C_COMPILER       | nvc             |
| CMAKE_CXX_COMPILER     | nvc++           |
| CMAKE_Fortran_COMPILER | nvfortan        |

**Additional Dependencies and/or Restrictions:**
TODO:

**Run:**
TODO:

### AMReX Driver + Fortran Subroutines with OpenMP
**Summary:**
This version of the mini-app uses an AMReX driver written in C++ and calls Fortran subroutines that are offloaded the compute heavy loop onto the GPUs using OpenMP directives.

**Build:**
TODO:

**Additional Dependencies and/or Restrictions:**
TODO:

**Run:**
TODO:

## Python
### Python + NumPy
TODO: Add instruction

### Python + GT4Py
TODO: Add instruction

## Julia
TODO: WE MIGHT REMOVE THE JULIA VERSION. REVISIT THIS DOCUMENTATION PAGE AFTER A DECISION