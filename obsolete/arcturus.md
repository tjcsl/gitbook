# Arcturus

{% hint style="warning" %}
This page contains information on systems that are in happy retirement from the Computer Systems Lab.
{% endhint %}

**Arcturus** is a big big SPARC server. Simply put, arcturus is a high-performance computing (HPC) server. It is also a Sun Ray server for HPC projects (separate from the primary Sun Ray FailOver Group (FOG)).

## Technical Specifications

| Field             | Value                                            |
| ----------------- | ------------------------------------------------ |
| **Server Type**   | Sun SPARC Enterprise M4000                       |
| **CPU**           | 4x SPARC64-VI (2 cores, 2 threads/core) @2.15GHz |
| **RAM**           | 16 GB on 2 boards (max 128 GB on 4 boards)       |
| **Hard Disks**    | 2x 73GB 2.5in 10K SAS, RAID 1                    |
| **OS**            | Solaris 10                                       |
| **Purchase Date** | Winter 2008                                      |

### Notes

_**IMPORTANT:**_ The power supplies are not redundant unless they are running at 208V and configured to be redundant. We are currently running them at 120V. Do not unplug either power cord without powering arcturus off first!

## History

Arcturus was received as part of the [2008 Sun AEG](../machines/history/2008-sun-aeg.md).

## Trivia

Arcturus replaced [Moloch](moloch.md). While it does not have hot swap CPU and memory boards like moloch did, just about every other part except the DVD player is hot-swappable, even the PCIe and PCI-X slots. It also pulls less power than moloch (idle vs. idle). Arcturus' CPUs can be upgraded to the newer SPARC64-VII (2.4 Ghz, 4 core, 2 threads/core) processors should we ever want to and can afford to.
