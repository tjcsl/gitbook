---
description: Describe the CSL's clusters
---

# Cluster

## Purpose

The cluster was purchased by the Computer Systems Lab to serve the Parallel Computing and Computer Vision classes, but is available for usage by all TJ students and staff. Several senior research labs have expressed interest in using the cluster's resources for their own purposes. Academic jobs run on the cluster will receive priority allocation of resources, but non-academic jobs are accepted as well as long as they abide by the FCPS Acceptable Use Policy \(Regulation 6410\).

## Specifications

The HPC cluster currently \(as of 2016\) consists of four physical nodes \(three compute units and one GPU unit\), a top-of-rack switch, and two UPSes. This setup occupies half of a rack; the other half of the rack is currently unoccupied and left empty in case of future HPC cluster expansion. Compute units are currently named after [robots from Futurama](https://en.wikipedia.org/wiki/List_of_Futurama_characters%20). The name of the login virtual machine is a reference to [an episode of the series](https://en.wikipedia.org/wiki/The_Why_of_Fry) in which The Infosphere, a colossal memory bank, attempts to catalog all of the information in the universe.

### Uninterruptible Power Supplies \(UPSes\)

The HPC cluster rack currently has two UPSes, each with a maximum power capacity of 5000 Volt-Amps \(Watts\). The UPSes are APC brand, model number SRT5KRMXLT. They are online UPSes, which means they are continuously conditioning power, and there would be no delay in switching to battery if power were to fail, like with an offline UPS. One NEMA L6-30R 30A output from each UPS is connected to an APC PDU which runs on the side of the cluster rack. **The entire cluster rack operates at 240V, not 120V.**

### Top-of-rack Switch

\(most likely out of date, change later\)

One Juniper EX4300-48T-AFI 48-port Gigabit Ethernet switch, named [Imply](../../machines/switches/imply.md), operates as a top-of-rack switch for the cluster nodes and for the UPSes. The switch is managed and runs JUNOS. It has one EX-UM-4X4SFP 4x10GigabitEthernet uplink module, which connects the switch to the CSL core switch, [Xnor](../../machines/switches/xnor.md), over bonded, redundant 10GE fiber optic uplinks. The switch has one 350W power supply.

### Compute Units

Each compute unit is a 2U [Supermicro SuperServer 2027TR-HTRF](http://www.supermicro.com/products/system/2U/2027/SYS-2027TR-HTRF.cfm), with four hot-pluggable nodes, for a total of twelve logical compute nodes in the HPC cluster.

#### Power

Power is shared between the nodes in each unit. Each unit has two redundant 80 Plus Platinum 1620 Watt power supplies, which are connected to the _same UPS_. The reason for this is that if one of the UPSes were to fail or run out of power faster than the other UPS, one UPS \(with a maximum power capacity of 5000 Watts\) would not be able to sustain the entire cluster \(with a maximum power capacity of approximately 6900 Watts\) on its own. Therefore, the cluster units are distributed between the two UPSes, but are not redundant between the two.

#### Networking

Each node in each unit has independent networking. Each of the twelve nodes has two primary Gigabit Ethernet uplinks to the top-of-rack switch, as well as one uplink for the Supermicro Intelligent Platform Management Interface \(IPMI\) management port.

#### Processors and Memory

Each node in each unit has its own processors and memory. Each node has two Intel Xeon E5-2630v2 6-core 2.6GHz processors, for a total of 144 cores in the cluster. Each node also has 4x16GB Kingston DDR3 1600 ECC Registered memory units, for a total of 64GB of memory per node, for a total of 768GB of memory in the cluster.

#### Storage

Each node has independent storage, located on the front of the compute units. Each node has six hard drive bays, and two of these are filled per node with Seagate Constellation 2.5in 7.2k 250GB SATA HDDs, which are configured in a RAID1 array on each node. However, most cluster storage is located on the Sonic \(OUT OF DATE?\) disk array and mounted via NFS, since user storage must be shared between nodes.

### GPU Node

The cluster also contains one 1U [SuperMicro SuperServer 1028GQ-TRT](http://www.supermicro.com/products/system/1u/1028/SYS-1028GQ-TRT.cfm) which functions as a node for GPU-intensive processing. It contains two NVIDIA Tesla K80 GPU Accelerators, each with two Kepler GK210 GPUs. Each GK210 GPU has 12GB of GDDR5 memory and 2496 CUDA cores, for a total of 48GB GDDR5 and 9982 CUDA cores.

Aside from the GPUs, it has two Intel Xeon E5-2620 v3 8-core 2.4GHz processors and 4xKingston ValueRAM 16GB DDR4 ECC Registered memory modules, for a total of 64GB RAM. For power, it has two redundant 80-Plus Platinum 2000W power supplies, and for networking it has two primary 10GbE uplinks to the top-of-rack switch \(only operating at 1000Gbase-T\) and one IPMI management port uplink to the top-of-rack switch.

## Configuration

The cluster's infrastructure is managed using the Ansible configuration management system. The Ansible plays are located in the [Ansible repository on GitLab](https://gitlab.tjhsst.edu/sysadmins/ansible), under the files `hpc.yml`, `hpcgpu.yml`, `ibm.yml`, `ibmgpu.yml`, and `clustermaster.yml`.

The [CephFS](../../technologies/storage/ceph/cephfs.md) mount `/cluster` contains user home directories, which are shared between cluster nodes and the login VM. Users are expected to login to the login VM \(infosphere\) to run jobs using SLURM.

Speaking of SLURM \(the Simple Linux Utility for Resource Management\), Slurm is the utility used for job control and submission. Users log in to infosphere, run some simple commands, specifying what they want to run, how many resources it should have, priority, and other optional arguments, and SLURM takes care of allocating cluster resources for them, and provides job accounting so users know the status of their jobs. More information at our [Slurm docs](slurm.md).

