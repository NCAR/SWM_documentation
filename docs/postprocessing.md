# Postprocessing

## AMReX Postprocess
- AMRex Solution Verification
    - TODO: Update for new comparison methods
    - One time setup for building AMReX provided tool to diff plotfiles (fcompare). Full instructions on how to build AMReX postprocessing tools [here](https://amrex-codes.github.io/amrex/docs_html/Post_Processing.html).
        - cd $AMREX_HOME/Tools/Plotfile/
        - make
            - This should produce an executable: 
                - $AMREX_HOME/Tools/Plotfile/fcompare.gnu.ex
            - Note the exact name will vary depending on the compiler you used. The above example is when gcc was used.
    - Now you can diff two plot files using the fcompare tool:
        -    $AMREX_HOME/Tools/Plotfile/fcompare.gnu.ex plotfile1 plotfile2

- python plot.py
    - Currently just generates images of plots the x velocity (u), y velocity (v), and pressure (p) for first and last time step. TODO - add check against the output from the other versions of the [mini-app](https://github.com/NCAR/SWM).