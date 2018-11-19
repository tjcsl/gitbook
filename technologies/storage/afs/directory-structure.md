# AFS Directory Structure

## General Conventions

Usually, if a mount point is read-only, then there will be another read-write mount point with the same name with dot in front of the name. For example, the mount point /afs/csl/service is read-only, but /afs/csl/.service is read-write. Both of those mount points are the same volume (service), but one is a read-only copy, and thus more likely to be available.

We also try to have volume names roughly correspond to where the volume is typically mounted in the directory tree, using dots as delimiters instead of slashes. For example, /afs/csl/service/matlab is a mount point to service.matlab, and /afs/csl/web/www is web.www. They do not have to completely correspond (volume names can only be so long), but they should be similar.

## Directory Hierarchy

### `/afs/csl.tjhsst.edu/`

This is the read-only mount point for `root.cell` for our cell. `/afs/.csl.tjhsst.edu/` is the read-write mount point, and `/afs/csl/` is a symlink to `/afs/csl.tjhsst.edu/`, and `/afs/.csl/` is a symlink to `/afs/.csl.tjhsst.edu/`

### `archive/`

This contains mountpoints for various volumes that are no longer used in day-to-day operations but that we want to continue to be accessible.

#### `cronos/`

All of the home directories before the lab switched to AFS (the old fileserver was called 'cronos', and served data with NFS). They are still around so that their web-docs are still available, so we do not break links.

#### `i1/`

Data files for the original [Intranet](../../../services/ion/README.md).

#### `legacy/`

Deactivated websites are remounted here in subfolders corresponding to their old subdomain.

#### `web.XXX/`

These are various old versions of the main tjhsst.edu website.

### `common/`

This contains shared directories for various groups and functions.

### `fcps/`

Home directories for non-TJHSST FCPS students/staff. They are separated into sub-directories by site.

### `parents/`

Home directories for parents; primarily webmasters.

### `staff/`

Staff home directories for accounts that authenticate against the windows servers are stored here.

### `students/`

Student home directories for accounts that authenticate against the windows servers are stored here, each separated by graduation year.

### `user/`

Legacy user home directories from when CSL user accounts were separate from the rest of the school.

### `web/`

Data for various web services. Many of the sub-directories here correspond to a subdomain http://<directory>.tjhsst.edu/, although some newer websites do not (see [Director](../../../services/director/README.md) and [Othello](../../../services/othello/README.md))

#### `sitemap/`

Contains information for the sitemap for the legacy Krysalis system.

#### `www/`

Data files for the main website under the [www](../../../services/www/README.md) subdomain (since deprecated). Other web subdomains (such as arts, activities, sports, etc.) also have their own directories in web/.

#### `webadmin/`

Data files for the [webadmin] application. See the [webadmin] article for information on the subdirectories here.

### `service/`

FINISH THIS

#### `belltab/`



#### `music/`



#### `sound/`



#### `bind/`



#### `mailman/`



#### `postfix/`



#### `httpd.intranet/`



#### `convert/`



#### `emperor_stuff/`



#### `images/`



#### `imaging/`



#### `logs/`



#### `matlab/`



#### `sysadmins/`



#### `haa/`



### `tokenizer/`
