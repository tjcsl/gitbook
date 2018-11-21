# Administration

OpenAFS is an implementation of the Andrew File System. It is used to store everything from student home directories to activity websites. There are currently five active storage servers \(openafs\[1-5\]\) and a planned archive server \(openafs6\).

Our OpenAFS cell is called "csl.tjhsst.edu", but you can just call it "csl" when navigating to it in the AFS tree.

## Administration Concepts

As you might expect, OpenAFS is a rather complex system. It is laid out using a few main components:

* `bos`, the **B**asic **O**ver**s**eers, which essentially oversees every other server
* `pts`, the **P**ro**t**ection **S**erver, which keeps track of users and groups
* `vos`, the **Vo**lume **S**erver, which keeps track of volumes
* `fs`, which, as you might expect, is the **F**ile **S**erver.

Each of these systems has a command named after it \(i.e. `vos` lets you interface with the volume server, `fs` works with the file server, and so on\) to allow you to administer it.

## Servers

The CSL has quite a few AFS servers:

* [openafs1](../../../machines/vm-servers/gorgona.md)
* [openafs2](../../../machines/vm-servers/galapagos.md)
* [openafs3](../../../machines/vm-servers/antipodes.md)
* [openafs4](../../../machines/vm-servers/chatham.md)
* [openafs5](../../../machines/vm-servers/cocos.md)

## Managing OpenAFS

OpenAFS has a few command-line tools that you can use in order to manage it. Before using them, gain administrative access to AFS. If you don't, you won't be able to make changes.

```text
$ kinit 2016fwilson/admin
Password for 2016fwilson/admin@CSL.TJHSST.EDU:
$ aklog
```

Once you've done this, if your `/admin` principal is in the AFS group `system:administrators`, you will have administrative access to AFS.

## `bos` \(Basic Overseer\)

`bos` manages everything. Hopefully, you won't have to deal with it much. If something breaks, `bos help` is a great resource. The one common use case for `bos` is if a volume suddenly goes offline. If this happens, it may need to be "salvaged." This can be done with the following command:

```text
$ bos salvage <server> <partition> <volume>
```

Replacing the text in the angle brackets with the correct values \(without the angle brackets, obviously\).

## `vos` \(Volume Server\)

AFS has a concept of volumes. A volume is simply a logical container for files. It differs from a directory in that it can be mounted anywhere in the AFS tree, and can be moved from server to server as needed.

To create a new volume:

```text
$ vos create <server> <partition> <volume>
```

using the same replacements as before.

After creating a volume, you will probably want to set the quota. See the section on Quotas below. You will probably also want to mount the volume somewhere, so you can actually use it. See the section on Volume Mountpoints below.

You can examine volumes with the `examine` subcommand:

```text
$ vos examine 2016.2016fwilson
2016.2016fwilson                  536910165 RW     875423 K  On-line
...
number of sites -> 1
   server openafs4.csl.tjhsst.edu partition /vicepa RW Site
```

Another common operation is listing volumes. This can be done by referring to the VLDB \(volume database\)

```text
$ vos listvldb
```

Or, if you want to query the server directly, you can do that as well:

```text
$ vos listvol openafs4
```

You can restrict listings to a specific partition as well.

When a partition on a server is running out of space, you may want to move volumes to another server or partition. This can be done with the `move` subcommand:

```text
$ vos move <volume> <oldserver> <oldpartition> <newserver> <newpartition>
```

This might take a while, and progress isn't printed. Be patient! If you don't know which volumes to move, this obscure command can print out the largest ones:

```text
$ vos listvol server partition | tr -s " " | tail -n +2 | head -n -3 | grep -v "\.backup" | sort -g -k 4 -r | cut -d' ' -f1,4 | less
```

You can also take volumes offline, or restore them, with the offline and online subcommands:

```text
$ vos offline 2016.2016fwilson
$ vos online 2016.2016fwilson
```

If a volume can't come online, it may need to be salvaged. See the section on `bos` at the beginning of the article.

