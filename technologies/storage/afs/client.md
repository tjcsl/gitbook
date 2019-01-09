---
description: Describes client setup for OpenAFS
---

# Client Setup

This page describes the configuration steps necessary to set up an [OpenAFS](client.md) client. For server setup, see:

{% page-ref page="setup.md" %}

TL;DR use ansible for both clients and servers.

## Overview

The OpenAFS client consists of a kernel module, and a userspace program: `afsd` \(also called the "Cache Manager"\). One of the only advantages of AFS over other networked file systems is the aggressive caching system in AFS employed by the client.

## Kernel Module

### Debian

```text
sudo apt install openafs-modules-dkms
```

See the [DKMS manual](https://linux.die.net/man/8/dkms) for more details about dynamic kernel modules.

## `afsd`

After the kernel module is installed, then just install the OpenAFS client program like normal \(`sudo apt install openafs openafs-client` on Debian-based systems\), and configure it.

The Cache Manager attempts to store every file accessed in its local cache, to speed things up. If a file in its cache becomes old, it is the server's responsibility to notify the client of this \(to break the callback to the client\). This is why in OpenAFS, reads are generally much faster than writes.

### Configuration

The configuration file `/etc/openafs/afs.conf` is used by the Debian specific init scripts, and doesn't usually need to be touched. `/etc/openafs/afs.client.conf` is also Debian specific, but may need to be changed. Here's an explanation for the options here \(all of them should be set to either true or false\):

* `AFS_CLIENT`: Specifies whether the OpenAFS client should start on boot. If this is set to false, you can start the client manually with "/etc/init.d/openafs-client force-start".
* `AFS_AFSDB`: Specifies whether or not the client should use AFSDB records.
* `AFS_CRYPT`: Specifies whether or not encryption should be on by default \(encryption encrypts all data travelling between the servers and the client, at the cost of performance\).
* `AFS_DYNROOT`: Specifies whether or not the cell mount points in /afs/ should be dynamically populated according to the cells that are present in /etc/openafs/CellServDB.
* `AFS_FAKESTAT`: Specifies whether or not the client fakes stat\(2\) on directories under certain conditions, so the client doesn't have to look it up every time, which can take a while.

Most of the configuration should be done automatically by the `openafs-client` ansible role. Look at it in [GitLab](https://gitlab.tjhsst.edu/sysadmins/ansible/blob/master/roles/openafs-client/tasks/main.yml) for better info.  
Summary of the play:

* Installs the `ppa:openafs/stable` for Ubuntu
* Installs all the required modules
  * `dkms`
  * `openafs-client`
  * `openafs-krb5`
  * `openafs-modules-dkms`
  * `libpam-afs-session`
* Copies pre-configured versions of `/etc/openafs/ThisCell`, `/etc/openafs/cacheinfo`, and `/etc/openafs/CellServDB`
* Adds `pam_afs_session.so` to pam
  * `session optional pam_afs_session.so always_aklog` in the `comon-session` PAM options file \(to allow people to access their AFS files after login\)
* Copies useful AFS binaries

#### Sample `ThisCell`

Just a single line:

```text
csl.tjhsst.edu
```

Wow. Feel underwhelmed please.

#### Sample `cacheinfo`

Not as underwhelming because there's actually stuff going on here. Still just one line though.  

This file contains the location of the cache directory \(where all of the OpenAFS cache is stored\) and then the size of the cache in kilobytes.

```text
/afs:/var/cache/openafs:10000000
```

#### Sample `CellServDB`

Same as the server's version:

```text
>csl.tjhsst.edu
198.38.16.19    #openafs1.csl.tjhsst.edu
198.38.16.22    #openafs2.csl.tjhsst.edu
198.38.16.23    #openafs3.csl.tjhsst.edu
198.38.16.24    #openafs4.csl.tjhsst.edu
198.38.16.25    #openafs5.csl.tjhsst.edu
```

