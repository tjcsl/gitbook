# OpenAFS

The open-source implementation of AFS that the CSL uses.

## Basic Terminology

* A **cell** is analogous to a [Kerberos](../../authentication/kerberos.md) realm. A cell corresponds to a single administrative unit, such as a company or a university (or, in our case, a high school). A large company or university may have several cells, however. CMU and MIT both have more than one.
* A **volume** is the smallest amount of data in OpenAFS that can be moved around by server commands, and not by a client. A volume is smaller than a partition, but is much larger (usually) than a single file or directory. For example, every user's home directory is its own volume.

## Server Processes

OpenAFS is probably best explained by explaining each of the different server processes and how they interact with each other. The servers are, in no particular order,&#x20;

* the fileserver
* the volume server
* the salvager
* the protection server
* the volume location server
* the bos server
* the backup server

### Organization

AFS servers are usually defined in two groups: file servers, and database servers (any server can be one of these, or both). File servers run the file server, volume server, and the salvager. Database servers run the volume location server, the protection server, and the backup server. All AFS servers (usually) run the bos server.

### File server

The fileserver basically serves files; i.e. something asks for a particular file, and the fileserver gives it. It does a few more things, like checking if the user has permission to access a certain file (which it does by accessing the one of the servers in the CellServDB), but those are at such a level of detail that it doesn't really matter to an administrator or to a user.

There is an `fs` command, but it doesn't really correspond to the server file server process itself. It is primarily a client command, see [OpenAFS client](client.md).

#### Storage

On UNIX-like systems, there are two storage backends for the OpenAFS file server:&#x20;

* **inode:** Inode-based file servers manipulate the underlying filesystem at a very low level, for a potential speed advantage. Also, if an fsck is run on the partition of an inode file server, it will see it as "corrupted", which is why a modified fsck is shipped with OpenAFS sometimes for use with inode-based fileservers.
* **namei:** Namei stores its data files in normal files, and therefore is more portable. Namei file servers are available for far more platforms (such as Linux) than inode-based ones, for that reason. Namei can also run on many filesystems, whereas inode can only run on a few (such as Irix XFS and ext2, possibly others).  However, in modern days the speed difference between inode and namei is becoming negligible, so most cells these days run namei file servers.

The file server stores it's data in the /vicepX (i.e. `/vicepa`, `/vicepb`, etc.) directories or partitions on the server. Typically, these directories are mount points for separate partitions. They do not have to be, however (for namei-based file servers). Although OpenAFS will refuse to use a `/vicepX` partition that is not a mount point initially, if you create the file `/vicepX/AlwaysAttach`, it will force the file server to use the directory. This is so that the file server does not accidentally use an empty `/vicepX` partition if for whatever reason the corresponding disk partition failed to mount.

Technically, there also exists a back end for the Windows operating system, but the OpenAFS Windows server is not as actively maintained as the Linux one.

{% hint style="info" %}
OpenAFS on Windows is limited to Microsoft Windows 2000, XP, 2003, XP64, Vista (all editions), Server 2008 (all editions), Windows 7 (all editions), Server 2008 R2 (all editions), Windows 8.x (amd64, x86), and Server 2012 (all editions) (as of December 2018).
{% endhint %}

### Volume Server

The volume server, or **vos**, is used to move, create, delete, and basically administrate data on a volume level. It does not do anything most of the time, unless an administrator somehow manipulates a volume.

See `man vos` for how to control the volume server process.