## `fs` \(File Server\)

### Permissions

AFS completely ignores standard UNIX permissions. **That means that** `chmod` **will do absolutely nothing for you**. Instead, AFS uses its own permission system, which can only apply to an entire directory at a time, instead of just a single file. This means that you may have to find clever workarounds to some problems. As an AFS admin, you'll be able to modify permissions anywhere in the tree. Here's how you can do that:

```text
$ cd /afs/csl/some/directory
$ fs la .
... some output ...
$ fs sa . user <permissions>
```

`la` is an abbreviation for "list access list/ACL", and `sa` is an abbreviation for "set access list/ACL."

The `permissions` you can grant a user are as follows:

* `r`: read files in the directory, but not list them
* `l`: list files in the directory, but not read them
* `i`: create new files \(insert\) in the directory \(does not imply read/write after the files are created\)
* `d`: delete files in the directory
* `k`: set locks on files
* `w`: write to files in the directory
* `a`: set permissions on files in the directory
* `read`: an alias for `rl`
* `write`: an alias for `rlidkw`
* `all`: an alias for `rlidkwa`

### Quotas

AFS volumes have quotas \(i.e. storage limits\). The two major operations involved are examining quotas and setting quotas. First, `cd` to the directory where the volume in question is mounted:

```text
$ cd /afs/csl/students/2016/2016fwilson
```

To show the quota, use the `lq` subcommand \(short for "list quota"\):

```text
$ fs lq
Volume Name                    Quota       Used %Used   Partition
2016.2016fwilson             4194304     875423   21%          2%
```

To set the quota, use the sq command \(short for "set quota"\):

```text
$ fs sq . 4194304
```

`fs lq` will reflect this change. Note that the quota value you specify must be in kilobytes.

### Volume Mountpoints

Volumes can be mounted anywhere in the AFS tree. To manage volume mountpoints, there are two primary commands.

In order to mount a volume at a point in the tree, use the `mkmount` subcommand:

```text
$ fs mkmount /afs/csl/web/user/2016fwilson 2016.2016fwilson
```

To remove a mountpoint:

```text
$ fs rmmount /afs/csl/web/user/2016fwilson
```

Note that when creating a mountpoint, the target directory shouldn't already exist.

### Cache

Rarely, the AFS cache will act up on a specific machine. This problem may manifest itself in the form of an empty directory, for example. Fortunately, fixing it isn't that difficult.

If you're feeling lazy:

```text
$ fs flushall
```

You can also target a specific directory:

```text
$ fs flush /afs/csl/web/www
```

...or a specific volume:

```text
$ fs flushvolume 2016.2016fwilson
```

## pts \(Projection Server\)

The Protection Server keeps track of users and groups. By the way, it always assigns negative IDs to groups and positive IDs to users.

Help on all pts commands can be found with the `pts help` subcommand.

### Groups

AFS has a concept of groups. As an AFS admin, you can manage all existing groups \(including `system:administrators`\), and create new ones.

You can inspect an existing group by using the `examine` subcommand:

```text
$ pts examine system:administrators
... some information about the group ...
```

You can view the members of a group by using the membership subcommand:

```text
$ pts membership system:administrators
...
2016fwilson.admin
...
```

You can also view which groups a user is a member of using the same command.

```text
$ pts membership 2016fwilson
...
2016fwilson.conductor
web.admin
csl-research-6
...
```

Adding users to groups can be done using the `adduser` subcommand:

```text
$ pts adduser 2016jwoglom.admin system:administrators
```

The inverse action, removing users from groups, uses the `removeuser` subcommand:

```text
$ pts removeuser 2016jwoglom.admin system:administrators
```

Creating groups can be done with the creategroup subcommand:

```text
$ pts creategroup csl-research-6
```

### Users

AFS users are separate from LDAP users, and as such, must be created separately. This can be done with the `createuser` subcommand:

```text
$ pts createuser 2016jwoglom.admin
```

You can examine a user with the examine subcommand:

```text
$ pts examine 2016fwilson
```

