# Data Recovery

## Introduction

With Ceph's RBD snapshots, we are able to restore RBD images to previous states in order to recover data.

## Ceph RBD

On a Ceph admin node, \(substitute `<IMAGE NAME>` with the appropriate image path e.g. `virtual-machines/steeltoe`\)

1. find the appropriate image by running `rbd ls`
2. list all the image's snapshots by running `rbd snap ls <IMAGE NAME>` and gain information about the image with `rbd info <IMAGE NAME> .`
3. disable necessary features with `rbd feature disable <IMAGE  NAME> object-map fast-diff`
4. `cd /mnt`
5.  `mkdir <SOME DIRECTORY>`
6. map the image \(`<SNAP NAME>` obtained from step 2\) with `rbd map <IMAGE NAME>@<SNAP NAME>`
7. mount the snapshotted image with `mount /dev/rbd/<IMAGE NAME>-part1@<SNAP NAME> /mnt/<SOME DIRECTORY>`
8. obtain the necessary files from the snapshot
9. umount the snaphshotted image with `umount /mnt/<SOME DIRECTORY>`
10. unmap the image: `rbd unmap <IMAGE NAME>@<SNAP NAME`
11. re-enable the necessary features with `rbd feature enable <IMAGE NAME> object-map fast-diff`

For example to restore an image named `virtual-machines/steeltoe` from the snapshot `20190108` :

```bash
rbd feature disable virtual-machines/steeltoe object-map fast-diff
cd /mnt
mkdir steeltoe
rbd map virtual-machines/steeltoe@20190108
mount /dev/rbd/virtual-machines/steeltoe-part1@20180108 /mnt/steeltoe
# Obtain the necessaryy files from /mnt/steeltoe
umount /mnt/steeltoe
rbd unmap virtual-machines/steeltoe
rbd feature enable virtual-machines/steeltoe object-map fast-diff
```



a

