# AFS

The **Andrew File System** is the network file system used by the Computer Systems Lab. It is a networked file system with a global namespace, and is in use among many universities and companies.

AFS currently runs all of the student home directories for the [workstations](../../../services/workstations/README.md), and is under consideration for being dropped in favor of [Cephfs](../cephfs.md)

## Notes

Currently all Solaris systems in the CSL use Transarc paths. For those not familiar with the difference in paths, see the Gentoo Linux OpenAFS Guide (see External Links below) for a handy comparison chart. At the time of writing, the guide does not specify a directory for client binaries in the Transarc paths section, but according to the old docs, they can be found at `/usr/afsws/bin`. (You shouldn't really need to worry about this as we don't run Solaris anymore, but just in case).

## Implementations

The CSL AFS servers and clients all run the [OpenAFS] implementation, but there also exist two others: Arla and IBM/Transarc. The IBM/Transarc implementation is an old version, back from when AFS was being developed by IBM. It is no longer maintained, but IBM open sourced the project when they decided to no longer maintain it, and that developed into the [OpenAFS] project. Arla was developed while IBM's AFS was not Open Source, in order to provide an Open Source implementation. The client is very functional today and is actively maintained, but the server side is not considered finished yet, and is not widely used. However, Arla's client is compatible (mostly) with OpenAFS servers, so the client has seen widespread use. Although the OpenAFS client is probably more popular in general, Arla can be run on several platforms that the OpenAFS client has issues with (such as the BSDs), and so it has achieved popularity with use on those platforms.

## AFS Servers

Currently, our OpenAFS servers run on the VMs [openafs1], [openafs2], [openafs3], [openafs4], and [openafs5]. Fun fact, this actually takes up a large part of our virtual machine capacity.

{% page-ref url="setup.md" %}

Openafs3 used to run on a Solaris Zone running on [Seatac] and [Dulles], aptly named haafs2. The Solaris Cluster ran on Seatac and Dulles, allowing for automatic fail-over of either AFS zone to the other host.  
Configuration would be provided here for posterity, but c'mon, it's Solaris, it's never going to get used again and besides there's too much anyway.

## AFS Backup

All volumes are backed up daily by a crontab on [openafs1]. The crontab runs a script which backs up web2.0\*, web.\*, 2014.\*, 2015.\*, 2016.\*, 2017.\*, staff.\*, user.\*, and parents.\*, along with some alumni sysadmins' homedirs.

## External Links

[The OpenAFS Homepage](http://www.openafs.org/)
[The Arla Project](http://www.stacken.kth.se/projekt/arla/)
[Gentoo Linux OpenAFS Guide](http://www.gentoo.org/doc/en/openafs.xml)

[OpenAFS]: openafs.md
[openafs1]: ../../../machines/vm-servers/gorgona.md
[openafs2]: ../../../machines/vm-servers/galapagos.md
[openafs3]: ../../../machines/vm-servers/antipodes.md
[openafs4]: ../../../machines/vm-servers/chatham.md
[openafs5]: ../../../machines/vm-servers/cocos.md
[Seatac]: ../../../machines/obsolete/seatac.md
[Dulles]: ../../../machines/obsolete/dulles.md
