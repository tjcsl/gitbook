# AFS

The **Andrew File System** is the network file system used by the Computer Systems Lab. It is a networked file system with a global namespace, and is in use among many universities and companies.

AFS currently runs all of the student home directories for the [workstations](../../../services/workstations.md), and is under consideration for being dropped in favor of [CephFS](../ceph/cephfs.md).

## Implementations

The CSL AFS servers and clients all run the [OpenAFS](openafs.md) implementation, but there also exist two others: Arla and IBM/Transarc. The IBM/Transarc implementation is an old version, back from when AFS was being developed by IBM. It is no longer maintained, but IBM open sourced the project when they decided to no longer maintain it, and that developed into the [OpenAFS](openafs.md) project. Arla was developed while IBM's AFS was not Open Source, in order to provide an Open Source implementation. The client is very functional today and is actively maintained, but the server side is not considered finished yet, and is not widely used. However, Arla's client is compatible \(mostly\) with OpenAFS servers, so the client has seen widespread use. Although the OpenAFS client is probably more popular in general, Arla can be run on several platforms that the OpenAFS client has issues with \(such as the BSDs\), and so it has achieved popularity with use on those platforms.

TL;DR: There are many other implementations of AFS but we use OpenAFS because of the above reasons.

## AFS Servers

Currently, our OpenAFS servers run on the VMs [openafs1](../../../machines/vm-servers/gorgona.md), [openafs2](../../../machines/vm-servers/galapagos.md), [openafs3](../../../machines/vm-servers/antipodes.md), [openafs4](../../../machines/vm-servers/chatham.md), and [openafs5](../../../machines/vm-servers/cocos.md). 

{% hint style="info" %}
Fun fact: This actually takes up a large part of our virtual machine capacity.
{% endhint %}

{% page-ref page="./" %}

## AFS Backup

We don't have any running backups \(which is bad\). Used to be backed up via a daily cron job.

## External Link

[The OpenAFS Homepage](http://www.openafs.org/) 

[The Arla Project](http://www.stacken.kth.se/projekt/arla/) [Gentoo Linux ](http://www.gentoo.org/doc/en/openafs.xml)

[Gentoo OpenAFS Guide](http://www.gentoo.org/doc/en/openafs.xml)

