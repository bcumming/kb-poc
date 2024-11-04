Uenv are user environments that provide scientific applications, libraries and tools on Alps. This article will explain how to activate uenv, use them to build software and how to enable them in SLURM jobs.

Unlike the standard Cray Programming Environment (CPE), which aims to provide a complete software stack that can be configured to build all types of software on all node architectures, uenv are typically application-specific, domain-specific or tool-specific - each uenv contains only what is required for the application or tools that it provides.

User environments are packaged in a single file (in the  SquashFS file format), that stores a compressed directory tree that contains all of the software, tools and other information like modules, required to provide a rich environment.

Each environment contains a software stack, comprised of compilers, libraries, tools and scientific applications, built using Spack.

??? info "build your own uenv"
    For more information on how CSCS builds user environments, have a look at the documentation for the Stackinator tool that CSCS use to configure Spack to build environments with optimisations for the Alps architecture.

    This tool can also be used by motivated users to provide their own software stacks.

## Getting started

After logging into an Alps cluster, you can quickly check the availability of uenv with the following commands:

```terminal
> uenv status
there is no uenv loaded
> uenv --version
5.1.0
```

!!! howto "installing uenv"
    The command line tool can be installed from source, if you are working on a cluster that does not have uenv installed, or if you need to test a new version.

    ```terminal title="manually installing uenv in the terminal"
    > git clone https://github.com/eth-cscs/uenv.git
    > cd uenv
    # run the installation script.
    # if this is the first time that you are installing uenv,
    # type "yes" or "y" when prompted whether to update your ~/.bashrc file.
    > ./install --local
    ```


    !!! warning
        Before uenv can be used, you need to log out then back in again and type "uenv --version" to verify that uenv has been installed.

## Finding uenv

Uenv for programming environments, tools and applications are provided by CSCS on each Alps system.

!!! info
    The same uenv are not installed on every system. Instead uenv that are supported for the users of that platform are provided.

The available uenv images are stored in a registry, that can be queried using the `uenv image find`  command:

``` terminal title="uenv image find"
> uenv image find
uenv/version:tag                        uarch date       id               size
prgenv-gnu/24.2:v1                      gh200 2024-05-23 3ea1945046d884ee 3.7GB
prgenv-gnu/24.2:latest                  gh200 2024-05-23 3ea1945046d884ee 3.7GB
namd/3.0b6:v1                           gh200 2024-05-29 e8606d64eb8dcd2f 3.7GB
namd/3.0b6:latest                       gh200 2024-05-29 e8606d64eb8dcd2f 3.7GB
cp2k/2024.1:v1                          gh200 2024-05-29 af81b3bbbe68a62f 3.9GB
cp2k/2024.1:latest                      gh200 2024-05-29 af81b3bbbe68a62f 3.9GB
arbor/v0.9:v1                           gh200 2024-05-23 69fdbd75e246d439 3.6GB
```

The output above shows that there are four uenv (`prgenv-gnu`, `namd` , `cp2k` and `arbor`).

The full name of a uenv is of the form `uenv/version:tag`

* `uenv`: is the name of the uenv.
* `version`: the version of the uenv, typically a in the form `year`, `year.month` or matching the version of the software package provided by the uenv (e.g version `3.0b6` of NAMD above)
* `tag`: incremental tag applied to versions when they are deployed with an update, e.g v1 , v2  or latest .

The id  of the image is a unique identifier derived from the sha256 hash of the image.

!!! note
    The full name and id can both be used to refer to a specific uenv.

## Downloading uenv

To use a uenv, it first has to be pulled from the registry to local storage where you can access it.
For example, to use the `prgenv-gnu` uenv, use the `uenv image pull` command:

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
