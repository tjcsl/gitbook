# Cluster Introduction

## Purpose

The cluster was purchased by the Computer Systems Lab to serve the Parallel Computing and Computer Vision classes, but is available for usage by all TJ students and staff. Several senior research labs have expressed interest in using the cluster's resources for their own purposes. Academic jobs run on the cluster will receive priority allocation of resources, but non-academic jobs are accepted as well as long as they abide by the FCPS Acceptable Use Policy (Regulation 6410).

## Specifications

The full cluster consists of 12 HPC cluster nodes, 40 Borg nodes, and 1 dedicated GPU node (zoidberg). This setup occupies almost 3 full racks in the server room. The CSL obtained the 40-node Borg cluster from NASA through an educational grant. The Borg nodes are named borg\[1-40] consecutively and the HPC nodes are named hpc\[1-12]. The login node is `infoprism`



### SSH Access

You can directly access any available cluster node through `ssh`. To `ssh` into a cluster node:

1. `ssh` into `remote.tjhsst.edu` using your TJCSL username and password
   * `ssh 2021abagali1@remote.tjhsst.edu`
2. `ssh` into a cluster node
   * `ssh borg1` (or any available node) &#x20;



## Slurm

Speaking of SLURM (the Simple Linux Utility for Resource Management), Slurm is the utility used for job control and submission. Users log in to `infoprism`, run some simple commands, specifying what they want to run, how many resources it should have, priority, and other optional arguments, and SLURM takes care of allocating cluster resources for them, and provides job accounting so users know the status of their jobs. More information at our [Slurm docs](using-infoprism.md).
