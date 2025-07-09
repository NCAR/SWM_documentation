# Quickstart

This quick start guide shows how to get the SWM code and and build the C and Fortran versions mini-app. The only required dependencies are:

- CMake
- C Compiler
- Fortran Compiler

For an overview of all available versions of the mini-app see the [swm repository tour](swm_repository_tour.md) page. 

## Step 1: Get the Code

The source code is available on GitHub at [NCAR SWM](https://github.com/NCAR/SWM). The recommended way to get it is:

```bash
git clone https://github.com/NCAR/SWM.git
```
Throughout this document `SWM_ROOT` will refer to the directory were you pulled the SWM repository. We recommend exporting `SWM_ROOT` as an environment variable in your shell so you can copy pase subsequent steps in this quickstart tutorial. In the bash shell you can do this with the command:
```bash
export SWM_ROOT=REPLACE_WITH_PATH_TO_SWM_REPO
```

> Some of the provided build/run scripts in the SWM repository also use the `SWM_ROOT` variable.

## Step 2: Generate the Build Directory 
CMake is the recommend way to build the various versions of the SWM mini-app. The following commands generate a build that uses all default options. 

```bash
cmake -S $SWM_ROOT -B $SWM_BUILD_DIR
```

Where `SWM_ROOT` points to the directory where you pulled the SWM repository in the previous step and `SWM_BUILD_DIR` is a directory where the build will be generated. 

> You can place the build directory `SWM_BUILD_DIR` anywhere, but it is recommend to put it somewhere outside of the `SWM_ROOT` directory. That way the files generated in `SWM_BUILD_DIR` are independent of your source controlled `SWM_ROOT` directory. Hence you could delete `SWM_BUILD_DIR`, generate multiple builds with different options, or run the executables within a build without generating a lot of untracked files in your `SWM_ROOT` directory. I usually just put the build directory right next to the source directory with: 
>```bash
 export SWM_BUILD_DIR=$SWM_ROOT/../SWM_build
 ``` 

For more on the available SWM CMake build options see the [build](build.md) page .

## Step 3: Build
To build the code run the command: 
```bash
cmake --build $SWM_BUILD_DIR
```

>When we generated the build directory in the previous set we did not specify which build system to use. But most systems will use Make by default. If that is the case on your system then our build directory should have a top level makefile that can be used to build all the executables. So an alternative way to build the code is: 
>```bash
 cd $SWM_BUILD_DIR
 make
 ```
> You can also build each version of the mini-app by name from the top level makefile. For example to build only the c version of the mini-app:
>```bash
 cd $SWM_BUILD_DIR
 make swm_c
 ```
> Each subdirectory in `SWM_BUILD_DIR` that generates an executable also has a makefile that we can run. So another way to build only the c version of the mini-app is: 
>```bash
 cd $SWM_BUILD_DIR/swm_c/c
 make
 ```

## Step 4: Run
There should now be two executables in `SWM_BUILD_DIR`. The C and Fortran versions of the mini-app do not take any commandline arguments or input files. You can run them via:

```bash
$SWM_BUILD_DIR/swm_c/c/swm_c
$SWM_BUILD_DIR/swm_fortran/fortran/swm_fortran
```

## Step 5: Solution Verification
TODO
