Uenv are user environments that provide scientific applications, libraries and tools on Alps. This article use them to build software.

For more documentation on how to find, download and use uenv in your workflow, see the [env tool documentation](../tools/uenv.md).

## Building software using Spack

The sofware in uenv is built with [Spack] using the [Stackinator] tool.
Therefore, a uenv is tightly coupled with [Spack] and can be used as an upstream [Spack] instance.

!!! tip
    See [Chaining Spack Installations] for more details about how upstream Spack instances work.
    You do not need to have a deep understanding of this if you use the uenv-spack tool.

CSCS provides `uenv-spack`, a tool that can be used to quickly install software using the software and configuration provided inside a uenv.

!!! note
    The Spack configuration inside each uenv contains information that Spack uses to customise packages for the Cray EX interconnect.
    Using the uenv-spack tool ensures that Spack uses this information when building your software packages.

### Installing `uenv-spack`

```bash
# download the uenv-spack tool
git clone https://github.com/eth-cscs/uenv-spack.git

# initialise the tool
(cd uenv-spack && ./bootstrap)

export PATH=$PWD/uenv-spack:$PATH
```

### Select The uenv

In this example we use the `prgenv-gnu` uenv, which provides a selection of commonly used HPC tools and libraries, including `cray-mpich`, `gcc`, `python`, `fftw`.

??? example "How to download the uenv"
    ```bash
    # list all releases of the November 2024 version, then pull down the desired version
    uenv image find prgenv-gnu/24.11
    uenv image pull prgenv-gnu/24.11:v1
    ```

To use Spack as an upstream, the uenv has to be started with the `spack` view:

```bash
uenv start prgenv-gnu/24.11:v1 --view=spack
```

??? note "what does the `spack` view do?"
    The `spack` view simply sets some environment variables that provide information about the version of Spack that was used to build the uenv, and where the uenv Spack configuration is stored.

    This information is useful because it is strongly recomended that you use the same version of Spack to build software on top of the uenv.

    | variable | example | description |
    | -------- | ------- | ----------- |
    | `UENV_SPACK_CONFIG_PATH` | `user-environment/config` | the path of the upstream [spack configuration files]. |
    | `UENV_SPACK_REF`         | `releases/v0.23` | the branch or tag used - this might be empty if a specific commit of Spack was used. |
    | `UENV_SPACK_URL`         | `https://github.com/spack/spack.git` | The git repository for Spack - nearly always the main spack/spack repository. |
    | `UENV_SPACK_COMMIT`      | `c6d4037758140fe15913c29e80cd1547f388ae51` | The commit of Spack that was used |

### Create the build path

Create a build path with a template `spack.yaml` and `repo`:
```
uenv-spack $SCRATCH/install/arbor --uarch=gh200
cd $SCRATCH/install/tools
./build
```

Create a build path and populate the `spack.yaml` file with some [specs]:
```
uenv-spack $SCRATCH/install/tools --uarch=gh200 \
           --specs="tree, screen, emacs +treesitter"
cd $SCRATCH/install/tools
./build
```

Create a build path and use a pre-configured `spack.yaml` and `repo`:
```
uenv-spack $SCRATCH/install/arbor --uarch=gh200 \
           --recipe=<path-to-recipe>
cd $SCRATCH/install/tools
./build
```

**todo** Create a build path and use your own `spack.yaml`:
```
uenv-spack $SCRATCH/install/arbor --uarch=gh200 \
           --recipe=<path-to-spack.yaml>
cd $SCRATCH/install/tools
./build
```


[Chaining Spack Installations]: https://spack.readthedocs.io/en/latest/chain.html
[Spack]: https://spack.readthedocs.io/en/latest/
[Spack Basic Usage]: https://spack.readthedocs.io/en/latest/basic_usage.html
[Stackinator]: https://eth-cscs.github.io/stackinator/
[Spack configuration files]: https://spack.readthedocs.io/en/latest/configuration.html
[spec]: https://spack.readthedocs.io/en/latest/basic_usage.html#sec-specs
[specs]: https://spack.readthedocs.io/en/latest/basic_usage.html#sec-specs
