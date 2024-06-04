# Cluster Introduction

## Purpose

The cluster was purchased by the Computer Systems Lab to serve the Parallel Computing and Computer Vision classes, but is available for usage by all TJ students and staff. Several senior research labs have expressed interest in using the cluster's resources for their own purposes. Academic jobs run on the cluster will receive priority allocation of resources, but non-academic jobs are accepted as well as long as they abide by the FCPS Acceptable Use Policy (Regulation 6410).

## Specifications

The full cluster consists of 12 HPC cluster nodes, 40 Borg nodes, and 1 dedicated GPU node (zoidberg). This setup occupies almost 3 full racks in the server room. The CSL obtained the 40-node Borg cluster from NASA through an educational grant. The Borg nodes are named borg\[1-40] consecutively and the HPC nodes are named hpc\[1-12]. The login node is `infocube`

## Cluster Nodes

{% hint style="danger" %}
The **recommended** method of running jobs on the Cluster is through [Slurm](using-infocube.md)
{% endhint %}

All nodes on the cluster are directly accessible through `ssh` as well as Slurm. However, due to unfinished maintenance caused by COVID-19, some nodes are inaccessible indefinitely.

#### Available Cluster Nodes

Format: \<prefix>\[sequence]. (e.g borg\[1-3] means `borg1`, `borg2`, and `borg3` are available)

* borg\[1-3], borg\[6-31], borg40
* hpc\[1-6], hpc\[8-12], zoidberg
* snowy

#### Unavailable Cluster Nodes

* borg\[4-5], borg\[32-38]
* hpc7
* duke

An always up-to-date version of this list can be viewed by running `sinfo` on `infocube` or an available node.

### SSH Access

You can directly access any available cluster node through `ssh`. To `ssh` into a cluster node:

1. `ssh` into `remote.tjhsst.edu` using your TJCSL username and password
   * `ssh 2021abagali1@remote.tjhsst.edu`
2. `ssh` into a cluster node
   * `ssh borg1` (or any available node) &#x20;

## File Management

The login node and all cluster nodes use a file system that is separate from the `ras1`/`ras2`/workstation AFS directories. Instead all user files are stored in CephFS under the directory `/cluster`. For example the cluster files for `2021jdoe` would be in the directory `/cluster/2021jdoe`.&#x20;

When you log into `infocube` or any cluster node your cluster directory will be `/cluster/<username>`. In addition to the cluster nodes, you can access your cluster files on `ras1`/`ras2`, under the same directory.&#x20;

Note: on `ras` your `/cluster` directory is not your default directory and is separate from your default directory files. You may use `cp`,`mv`, or another utility for moving files back and forth from anywhere on `ras` to your `/cluster` directory. This feature is currently unavailable on workstations. If you wish to copy files from a workstation over to your `/cluster` directory you may use `sftp` or `scp` to copy your files from the workstation over to the target cluster node.

It is safe to assume use of the keywords: "default directory", "home directory", "homedir", "cluster directory", etc while in the context of the Cluster refer to the directory at `/cluster/<username>`

## Slurm

Speaking of SLURM (the Simple Linux Utility for Resource Management), Slurm is the utility used for job control and submission. Users log in to `infocube`, run some simple commands, specifying what they want to run, how many resources it should have, priority, and other optional arguments, and SLURM takes care of allocating cluster resources for them, and provides job accounting so users know the status of their jobs. More information at our [Slurm docs](using-infocube.md).
