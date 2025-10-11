---
cover: ../.gitbook/assets/PXL_20220511_195818005.jpg
coverY: 0
---

# Borg Cluster

Acquired as a gift by NASA, the **Borg Cluster** is our second high performance computing cluster. It consists of 40 nodes, labeled `borgw2[01-40].`

## Technical Specifications

Every node in the cluster has the same specifications:

| **Specification** | Description                        |
| ----------------- | ---------------------------------- |
| **Server Type**   | IBM dx360 M4                       |
| **CPU**           | Intel(R) Xeon(R) E5-2670 @ 2.60GHz |
| **RAM**           | 32 GB                              |
| **Hard Disks**    | 500GB                              |
| **OS**            | CentOS 7                           |
| **Purchase Date** | November 2016                      |

## History

Dr. Torbert requested it during the previous school year through a NASA program that allowed TJ to procure it for free. The cluster came from NASA Goddard Space Flight Center in Greenbelt, MD and was previously used for climate modeling.

The original purchase price was $283,917, according to the invoice. To make space for the new cluster, the i7 Cluster was moved out and subsequently decommissioned.

The IBM cluster came with a giant radiator attached to the back for watercooling and used a huge high-voltage plug, so it wouldn't work in the machine room. The radiator was recycled and the cluster is currently using a single APC PDU. There are plans to purchase a more suitable solution, as the entire cluster cannot be powered from the single PDU.

There are 40 IBM dx360 M4 nodes in the cluster and each have dual 8-core Intel Xeon E5-2670's and 32GB of RAM. They each have a Xeon 5110P Phi as well, except virgo, which has a Tesla K40c. The nodes are labelled borgw201-borgw240, but were renamed after constellations. This constellation rename has been reverted in spring of 2019, sans the "w2" in the middle (so borg1-borg40).

## Trivia

Every node in the borg cluster is alternatively named after a constellation. This makes all them really, really hard to remember (even harder than the [HPC Cluster](hpc-cluster/) names!) except for by their number.
