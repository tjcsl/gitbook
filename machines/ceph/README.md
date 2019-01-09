# Ceph

The current set of G10 Ceph servers \(karel, wumpus, and stobar\) was acquired in 2018. Each server has 64GB of RAM, an 8-core AMD EPYC processor, 12 4TB hard drives, and two 400GB SSDs. See [here](../../technologies/storage/ceph/) for more info on how they are configured.

[Waitaha](waitaha.md), [Barrel](barrel.md), and [Valdes](valdes.md) make up the failover Ceph cluster.  The OSD disks themselves are on the [Apocalypse](apocalypse.md) array.

## History

The current Ceph servers were purchased in the summer of 2018 along with [Overlord](../vm-servers/overlord.md), [Torch](../vm-servers/torch.md), and [Waverider](../vm-servers/waverider.md) \(see [2018 Server Purchases](../history/2018-purchases.md)\).

After the events of the [Cephpocalypse](../history/2018-cephpocalypse.md), karel, wumpus, and stobar became the new Ceph cluster. For a period of time, wumpus was running Ubuntu 16.04 LTS, leading to high latencies on the entire cluster. The ability to take it down \(in the middle of a school day\) to upgrade it \(to 18.04 LTS with a newer kernel\) is a testament to the fault-tolerancy and redundancy of Ceph.

