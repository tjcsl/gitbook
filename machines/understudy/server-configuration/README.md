# Server Configuration

## Saruman

Saruman is a server residing in the CSL [Machine Room](../../../general/machine-room.md) that currently serves as the main Sysadmin Understudy server. 

Saruman will be running the VMs with ion and director \(to mimick the real production server\).

### Technical Specifications

|  |  |
| :--- | :--- |
| Server Model | HP ProLiant DL380 G6 |
| CPU | Intel Xeon E5540 \(16\) @ 2.533GHz |
| RAM |  |
| Hard Disks |  |
| Graphics | AMD ATI ES1000 |
| OS | Ubuntu 18.04 LTS |
| Firmware |  |
| iLO Firmware |  |

## Fiordland

{% hint style="info" %}
Much of the information below has been imported from Livedoc.
{% endhint %}

Fiordland is also server residing in the CSL [Machine Room](../../../general/machine-room.md) that currently serves as the second Sysadmin Understudy server.

Fjordland will be running the VM with the DNS. 

### Technical Specifications

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

### History

Fiordland used to be one of the main [AFS ](../../../technologies/storage/afs/)servers for the lab until afs was moved to the redundant haafs setup. Fiordland then served as a storage server in HA-iSCSI and then became the lab backup server for server and AFS backups using disks exported via iSCSI from Alexandria.

