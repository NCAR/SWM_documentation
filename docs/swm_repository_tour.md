# SWM Repository Tour

## High Level Overview
The [SWM](https://github.com/NCAR/SWM) repository is organized on the top level by the language that the mini-app is written in. There are versions of the mini-app written in C, Fortran, Python, AMRex, and Julia (NOTE: WE MAY DROP THE JULIA VERSION SOON). 

Then in each language directory SWM/swm_{c, fortran, python, amrex, etc...} there are subdirectors for the different versions of the mini-app written in that language. For example a version of the mini-app written C with OpenACC is in the directory SWM/swm_c/c_OpenACC.


Build instructions for the for the compiled versions of the mini-app (C, Fortran, and AMReX) are [here](build.md). Some of the below sections for the compiled languages mention build flags covered on that page.


## C

### Plain C
A serial version of the code written in C. 

To build this version of the mini-app your CMake build should use:

| Variable Name | Possible Values |
|---------------|-----------------|
| SWM_C         | ON              |
| SWM_DEVICE    | cpu             |


### C with OpenACC
A parallel version of the mini app written in C with OpenACC directives to parallelize the compute heavy loops. 

To build this version of the mini-app your CMake build should use:

| Variable Name | Possible Values |
|---------------|-----------------|
| SWM_C         | ON              |
| SWM_OPENACC   | ON              |
| SWM_DEVICE    | cpu or gpu      |

Currently only the GNU and Nvidia compilers are supported. 

## Fortran

### Plain Fortran
A serial version of the code written in Fortran. 

To build this version of the mini-app your CMake build should use:

| Variable Name | Possible Values |
|---------------|-----------------|
| SWM_FORTRAN   | ON              |
| SWM_DEVICE    | cpu             |

### Fortran with OpenACC
A parallel version of the mini app written in Fortran with OpenACC directives to parallelize the compute heavy loops.

To build this version of the mini-app your CMake build should use:

| Variable Name | Possible Values |
|---------------|-----------------|
| SWM_FORTRAN   | ON              |
| SWM_OPENACC   | ON              |
| SWM_DEVICE    | cpu or gpu      |

Dependencies:
Currently only the GNU and Nvidia compilers are supported. 

## AMReX
The AMReX versions of the mini-app are really written in C++ but they use the AMReX library extensively. So all the AMReX versions are grouped together in their own directory.

### Plain AMReX

| Variable Name | Possible Values |
|---------------|-----------------|
| SWM_AMREX     | ON              |
| SWM_OPENMP    | ON or OFF       |
| SWM_MPI       | ON or OFF       |
| SWM_CUDA      | ON or OFF       |
| SWM_DEVICE    | cpu or gpu      |

Dependencies:
TODO: Go into details about AMReX build options and how they must be enabled to correspond to these features. 
OPENMP
MPI
CUDA - Mention only works with nvidia cuda compiler

Give example of AMReX Cmake Build that we can link against depending on the options you choose.

Restrictions:
TODO: Mention conflict between cpu and cuda. 

### AMReX Driver + Fortran Kernels
TODO: THIS VERSION OF THE CODE NEEDS TO BE UPDATED. ITS IS STILL USING THE THE C++ KERNELS INSTEAD OF THE FORTRAN KERNELS.

### AMReX Driver + Fortran Subroutines

#### AMReX Driver + Fortran Subroutines with OpenACC

| Variable Name | Possible Values |
|---------------|-----------------|
| SWM_AMREX     | ON              |
| SWM_FORTRAN   | ON              |
| SWM_OPENACC   | ON              |
| SWM_DEVICE    | cpu or gpu      |


## Python
### Python + NumPy
TODO: Update for d the GridTools 

### Python + GT4Py
TODO: Update for d the GridTools 

## Julia
TODO: WE MIGHT REMOVE THE JULIA VERSION. REVISIT THIS DOCUMENTATION PAGE AFTER A DECISION