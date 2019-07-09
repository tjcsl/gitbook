# 2018 Purchases

### Summary

In the summer of 2018, the CSL purchased 6 new HP DL385 G6 servers: three to serve VMs and three as storage servers. They were delivered and installed at the beginning of the 2018-19 school year. 

### Rationale

Previously the storage backend had been a single disk array \(Apocalypse\) connected to [Valdes ](../ceph/valdes.md)and [Barrel](../ceph/barrel.md). With the introduction of Ceph for storage, these servers were deemed insufficient.  [Ceph ](../../technologies/storage/ceph/)requires each OSD server to have its own dedicated drives, and with the Apocalypse array servers were not being assigned the right drives at startup.  Therefore, DL385s with dedicated internal drives were deemed to be an optimal solution.  As the production cluster was moved to these new servers, the Apocalypse array was repurposed as a backup Ceph cluster.

At the same time as the storage servers purchase, three new DL385s were also purchased as VM hosts to increase the CSL's VM capacity and phase out older hardware.

### VM Servers

* [Overlord](../vm-servers/overlord.md)
* [Waverider](../vm-servers/waverider.md)
* [Torch](../vm-servers/torch.md)

### Storage Servers

* [Karel](../ceph/karel.md)
* [Wumpus](../ceph/wumpus.md)
* [Stobar](../ceph/stobar.md)



