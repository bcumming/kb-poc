Uenv are user environments that provide scientific applications, libraries and tools on Alps. This article will explain how to find, dowload and use uenv on the command line, and how to enable them in SLURM jobs.

Uenv are typically application-specific, domain-specific or tool-specific - each uenv contains only what is required for the application or tools that it provides.

Each uenv is packaged in a single file (in the [Squashfs](https://docs.kernel.org/filesystems/squashfs.html) file format), that stores a compressed directory tree that contains all of the software, tools and other information like modules, required to provide a rich environment.

Each environment contains a software stack, comprised of compilers, libraries, tools and scientific applications, built using Spack.

!!! warning

    This documentation is for the new uenv2 implementation of uenv, that is not yet installed on Alps.

## Getting started

After logging into an Alps cluster, you can quickly check the availability of uenv with the following commands:

```terminal
> uenv status
there is no uenv loaded
> uenv --version
7.0.0
```

!!! howto "installing uenv"
    The command line tool can be installed from source, if you are working on a cluster that does not have uenv installed, or if you need to test a new version.

    ```bash title="manually installing uenv in the terminal"
    git clone https://github.com/eth-cscs/uenv2.git
    cd uenv2

    # run the installation script.
    # this will install uenv2 in $HOME/.local/$(uname -m)/
    ./install-alps-local.sh

    # update bashrc
    echo "export PATH=\$HOME/.local/\$(uname -m)/bin:\$PATH" >> $HOME/.bashrc
    echo "unset -f uenv" >> $HOME/.bashrc
    ```


    !!! warning
        Before uenv can be used, you need to log out then back in again and type "uenv --version" to verify that uenv has been installed.
        The version should be `6.0.0-dev` if succesfull.

## Naming uenv


Uenv are referred to using labels, where a label has the following form `name/version:tag@system%uarch`, for example `prgenv-gnu/24.11:v2@todi%gh200`.

#### `name`

the name of the uenv. In this case `prgenv-gnu`.

#### `version`

The version of the uenv. The format of `version` depends on the specific uenv.
Often they use the `yy.mm` format, though they may also use the version of the software being packaged.
For example the `namd/3.0.1` uenv packages version 3.0.1 of the popular [NAMD](https://www.ks.uiuc.edu/Research/namd/) simulation tool.

#### `tag`

Used to differentiate between _releases_ of a versioned uenv. Some examples of tags include:

* `rc1`, `rc2`: release candidates.
* `v1`: a first release typically made after some release candidates.
* `v2`: a second release, that might fix issues in the first release.

#### `system`

The name of the Alps cluster for which the uenv was built.

#### `uarch`

The node type (microarchitecture) that the uenv is built for:

| uarch | CPU | GPU | comment |
| ----- | --- | --- | ------- |
|[gh200]| 4 72-core NVIDIA Grace (`aarch64`) | 4 NVIDIA H100 GPUs | |
|[zen2] | 2 64-core AMD Rome (`zen2`)        | - | used in Eiger|
|[a100] | 1 64-core AMD Milan (`zen3`)       | 4 NVIDIA A100 GPUs | |
|[mi200]| 1 64-core AMD Milan (`zen3`)       | 4 AMD Mi250x GPUs  | |
| zen3  | 2 64-core AMD Milan (`zen3`)       | - | only in MCH system |

[gh200]: ../alps-systems/hardware.md#nvidia-gh200-gpu-nodes
[a100]: ../alps-systems/hardware.md#nvidia-a100-gpu-nodes
[zen2]: ../alps-systems/hardware.md#amd-rome-cpu-nodes
[mi200]: ../alps-systems/hardware.md#amd-mi250x-gpu-nodes

### using labels

The uenv command line has a flexible interface for describing

```bash
# search for all uenv on the current system that have the name prgenv-gnu
uenv image find prgenv-gnu

# search for all uenv with version 24.11
uenv image find /24.11

# search for all uenv with tag v1
uenv image find :v1

# seach for a specific version
uenv image find prgenv-gnu/24.11:v1
```

By default, the `uenv` filters results to only those 

```bash
# log into the eiger vCluster
ssh eiger

# this command will search for all pgrenv-gnu uenv on _eiger_
uenv image find prgenv-gnu

# use @ to search on a specific system, e.g. on daint:
uenv image find prgenv-gnu@daint

# this can be used to search for all uenv on another system:
uenv image find @daint

# the '*' is a wildcard used meaining "all systems"
# this will show all images on all systems
uenv image find @'*'

# search for all images on Alps that were built for gh200 nodes.
uenv image find @'*'%gh200
```

!!! note
    The wild card `*` used for "all systems" must always be escaped in single quotes: `@'*'`.

## Finding uenv

Uenv for programming environments, tools and applications are provided by CSCS on each Alps system.

!!! info
    The same uenv are not installed on every system. Instead uenv that are supported for the users of that platform are provided.

The available uenv images are stored in a registry, that can be queried using the `uenv image find`  command:

``` terminal title="uenv image find"
> uenv image find
uenv                       arch  system  id                size(MB)  date
cp2k/2024.1:v1             zen2  eiger   2a56f1df31a4c196   2,693    2024-07-01
cp2k/2024.2:v1             zen2  eiger   f83e95328d654c0f   2,739    2024-08-23
cp2k/2024.3:v1             zen2  eiger   7c7369b64b5fabe5   2,740    2024-09-18
editors/24.7:rc1           zen2  eiger   e5fb284962908eed   1,030    2024-07-18
editors/24.7:v2            zen2  eiger   4f0f2770616135b1   1,062    2024-09-04
julia/24.9:v1              zen2  eiger   0ff97a74dfcaa44e     539    2024-11-09
linalg/24.11:rc1           zen2  eiger   b69f4664bf0cd1c4     770    2024-11-20
linalg/24.11:v1            zen2  eiger   c11f6c85028abf5b     776    2024-12-03
linalg-complex/24.11:v1    zen2  eiger   846a04b4713d469b     792    2024-12-03
linaro-forge/24.0.2:v1     zen2  eiger   65734ce35494a5f5     313    2024-07-18
linaro-forge/24.1:v1       zen2  eiger   b65d7c85adfb317a     344    2024-11-27
netcdf-tools/2024:v1       zen2  eiger   e7e508c34cf40ccd   3,706    2024-11-14
prgenv-gnu/24.11:rc4       zen2  eiger   811469b00f030493     570    2024-11-21
prgenv-gnu/24.11:v1        zen2  eiger   0b6ab5fc4907bb38     572    2024-11-27
prgenv-gnu/24.7:v1         zen2  eiger   7f68f4c8099de257     478    2024-07-01
quantumespresso/v7.3.1:v1  zen2  eiger   61d1f21881a65578     864    2024-11-08
```

The output above shows that there are 12 uenv (`prgenv-gnu`, `namd` , `cp2k` and `arbor`).

## Downloading uenv

To use a uenv, it first has to be pulled from the registry to local storage where you can access it.
For example, to use the `prgenv-gnu` uenv, use the uenv image pull command:

```terminal title="uenv image pull"
# The following commands have the same effect

# method 1: pull using the name of the uenv
> uenv image pull prgenv-gnu/24.2:v1

# method 2: pull using the id of the image
> uenv image pull 3ea1945046d884ee
```

!!! note
    In order to pull images, a local directory for storing the images must first be created, and you will receive an error message.
    To create a repo in the default location, use the following command:

    ```terminal title="uenv image repo"
    > uenv repo create
    ```

Some images can be large, over 10 GB, and it can take a while to download from the registry.

To view all uenv that have been pulled, and are ready to use use the `uenv image ls` command:

```terminal title="listing downloaded uenv"
> uenv image ls
uenv/version:tag                        uarch date       id               size
netcdf-tools/2024:v1                    gh200 2024-04-04 499c886f2947538e 1.2GB
linaro-forge/23.1.2:latest              gh200 2024-04-10 ea67dbb33801c7c3 342MB
icon-wcp/v1:v3                          gh200 2024-03-11 3e8f96370a4685a7 8.3GB
icon-wcp/v1:latest                      gh200 2024-03-11 3e8f96370a4685a7 8.3GB
```

### Accessing restricted software

By default, uenv can be pulled by all users on a system, with no restrictions.

Some uenv are not available to all users, for exampl the vasp  images are only available for users who have a VASP license, who are added to the vasp6  group once then have provided CSCS with a copy of their license.

To be able to pull such images, you first need to configure the token for that specific software. This step only needs to be performed once - once set up you will only need to perform it again if the token is changed, or if you need to use a different token for another uenv.

```terminal title="using a token to access VASP"
cat /capstor/scratch/cscs/bcumming/tokens/vasp6 | /opt/cscs/uenv/libexec/uenv-oras login jfrog.svc.cscs.ch --username admin --password-stdin
```

!!! note
    As of 15 Oct 2024 , the only restricted software is VASP.
