# Zoidberg

**Zoidberg** is part of the [HPC Cluster](./) in name and location only. It's treated as mostly separate, and can be used outside of [Slurm](../../services/cluster/slurm.md). Zoidberg is by far the most powerful HPC node as well, potentially the most powerful machine in the entire CSL.

## Technical Specifications

| **Specification** | Description |
| :--- | :--- |
| **Server Type** | SuperMicro 1U 4 GPU SuperServer |
| **CPU** | Intel\(R\) Xeon\(R\) E5-2630 v2 @ 2.60GHz |
| **GPU** | 4x NVIDIA Tesla K80 |
| **RAM** | 64 GB |
| **Hard Disks** | 2x 250GB RAID 1 |
| **OS** | CentOS 7 |
| **Purchase Date** | 2016 |

## Trivia

Zoidberg currently has a [workstation](../../services/workstations/) IP and is plugged into the workstation [VLAN](../../technologies/osi-model.md) by means of a long ethernet cable because the networking to the HPC cluster is broken \("it's fine if they can't DHCP" no it friggin isn't "well if they don't have static IPs then they're mis-managed" seriously?? also static IPs didn't seem to work anyway\).

