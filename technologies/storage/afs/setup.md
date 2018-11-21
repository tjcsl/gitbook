# Setup

This page describes how to set up AFS on a server. If you are looking for the client configuration, see:

{% page-ref url="client.md" %}

TL;DR use ansible for both clients and servers.

## Packages

On Ubuntu:

- `openafs-modules-dkms`
- `openafs-krb5`
- `openafs-fileserver`
- `openafs-dbserver`
- `openafs-client`

## Configuration

The ansible play mentions something about vicepa and vicepb mounted on virtual disks `/dev/vdb` and `/dev/vdc` respectively. I have no idea what they are for, but they get ext4 formatted and mounted every time the play runs.

The configuration files `CellServDB` and `UserList` are copied to `/etc/openafs/server/CellServDB`, then the following commands are run:

* Create the **ptserver** process:
  ```
  bos create <hostname> ptserver simple /usr/lib/openafs/ptserver -localauth
  ```
* Create the **vlserver** process:
  ```
  bos create <hostname> vlserver simple /usr/lib/openafs/vlserver -localauth
  ```
* Create the **buserver** process:
  ```
  bos create <hostname> buserver simple /usr/lib/openafs/buserver -localauth
  ```
* Create the **dafs** process:
  ```
  bos create <hostname> dafs dafs /usr/lib/openafs/dafileserver /usr/lib/openafs/davolserver /usr/lib/openafs/salvageserver /usr/lib/openafs/dasalvager -localauth
  ```
* Start the backup cron job:
  ```
  bos create <hostname> dailybackup cron -cmd "/usr/bin/vos backupsys -server <hostname> -localauth" "2:00"
  ```

Note: these commands should only be run once, when creating a new OpenAFS server.

### Sample `CellServDB`

```
>csl.tjhsst.edu    #Cell name
198.38.16.19    #openafs1.csl.tjhsst.edu
198.38.16.22    #openafs2.csl.tjhsst.edu
198.38.16.23    #openafs3.csl.tjhsst.edu
198.38.16.24    #openafs4.csl.tjhsst.edu
198.38.16.25    #openafs5.csl.tjhsst.edu
```

### Sample `UserList`

Entries correspond to [Kerberos](../../authentication/kerberos.md) principals.

```
2019okulkarn.admin
2020fouzhins.root
```

## Automation

Like it says at the beginning, [the ansible repo on gitlab](https://gitlab.tjhsst.edu/sysadmins/ansible/blob/master/roles/openafs-server/tasks/main.yml) is the practically the ultimate source of authority for configuring OpenAFS. This document exists to provide a summary of the `openafs-server` play in case something happens to the repo.
