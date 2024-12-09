# Debugging with Linaro DDT

The [Linaro Forge DDT] debugging tool can be used to debug serial,
multi-threaded (OpenMP), multi-process (MPI), accelerator based (Cuda) codes
written in Fortran, C/C++ and Python. 

!!! tip
    Before using the debugger, the code source must be compiled with the -g
    (for cpu) and -G (for nvcc) debugging flags.

[Linaro Forge DDT]: https://docs.linaroforge.com/latest/html/forge/index.html

!!! info GPU debugging support
    Forge can only be used at CSCS with NVIDIA GPUs.
    CSCS does not have a license to debug codes on AMD GPUs.

### UENV support

The documentation for using DDT with UENV images can be found in
[uenv-linaro-forge]

[uenv-linaro-forge]: https://eth-cscs.github.io/alps-uenv/uenv-linaro-forge/

### Latest features

#### Node memory threshold detection

Running out of memory often causes a job to be killed instantly with no further
debugging or diagnostic information available. To detect potential out of
memory errors early, enable `Set node memory threshold` ([doc](https://docs.linaroforge.com/24.1/html/forge/ddt/memory_debugging_ddt/node_memory_threshold_detection.html)).

<figure markdown="span">
  ![DDT](https://raw.githubusercontent.com/jgphpc/screenshots/refs/heads/main/ddt/mem_threshold_setup.png){ width=300 }
  <figcaption>Memory threshold detection: setup</figcaption>
</figure>

<figure markdown="span">
  ![DDT](https://raw.githubusercontent.com/jgphpc/screenshots/refs/heads/main/ddt/mem_threshold_stop.png){ width=300 }
  <figcaption>Memory threshold detection: pausing execution</figcaption>
</figure>

<figure markdown="span">
  ![DDT](https://raw.githubusercontent.com/jgphpc/screenshots/refs/heads/main/ddt/mem_threshold_stats.png){ width=300 }
  <figcaption>Memory threshold detection: statistics</figcaption>
</figure>
