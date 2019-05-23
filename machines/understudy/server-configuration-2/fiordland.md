# Fiordland

{% hint style="info" %}
Much of the information below has been imported from Livedoc.
{% endhint %}

**Fiordland** is a physical machine residing in the CSL [Machine Room](https://documentation.tjhsst.edu/general/machine-room). It currently serves as one of the [Understudy](https://documentation.tjhsst.edu/machines/understudy) servers, along with [Saruman](https://documentation.tjhsst.edu/machines/understudy/server-configuration-1/saruman).

## Technical Specifications <a id="technical-specifications"></a>

| Field | Value |
| :--- | :--- |
| Server Model | HP Proliant DL380 G4 |
| CPU | 2x Intel Xeon "Irwindale" Single-Core @ 3.6 GHz |
| RAM | 8GB |
| Hard Disks | 4x 72GB 3.5in 10K SCSI, RAID5 |
| Graphics | AMD Rage XL PCI |
| OS | Ubuntu 18.04 LTS |
| Firmware | P51 |
| iLO Firmware | 1.94 |
| Purchase Date | September 2005 |

## History <a id="history"></a>

Fiordland used to be one of the main [AFS](https://documentation.tjhsst.edu/technologies/storage/afs) servers for the lab until AFS was moved to the redundant haafs setup. Fiordland then served as a storage server in HA-iSCSI and then became the lab backup server for server and AFS backups using disks exported via iSCSI from Alexandria.

