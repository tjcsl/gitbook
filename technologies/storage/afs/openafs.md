# OpenAFS

The implementation of AFS that the syslab uses.

## Notes

We use modern encryption types and therefore pre openafs 1.8 we require libkrb5. This means use the **kerberos** flag on gentoo machines.

## Basic Terminology

* A **cell** is analogous to a [Kerberos] realm. A cell corresponds to a single administrative unit, such as a company or a university (or, in our case, a high school). A large company or university may have several cells, however. CMU and MIT both have more than one.
* A **volume** is the smallest amount of data in OpenAFS that can be moved around by server commands, and not by a client. A volume is smaller than a partition, but is much larger (usually) than a single file or directory. For example, every user's home directory is its own volume.

## Server Processes

OpenAFS is probably best explained by explaining each of the different server processes and how they interact with each other. The servers are, in no particular order, the fileserver, the volume server, the salvager, the protection server, the volume location server, the bos server, the backup server, the kaserver, and the upserver. The upserver, kaserver, and the backup server are not used in the CSL, but all of the rest are.

### Organization

AFS servers are usually defined in two groups: file servers, and database servers (any server can be one of these, or both). File servers run the fileserver, volume server, and the salvager. Database servers run the volume location server, the protection server, and the backup server. All AFS servers (usually) run the bos server.

### Fileserver

The fileserver basically serves files; i.e. something asks for a particular file, and the fileserver gives it. It does a few more things, like checking if the user has permission to access a certain file (which it does by accessing the one of the servers in the CellServDB), but those are at such a level of detail that it doesn't really matter to an administrator or to a user.

There is an "fs" command, but it doesn't really correspond to the server fileserver process itself. It is primarily a client command, see [OpenAFS client](client.md).

#### Storage

On UNIX-like systems, there are two storage backends for the OpenAFS fileserver: **inode** and **namei**. Inode-based fileservers manipulate the underlying filesystem at a very low level, for a potential speed advantage. Namei stores its data files in normal files, and therefore is more portable. Namei fileservers are available for far more platforms (such as Linux) than inode-based ones, for that reason. Namei can also run on many filesystems, whereas inode can only run on a few (such as Irix XFS and ext2, possibly others). Also, if an fsck is run on the partition of an inode fileserver, it will see it as "corrupted", which is why a modified fsck is shipped with OpenAFS sometimes for use with inode-based fileservers. However, in modern days the speed difference between inode and namei is becoming negligable, so most cells these days run namei fileservers.

The fileserver stores it's data in the /vicepX (i.e. /vicepa, /vicepb, etc.) directories or partitions on the server. Typically, these directories are mount points for separate partitions. They do not have to be, however (for namei-based fileservers). Although OpenAFS will refuse to use a /vicepX partition that is not a mount point initially, if you create the file /vicepX/AlwaysAttach, it will force the fileserver to use the directory. This is so that the fileserver does not accidentally use an empty /vicepX partition if for whatever reason the corresponding disk partition failed to mount.

Technically, there also exists a backend for the Windows operating system, but the OpenAFS Windows server of any kind is considered very unstable, not not yet suited for production use.

### Volume Server

The volume server, or **vos**, is used to move, create, delete, and basically administrate data on a volume level. It does not do anything most of the time, unless and administrator somehow manipulates a volume.

See `man vos` for how to control the volume server process.

