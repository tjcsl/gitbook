# 2011 Sun Upgrades

Notable for being the last time [Moon](../other/moon.md) was upgraded.

{% hint style="info" %}
This is written in present tense, but should not be taken as such \(for hopefully obvious reasons\).
{% endhint %}

## Timeframe

### Dulles/Seatac

We hope to start the Dulles/Seatac upgrade at approximately 8pm on Friday. This puts [AFS](../../technologies/storage/afs/) downtime at approximately 10pm on Friday April 15. Active web AFS volumes will be migrated to the Openafs2 zone before the upgrade in order to minimize downtime. The salvaging on each server will require about 4–5 hours of total downtime, about evenly split between the two servers. To keep the integrity of the volumes, web vols will need to be taken offline for a short period of time as well. The salvaging will start when the other upgrades finish, and will last for ~4–5 hours.

### Openafs2

Openafs2 will be upgraded after the Dulles/Seatac upgrade occurs. This probably will happen around 12am on Saturday April 16, or possibly earlier/later. No downtime will be experienced by current student, faculty, staff, or service volumes.

### Logan

[Logan](https://github.com/tjcsl/gitbook/tree/b18eaea16346c14456040c32aa7980539eedfbc2/machines/sun-servers/logan.md)'s work will be completed sometime during the time period of Saturday through Monday. There will be a short period \(5–10 minutes\) where CUPS and Sun Secure Remote Desktop will be unavailable.

### Sun Ray Services

The Sun Ray servers will be upgraded serially throughout the week. There should be no real noticed downtime of the network except for any sessions that might need to be terminated when rebooting one of the servers.

### Vega

The work occurring on [Vega](../sun-servers/vega.md) will occur during the week, most likely on Thursday or Friday \(April 21–22\).

### Moon

[Moon](../other/moon.md) will be upgraded Monday evening. A short outage is expected to occur to reboot into the new disk boot environment.

## Dulles/Seatac

Spring break is one of the few times that large systems can be modified for upgrades. The last major upgrade to [AFS](../../technologies/storage/afs/) happened during winter break of the 2008-2009 school year, when Dulles and Seatac were set up as a server cluster for AFS redundancy and high availability. At this time, we would like to upgrade these servers to keep up with the latest software patches for both security and reliability reasons.

### Solaris Cluster

Both servers are currently running version 3.2 of the Solaris Cluster software. The intent here is to upgrade these to Solaris Cluster 3.3 \(SC3.3\) which has some new features and added benefits, as well as other general software patches and fixes. The servers will be upgraded using the "live upgrade" method, which essentially builds a copy of the disk image and installs the SC3.3 software on top of that. After all nodes are upgraded, each will need to be shut down. This will require downtime of the services running on the cluster, which includes AFS and mail. The downtime here is estimated to be far less than two hours, assuming no additional hiccups are encountered along the way.

### Solaris

After upgrading each server to SC3.3, they will be individually upgraded to Solaris 10 update 9. The servers are currently running Solaris 10 update 6. An upgrade to Solaris 10 update 7 was attempted sometime during the 2008-2009 school year after the cluster's installation, but was aborted due to a bug in the Live Upgrade process that would have allowed for the upgrade. Upgrading these servers provides the latest patches for Solaris and the supporting software, and introduces new features - see the Release Notes for updates 7, 8, and 9. At this point we are not investigating the possibility of upgrading the cluster to Solaris 11.

### AFS

While these two servers have been operating without delay for the past couple of years, the [OpenAFS](../../technologies/storage/afs/) software has lagged behind the latest that is available, and is now several "dot" versions \(1.4.x\) out of date. The servers are currently running OpenAFS 1.4.11, whereas the latest available is 1.4.14.2. Due to the timing of this upgrade, we might be able to upgrade the servers to OpenAFS 1.6, which introduces Demand-attach AFS. This feature allows servers to only mount the volumes that it knows are being accessed, instead of mounting all the ones it knows about. This greatly reduces server startup time as well as salvage/recovery time.

### AFS Salvage

After upgrading the [OpenAFS](../../technologies/storage/afs/) server version on both Haafs1 and Haafs2, each server will perform a full partition salvage of all volumes. This is a routine measure meant to correct any errors found in the volumes.

## openafs2

The [OpenAFS](../../technologies/storage/afs/) server on [Betelgeuse](../sun-servers/betelgeuse.md)'s zone, openafs2, will be upgraded to the latest stable version as of its upgrade, the same version haafs1 and haafs2 will be upgraded to.

## Logan

### Solaris

[Logan](https://github.com/tjcsl/gitbook/tree/b18eaea16346c14456040c32aa7980539eedfbc2/machines/sun-servers/logan.md) is currently installed with Solaris 10 update 6, which was released in October 2008. The server will be reinstalled with Solaris 10 update 9.

### LDAP2

The LDAP2 zone will be reinstalled as an LDOM, which simplifies some of its administration. As with before, the virtual machine will host a Read-Only copy of LDAP, mirrored off of the LDAP1 LDOM on [Ohare](../sun-servers/ohare.md).

## Sun Ray Services

Each of the three main Sun Ray servers, [Centauri](../sun-servers/centauri.md), [Deneb](../sun-servers/deneb.md), and [Sirius](../sun-servers/sirius.md), will be patched to the latest current of software available from Oracle. After being patched, the servers will be upgraded to Sun Ray Server 5.1, which provides some enhancements to the current set of software.

## Vega

### Solaris

[Vega](../sun-servers/vega.md) will be upgraded from Solaris 10 update 8 to Solaris 10 update 9 via a clean reinstall. It will then be patched to comply with any/all TJ/FCPS system security standards.

### Virtualbox

Virtualbox will be upgraded from 3.0.8 to 4.0.4, or whatever the latest stable version is at that time.

### Moon

The version of Solaris installed on [Moon](../other/moon.md) will be Live Upgraded to update 9 and the latest patches will be installed.

