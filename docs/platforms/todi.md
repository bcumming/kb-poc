# Todi

Todi is the first early access system with Grace-Hopper (GH200) nodes on Alps that is opened to the CSCS user community.
To access the system projects need to have applied for special access.

It is available for all three platforms: HPCp, MLp and CWp.

!!! warning
    Todi is an early access system and represents a work in progress.
    CSCS engineers are working continuously on the system to improve the quality of service.
    Accounting is not enabled, and users are expected to perform their tests and benchmarks with care.

    We ask that you report any issues, and ask any questions, on the [CSCS Service Desk].

## Cluster Details

__TODO__: a standardised table with information about

* number and type of nodes

and any special notes

### Software and services

__TODO__ information about CSCS services/tools available

* container engine
* uenv
* CPE
* ... etc

### Storage

__TODO__: standardised format for

* attached storage and policies

### Slurm

__TODO__: standardised format for

* slurm queues

## Calendar and key events

Todi will be deployed in several stages.
    * Early Access: during early access Todi will provide 1 login node and 17 GH200 compute nodes
    * Scale out: between 15 June 2024 -31 Jul 2024 Todi will be scaled up to full production size.
    * From 20 Aug 2024 Todi will be returned to early access users with ca. 825 compute nodes; vClusters for the various platforms (High Performance Computing Platform, Machine Learning Platform and Weather and Climate Platform) will then be spun out from this system.


__TODO__: a calendar widget would be useful, particularly if we can have a central calendar, and a way to filter events for specific instances

## Change log

!!! change "special text boxes for updates"
    they can be opened and closed.

!!! change "2024-10-15 reservation `daint` available again"
    The reservation daint  is available again exclusively for Daint users that need to run their benchmarks for submitting their proposals, additionally to the debug  partition and free nodes.
    Please add the Slurm option --reservation=daint to your batch script if you want to use it

??? change "2024-10-07 New compute node image deployed"
    New compute node image deployed to fix the issue with GPU-aware MPI.

    Max job time limit is decreased from 12 hours to 6 hours

??? change "2024-09-18 Daint users"
    In order to complete the preparatory work necessary to deliver Alps in production, as of September 18 2024 the vCluster Daint on Alps will no longer be accessible until further notice: the early access will still be granted on Tödi using the Slurm reservation option `--reservation=daint`

## Known issues

__TODO__ list of know issues - include links to known issues page

[CSCS Service Desk]: https://jira.cscs.ch/plugins/servlet/desk