Volumes contain files and can be mounted read/write (served by only the RW copy on one fileserver) or read/only (server by any number of RO copies on any number of servers which can be updated with **vos release**.

The [fs mkmount document](http://docs.openafs.org/Reference/1/fs_mkmount.html) has rules for how the cache manager handles mountpoints of volumes.

### Salvager

This is another rarely-used process that only typically runs when the fileserver is shutdown uncleanly. It is similar to fsck for AFS data, and checks for any inconsistencies. It can also be manually run against an entire server, a partition, or a specific volume if something doesn't look right.

The salvager doesn't have it's own command to control it; see `man bos` for how to run the salvager.

### Protection Server

The protection server, or **pts** keeps track of users and group membership. It uses the Ubik protocol (which runs on top of Rx) to synchronize the database between the different servers.

See `man pts` for how to use the protection server.

### Volume Location Server

The volume location server, or **vlserver** runs the heart of AFS: location independence. The vlserver keeps track of what server each volume is on, so that the clients do not need to know. If a volume is moved from one fileserver to another, then the volume server would contact one of the vlservers to update the location. The client contacts the vlserver to find the fileserver to obtain a file from, so there is no change in the client configuration at all. The vlserver also uses the Ubik protocol the synchronize the VLDB (Volume Location Database) between the different vlservers.

The vlserver does not have its own command suite. It is contacted by the volume server, so use `vos` for things that may be related to the vlserver.

### Bos Server

The bos server is the <b>B</b>asic <b>O</b>ver<b>s</b>eer process, which basically controls all of the other server processes running on a particular machine. It is used to start, stop, or disable server processes, and also handles the interaction between the fileserver, volume server, and the salvager. It also restarts all server processes every day at a certain time and checks for new versions of the server process binaries.

See the `bos` command for how to control the bos process.

### Backup Server

The buserver keeps track of the backup database, which consists of update schedules and dump sets. It dumps volume data to a tape or to a certain file, if specified, and keeps track of which dumps are expired, and what data to backup when. It is explained in more detail in [OpenAFS Backups](backups.md).

### Kaserver

The kaserver is used for authentication, and functions similar to a Kerberos IV server. The CSL uses a normal [Kerberos] V server in favor of this, since it is more secure, as do many organizations these days.

### Upserver

The upserver is used to make sure that various configuration files between various servers are kept up to date and are the same (such as CellServDB and KeyFile).

## Files

A few files (mostly configuration files) are necessary to run the server processes. A few files are hard-linked in multiple locations on CSL servers either because they can be in different places depending on the implementation, or because the sysadmin working on the server forgets where they are, so he puts them in as many places as he can think of, to be sure as not to miss it. They are hard links so all of these files in separate locations contain the same data.

### `CellServDB`

This file is located in either `/etc/openafs/CellServDB` or `/etc/openafs/server/CellServDB`, and it contains the IP addresses of all of the Database servers for the cell. The format looks like this (in fact, this was once the CellServDB file for TJ's servers and clients):

```
 >csl.tjhsst.edu
 198.38.16.51            # openafs.tjhsst.edu
 198.38.16.56            # openafs2.tjhsst.edu
 198.38.16.57            # openafs3.tjhsst.edu
```

Note that text after a hash (#) is ***NOT*** a comment, and is a required field in CellServDB.

TJ's firewall used to prevent us from accessing foreign cells, and from clients outside the building from accessing our cell, as well, but that has since changed and we now keep fully populated CellServDBs. The easiest way to get a correct CellServDB is to pull it from any functioning and reasonably up-to-date server or workstation.

### `afs.conf` and `afs.client.conf`

These are Debian specific files in `/etc/openafs/`, and are read by the OpenAFS startup scripts. The former is relatively well documented, so it will not be explained here. The latter is explained in the [client](client.md) page.

### `KeyFile`

This is in `/etc/openafs/server/`, and is very important; any OpenAFS server cannot start without this (usually). This file contains a [Kerberos] keytab with the `afs@CELL` principal, transformed into OpenAFS's own format. This is a server's credentials when communicating with other servers, so that the server processes know that they are talking to a legitimate AFS server in the same cell. This also enables for various server commands to use the "-localauth" option when the file is readable. The file should be set so it is only readable by root.

**In modern AFS (post 1.4.X and 1.6.X and not 1.8 where there is a KeyFileExt)** non-single-DES keys (aka modern proper encryption type keys) are stored in rxkad.keytab in the openafs server directory. See [this](https://www.openafs.org/pages/security/install-rxkad-k5-1.6.txt) and [this](https://www.openafs.org/pages/security/how-to-rekey.txt) for details.

### `BosConfig`

This configuration file for bos is in `/etc/openafs/`. There are various `bos` commands to edit the file from bos itself, but you can also mangually edit the file, as it is a plain ASCII text file, although parts of the format are not very intuitive. It contains information about what server processes to start and what to do with them. It also sets the time to restart all of the processes and the time to check for new process binaries.

### `ThisCell`

ThisCell (located in `/etc/openafs/`) just contains the name of the cell that this machine belongs to.

### `cacheinfo`

This is in `/etc/openafs/`, and is a [client](client.md) configuration file.

### `NetInfo` and `NetRestrict`

These files, either in `/etc/openafs/` or `/etc/openafs/server/`, specify which IP addresses the OpenAFS processes advertise that they are using. Both of them have the same format; just one IP address per line. In the general case, OpenAFS just advertises all IP addresses that the machine has. However, if NetInfo exists, OpenAFS processes only advertise the IP addresses listed in that file. If NetRestrict exists, OpenAFS processes make sure that they do not use the IP addresses listed in that file.

These files are very useful for hiding addresses in private subnets such as 192.168.x.x if a machine has some, so that it doesn't advertise them to clients, so clients do not try and access private addresses (which could potentially decrease performance due to timeouts).

### `sysid`

This file, located in `/etc/openafs/server-local/` contains information about what IP addresses a server has to advertise as; essentially, what IP addresses a server determines as its identity. If you change what IPs a server has, removing this file and restarting bos will force it to regerate the sysid, and ensure that the list of IP addresses it uses is updated.

### `SALVAGE.fs`

This file, located in /etc/openafs/server-local/, determines if a server crashed, or was otherwise shutdown uncleanly. If it exists when bos starts up, it is one of the triggers to force a full system salvage.

### `prdb.DB0` and `vldb.DB0`

These are database files, located in `/var/lib/openafs/`. They contain the information for the protection database, and the volume location database, respectively.

## See Also

* [AFS](README.md)
* [OpenAFS Client](client.md)
* [AFS Directory Structure](directory-structure.md)

[Kerberos]: ../../authentication/kerberos.md
