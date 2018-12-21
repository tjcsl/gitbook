---
description: Apocalypse is a Supermicro SC847 in a multipathed JBOD configuration.
---

# Apocalypse

### Technical Specifications

| | |
| :--- | :--- |
| Array Type |  Supermicro SC847 E26-RJBOD1 |
| Hard Disks | 35 |
| Data Interface |  4x SFF-8088 SAS connectors \(2 + 2 configuration\) |
| Mgmt Interface | None |
| Purchase Date | December 2012 |

### Information

Apocalypse is a multipathed SAS storage array. This means that each drive has two SAS paths via two separate backplane modules. It also means that Apocalypse requires dual-ported SAS drives in order to take advantage of the redundant paths. There are a total of four SAS connectors on the back. The bottom two are redundant connections to the front SAS backplane and the top two are redundant connections to the rear SAS backplane.

### Population

The Apocalypse array contains the following disk groups:

* 11 2-terabyte 7.2k rpm Seagate ST2000NM0023 drives \(forms the second Apocalypse vdev\)
* 11 1-terabyte 7.2k rpm Seagate ST1000NM0001 drives \(forms the first Apocalypse vdev\)
* 4 6-terabyte 7.2k rpm HGST HUS726060AL4210 drives
* 6 900-gigabyte 10k rpm Western Digital WD9001HKHG-02VUC drives

  3 200-gigabyte HGST HUSMM1620ASS200 solid state drives \(currently unused\)

### Harddrive WWIDs

Below are the WWIDs for each of the HDDs installed in Apocalypse. If a drive fails, the below table can be used to identify the location of the failed drive and replace it.

#### Front

| | | | |
| :--- | :--- | :--- | :--- |
| 5000C50055DF0F1C | 5000C50055DF9D44 | 5000C50042012E70 | 5000C500558C5A0C |
| 5000C500420343AC | 5000C50042012F1C | 5000C500558C64B8 | 5000C500420A3408 |
| 5000C500420A3284 | 5000C500420A0920 | 5000C500420139FC | 50000C0F0119C010 |
| 50000C0F0115E6F0 | 50000C0F01B6D988 | 0PY13ZGA | 50000C0F0119C014 |
| 50000C0F01BE7CDC | 50000C0F01B6D20C | 0PY13LZA | 0PY13M1A |
| 5000cca24260ef7c | 5000cca242623e88 | 5000cca2425ff9d0 | 5000cca242623d40 |

#### Back

| | | | |
| :--- | :--- | :--- | :--- |
| Power Supplies | Blank | Blank | Blank |
| Power Supplies | Blank | Blank | Blank |
| Power Supplies | Blank | Blank | Blank |
| Blank | Blank | Blank | Blank |
| Blank | Blank | Blank | 5000C50056032227 |
| Blank | Blank | Blank | Blank |

