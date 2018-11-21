# OpenAFS Backups

The **OpenAFS backup system** consists of three main programs: **buserver**, **backup**, and **butc**. Currently, ***WE HAVE NO BACKUPS AND THIS IS BAD***. Please see this documentation for guidance for how to set up a backup system.

## buserver

The buserver is a daemon which keeps track of the "backup database," which contains all information about what dumps have been performed, the dumping schedule, and what volumes go on what dumps. It also knows what dumps are on what tapes, and keeps track of what port offsets are on which server. The buserver should be run on ***all*** of the DB Servers by running:

```
 bos create -server <servername> -instance buserver -type simple -cmd </path/to/buserver>
```

For example:

```
bos create -server openafs1 -instance buserver -type simple -cmd /usr/lib/openafs/buserver
```

## butc

To write a backup dump to a tape, there must be a butc process running for that specific tape device. The butc process coordinates obtaining the correct tape for writing, and getting volume dumps, etc. as well as writing to the tape. It will also print information about what is currently happening and prompt for user input if it gets confused. Currently, we do not have a tape backup system.

## backup

The **backup** command suite has several commands to run. It has a nice built-in help system (like most AFS commands) so that information will not be duplicated here. It is generally advised to run many backup commands through interactive mode (run backup by itself and then run the commands). This allows multiple commands to be queued up and then monitored. Currently, an interactive backup session with local authentication tokens is kept running in the same screen as the butc processes for primary backup scheduling. NOTE: closing this session will cancel any jobs that are currently scheduled through it (you can see these with the jobs command) so please make note of anything that is scheduled before closing the session and reschedule them ASAP.

### Dump Scheduling

A dump schedule isn't really a "schedule" per se. A dump "schedule" basically just consists of a dump level, and long until it expires. For example, one of our dump schedules is:

```
/sunday1  expires in  27d
/monday1  expires in  13d
/tuesday1  expires in  13d
/wednesday1  expires in  13d
/thursday1  expires in  13d
/friday1  expires in  13d
/saturday1  expires in  13d
```

Which are named pretty intuitively. The '/sunday1' schedule is a full dump, and should occur every four weeks (as you can see, it expires in 27 days, making it so the backup system prevents anything from overwriting a /sunday0 backup dump until 27 days have passed). The other levels (which are actually /sunday1/Monday1 and so on; backup just doesn't repeat all of that) are incremental dumps, each one only recording the information changed since the last /sunday1 dump. These dump schedules only expire in 13 days, so it's possible to overwrite them in two weeks (which is what we do). Currently, we keep weekly dumps for one month (Sunday1-Sunday4) and daily dumps for two weeks (Monday1-Saturday1 for odd Sundays and Monday2-Saturday2 for even Sundays). This gives us the ability to restore a volume to any day in the past two weeks and to any Sunday in the past month.

### Hosts and port offsets

When performing a backup dump, a "port offset" must be specified unless the default value of 0 is appropriate. A port offset determines both the host to contact, and what tape device that host uses. The backup database is responsible for tracking which port offsets have been assigned to hosts. The host itself defines which local tape device or file each of its port offsets corresponds to (See tapeconfig furthur down on this page).

### Volume sets

A volume set is what it sounds like: just a set of volumes. A volume set is defined by a series of volume entries. A volume entry is an entry specifying what volume to backup, and on what server and partition. The volume name, partition, and server can all be simple regular expressions (the only special characters are brackets, dots, backslashes, and asterisks), so you can specify large amounts of volumes with a single volume entry. For example, the following volume set encompasses all current students for the 2009-2010 school year:

```
 Volume set currentstudents:
   Entry   1: server .*, partition .*, volumes: 2010\..*\.backup
   Entry   2: server .*, partition .*, volumes: 2011\..*\.backup
   Entry   3: server .*, partition .*, volumes: 2012\..*\.backup
   Entry   4: server .*, partition .*, volumes: 2013\..*\.backup
```

Note that the `.` is a wildcard character so we need to escape it (`\.`) to indicate a literal `.`. The wildcard `.*` matches any server or partition.

In general, we do not put the actual volumes into the volume sets. Instead we add the .backup volumes that are created nightly as snapshots of the volume. The reason for this is that when AFS actually dumps the volume to tape, it puts a lock on the volume for the duration of the dump and so by dumping the backup volume, we only interrupt someone's yesterday folder instead of their homedir.

## Configuration

***(Note, this section below is very out of date, as currently we don't have any backups. It is kept for future reference)***

On openafs3, the backup system configuration and logging takes place in `/usr/local/var/openafs/backup`. The files that live there are:

### tapeconfig

This is a very simple file, which maps tape devices to port offsets, optionally with tape sizes and filemark sizes. As of the time of this writing, it contains the following (on openafs3):

```
 500G    0       /tapes/faketapea        0
 500G    0       /tapes/faketapeb        1
 500G    0       /tapes/faketapec        2
 500G    0       /tapes/faketaped        3
```

This specifies four files to backup to (actually symlinks) which are mapped to port offsets 0-3. The reason for creating four is so that each of our primary volsets can be mapped to a different port offset so that a problem with one backup will not affect the others. Note that normally, with modern tape drives, it is recommended to omit the tape size and filemark size but with files, the former is highly recommended to avoid running out of space on the partition so both should be specified (if you give one you have to give both). The use of symlinks as targets instead of the actual files is recommended by the OpenAFS documentation as it allows for a more graceful recovery in the event a backup hits either the disk space or file size limits.

### CFG_device_name

This is the configuration file used for the tape device device_name. If the tape device was `/dev/foo/bar`, then the configuration filename would be `CFG_foo_bar` and for a backup file in `/tapes/faketapea` the configuration filename would be `CFG_tapes_faketapea`. As of now, we're using `/tapes/faketape[a-d]` so we have the corresponding config files `CFG_tapes_faketape[a-d]`. Since they are all basically the same and are all used the same way, they're all symlinked to the same configuration file: `CFG_faketapes`

### CFG_faketapes

The configuration file for the backup files. Right now it just contains the following:

```
 FILE YES
 MOUNT /root/scripts/afsmountscript.sh
```

The first line tells butc that it will be backing up to a file instead of to a tapedrive. The second line specifies a mount script that will be called to load the appropriate tape into the drive. In this case it links to a script from the OpenAFS docs that symlinks a filename of the form `<volset>.<dumplevel>.<tapenumber>` to the given device name.

### TE_device_name

Using the same device_name convention as CFG, these files are where errors are logged by butc.

### TL_device_name

Using the same device_name convention as CFG, these files are where all butc operations are logged.

## External links

* Openafs - configuring the AFS backup system: <http://docs.openafs.org/AdminGuide/ch06.html>
* Openafs - dumping data to a backup file: <http://docs.openafs.org/AdminGuide/ch06s10.html#HDRWQ282>
