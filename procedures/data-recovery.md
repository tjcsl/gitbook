# Data Recovery

## Introduction

With Ceph's RBD snapshots, we are able to restore RBD images to previous states in order to recover data.

## Ceph RBD

On a Ceph admin node, \(substitute `<IMAGE NAME>` with the appropriate image path e.g. `virtual-machines/steeltoe`\)

1. find the appropriate image by running `rbd ls`
2. list all the image's snapshots by running `rbd snap ls <IMAGE NAME>` and gain information about the image with `rbd info <IMAGE NAME> .`
3. disable necessary features with `rbd feature disable <IMAGE  NAME> object-map fast-diff`
4. enable layering for RBD copy-on-write \([http://docs.ceph.com/docs/master/dev/rbd-layering/](http://docs.ceph.com/docs/master/dev/rbd-layering/)\) with `rbd feature enable <IMAGE NAME> layering`
5. using the image snapshot name obtained in step 2

For example to restore an image named `virtual-machines/steeltoe` from the snapshot `20190108` :

```bash
rbd feature disable virtual-machines/steeltoe object-map fast-diff
cd /mnt
mkdir steeltoe
rbd map virtual-machines/steeltoe@20190108
mount /dev/rbd/virtual-machines/steeltoe-part1@20180108 /mnt/steeltoe
# Obtain the necessary files from /mnt/steeltoe
umount /mnt/steeltoe
rbd unmap virtual-machines/steeltoe
rbd feature enable virtual-machines/steeltoe object-map fast-diff
```



a

