# Backups

## RBD Snapshots

To create an RBD snapshot, run 

```bash
rbd snap create <IMAGE NAME>@<SNAP NAME>
```

where `<SNAP NAME>` is the requested snapshot name.

For example, to snapshot `virtual-machines/steeltoe` with a snapshot named `20190108` :

```bash
rbd snap create virtual-machines/steeltoe@20190108
```

We use the `YYYYMMDD` format as our naming convention for snapshot based on the day of the snapshot.  For example, a snapshot made on January 1, 2019 would be `20190101` . Any further snapshots have a `-1` \(etc.\) appended.

## CephFS Snapshots

To create a snapshot of a CephFS directory, run:

```bash
mkdir <PATH TO DIR>/.snap/<SNAP NAME>
```

More information is available at [https://github.com/ceph/ceph/blob/master/doc/dev/cephfs-snapshots.rst\#creating-a-snapshot](https://github.com/ceph/ceph/blob/master/doc/dev/cephfs-snapshots.rst#creating-a-snapshot)

We use the same snapshot naming convention as RBD snapshots.

