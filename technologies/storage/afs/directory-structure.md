# Directory Structure

This list documents the current state of AFS directories in the CSL. For a more up-to-date list, perhaps ssh into RAS1 or RAS2 and `ls /afs/csl` yourself.

## General Conventions

Usually, if a mount point is read-only, then there will be another read-write mount point with the same name with dot in front of the name. For example, the mount point /afs/csl/service is read-only, but /afs/csl/.service is read-write. Both of those mount points are the same volume \(service\), but one is a read-only copy, and thus more likely to be available.

We also try to have volume names roughly correspond to where the volume is typically mounted in the directory tree, using dots as delimiters instead of slashes. For example, /afs/csl/service/matlab is a mount point to service.matlab, and /afs/csl/web/www is web.www. They do not have to completely correspond \(volume names can only be so long\), but they should be similar.

## Directory Hierarchy

### `/afs/csl.tjhsst.edu/`

This is the read-only mount point for `root.cell` for our cell. `/afs/.csl.tjhsst.edu/` is the read-write mount point, and `/afs/csl/` is a symlink to `/afs/csl.tjhsst.edu/`, and `/afs/.csl/` is a symlink to `/afs/.csl.tjhsst.edu/`

### `archive/`

This should contain mountpoints for various volumes that are no longer used in day-to-day operations but that we want to continue to be accessible. Currently non-existent, we probably lost them lol.

### `parents/`

Home directories for parents; primarily webmasters.

### `staff/`

Staff home directories for accounts that authenticate against the windows servers are stored here.

### `students/`

Student home directories for accounts that authenticate against the windows servers are stored here, each separated by graduation year.

* **`alumni/`**

  Dedicated folder for alumni.

* **`dropbox/`**

  Where students taking a class in the CSL can drop their code to turn it in.

### `web/`

Data for various web services. The sub-directories here used to correspond to a subdomain http://.tjhsst.edu/, although now all our newer websites are run through an nginx proxy that redirects traffic to the correct VM. See [Nginx](https://github.com/tjcsl/gitbook/tree/2135dd3e92566902bdf01c6d78e6adcad504ade8/technologies/web/nginx/md/README.md), [Director](../../../services/director/), and [Othello](https://github.com/tjcsl/gitbook/tree/2135dd3e92566902bdf01c6d78e6adcad504ade8/services/othello/README.md) for more details.

* **`www/` and `www.backup/`**

  Data files for the main website under the [www](https://github.com/tjcsl/gitbook/tree/2135dd3e92566902bdf01c6d78e6adcad504ade8/services/web/README.md) subdomain \(since deprecated\). Other web subdomains \(such as arts, activities, sports, etc.\) also have their own directories in web/.

* **`academics/` and `academics.backup/`**

  Ran the old [https://academics.tjhsst.edu](https://academics.tjhsst.edu) club websites. Currently obsolete, since replaced by [Director](../../../services/director/).

* **`web-docs/`**

  Peter Morasca, former Systems Administrator for the Windows domain, has some files in here. idk what for or why, but they're probably obsolete.

### `service/`

Various administrative scripts and files used to be stored here. Now it's much fewer than before, [NFS](https://en.wikipedia.org/wiki/Network_File_System) \(now not used either\) and [CephFS](../cephfs.md).

### `slt/`

Storage for student councils of each grade can place files to share between themselves and with the rest of the populace. Since unused in favor of Google Docs \(_\*shakes fist at clouds\*_\).

