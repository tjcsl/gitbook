---
description: Describes how to access to the CSL network from your laptop via sshuttle
---

# sshuttle Usage

We use sshuttle to access the CSL network on our own computers without having to be connected to a remote access server.

On Ubuntu, you can download sshuttle by running `sudo apt install sshuttle`

With root privileges, to make a connection to ras2, run this: `sshuttle -r [username]@ras2.tjhsst.edu 0/0` where \[username\] represents your tj csl account username.  After running this, you will need to open a new terminal session to connect to other devices.  You also need to add .tjhsst.edu to every host that you want to connect to.  For example, you would use `ssh rkim@asm.tjhsst.edu` and NOT `ssh rkim@asm` like you would if you were on ras1 or ras2, unless you set up your resolv configuration to allow you to do so.

**Setting up resolv.conf to use hostnames without .tjhsst.edu**

If you would like to not append .tjhsst.edu, you can modify your resolv config.  To do this, you would need to add the line`search csl.tjhsst.edu tjhsst.edu` to `/etc/resolvconf/resolv.conf.d/base`  After that, you need to run `resolvconf -u` to make these changes to your computer