Volumes contain files and can be mounted read/write (served by only the RW copy on one file server) or read/only (server by any number of RO copies on any number of servers which can be updated with **vos release**.

The [fs mkmount document](http://docs.openafs.org/Reference/1/fs\_mkmount.html) has rules for how the cache manager handles mount points of volumes.

### Salvager

This is another rarely-used process that only typically runs when the file server is shutdown uncleanly. It is similar to `fsck` for AFS data, and checks for any inconsistencies. It can also be manually run against an entire server, a partition, or a specific volume if something doesn't look right.

The salvager doesn't have it's own command to control it; see `man bos` for how to run the salvager.

### Protection Server

The protection server, or **pts** keeps track of users and group membership. It uses the Ubik protocol (which runs on top of Rx) to synchronize the database between the different servers.

See `man pts` for how to use the protection server.

### Volume Location Server

The volume location server, or **vlserver** runs the heart of AFS: location independence. The vlserver keeps track of what server each volume is on, so that the clients do not need to know. If a volume is moved from one file server to another, then the volume server would contact one of the vlservers to update the location. The client contacts the vlserver to find the file server to obtain a file from, so there is no change in the client configuration at all. The vlserver also uses the Ubik protocol the synchronize the VLDB (Volume Location Database) between the different vlservers.

The vlserver does not have its own command suite. It is contacted by the volume server, so use `vos` for things that may be related to the vlserver.

### Bos Server

The bos server is the **B**asic **O**ver**s**eer process, which basically controls all of the other server processes running on a particular machine. It is used to start, stop, or disable server processes, and also handles the interaction between the file server, volume server, and the salvager. It also restarts all server processes every day at a certain time and checks for new versions of the server process binaries.

See the `bos` command for how to control the bos process.

### Backup Server

The buserver keeps track of the backup database, which consists of update schedules and dump sets. It dumps volume data to a tape or to a certain file, if specified, and keeps track of which dumps are expired, and what data to backup when. It is explained in more detail in [OpenAFS Backups](backups.md).

## Files

A few files (mostly configuration files) are necessary to run the server processes. A few files are hard-linked in multiple locations on CSL servers either because they can be in different places depending on the implementation, or because the sysadmin working on the server forgets where they are, so he puts them in as many places as he can think of, to be sure as not to miss it. They are hard links so all of these files in separate locations contain the same data.

### `CellServDB`

This file is located in either `/etc/openafs/CellServDB` (for clients) or `/etc/openafs/server/CellServDB`(for servers), and it contains the IP addresses of all of the Database servers for the cell. The format looks like this (in fact, this was once the CellServDB file for TJ's servers and clients):

```
 >csl.tjhsst.edu
 198.38.16.51            # openafs.tjhsst.edu
 198.38.16.56            # openafs2.tjhsst.edu
 198.38.16.57            # openafs3.tjhsst.edu
```

Note that text after a hash (#) is _**NOT**_ a comment, and is a required field in `CellServDB`.

### `afs.conf` and `afs.client.conf`

These are Debian specific files in `/etc/openafs/`, and are read by the OpenAFS startup scripts. The former is relatively well documented, so it will not be explained here. The latter is explained in the [client](client.md) page.

### `KeyFile`

This is in `/etc/openafs/server/`, and is very important; any OpenAFS server cannot start without this (usually). This file contains a [Kerberos](../../authentication/kerberos.md) keytab with the `afs@CELL` principal, transformed into OpenAFS's own format. This is a server's credentials when communicating with other servers, so that the server processes know that they are talking to a legitimate AFS server in the same cell. This also enables for various server commands to use the `-localauth` option when the file is readable. The file should be set so it is only readable by root.

In modern AFS,  non-single-DES keys (aka modern proper encryption type keys) are stored in rxkad.keytab in the `/etc/openafs/server` directory. See [this](https://www.openafs.org/pages/security/install-rxkad-k5-1.6.txt) and [this](https://www.openafs.org/pages/security/how-to-rekey.txt) for details.

### `BosConfig`

This configuration file for **bos** is in `/etc/openafs/`. There are various `bos` commands to edit the file from bos itself, but you can also manually edit the file, as it is a plain ASCII text file, although parts of the format are not very intuitive. It contains information about what server processes to start and what to do with them. It also sets the time to restart all of the processes and the time to check for new process binaries.

### `ThisCell`

ThisCell (located in `/etc/openafs/`) just contains the name of the cell that this machine belongs to.

### `cacheinfo`

This is in `/etc/openafs/`, and is a [client](client.md) configuration file.

### `prdb.DB0` and `vldb.DB0`

These are database files, located in `/var/lib/openafs/`. They contain the information for the protection database, and the volume location database, respectively.
