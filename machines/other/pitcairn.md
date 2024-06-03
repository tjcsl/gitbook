# Sauron

**Sauron** is a physical machine residing in the CSL Machine room. It currently acts as IPA1, our main FreeIPA server. In this capacity, it is the lab's primary LDAP, Kerberos, and DNS server. In addition, sauron is our primary [NTP](../../technologies/networking/ntp.md) and [DHCP](../../technologies/networking/dhcp.md) server.

## Technical Specifications

| Field             | Value                                    |
| ----------------- | ---------------------------------------- |
| **Server Type**   | HP Proliant DL380 G6                     |
| **CPU**           | 2x Intel Xeon E5540 Hexa-Core @ 2.93 GHz |
| **RAM**           | 74 GB                                    |
| **Hard Disks**    | 2x 146GB 2.5in 15K SAS, RAID 1           |
| **OS**            | AlmaLinux 9                              |
| **Purchase Date** | Unknown                                  |

## History

Sauron was one of three servers running VMware ESXi used to host JCIRN. It later served as the lab's primary DNS server. Sauron was chosen as the CSL's primary FreeIPA server over Utonium since it seemed more worthwhile to assign Utonium, the newer server, a more resource-heavy role managing VMs.
