# Utonium

**Utonium** is a physical server residing in the CSL Machine Room that currently serves [KVM Virtual machines](../../technologies/virtualization-stack/).

## Technical Specifications

| **Server Type**   | HP Proliant DL380 G10            |
| ----------------- | -------------------------------- |
| **CPU**           | Intel Xeon Silver 4208 @ 2.1 GHz |
| **RAM**           | 32 GB                            |
| **Hard Disks**    | 3x 2.4TB 2.5in 10K SAS RAID 1    |
| **OS**            | Ubuntu 22.04                     |
| **Firmware**      | U30                              |
| **iLO Firmware**  | 2.60                             |
| **Purchase Date** | April 2022                       |

## History

Utonium was purchased in 2022 as a replacement for the aging Sun servers. It is named after Professor Utonium from _The Powerpuff Girls._ It took over a year for Utonium to be used in production following its acquisition.

Utonium was originally intended to be the lab's primary FreeIPA server, but this role was ultimately given to Sauron, the lab's existing DNS server, and Utonium was reimaged as a VM server instead.
