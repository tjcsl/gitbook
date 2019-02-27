---
description: >-
  The tutorial for running sshuttle on your computer to have access to the tj
  network on a terminal from your laptop.
---

# Using sshuttle

We use sshuttle to use vpn over ssh.  This allows us to run commands from the tj csl network on our own computers without having to be on afs when usually connecting from ras1 or ras2.  

On ubuntu, you can download sshuttle by running `sudo apt-get install sshuttle`

 You need root access to run sshuttle on your machine. To make a connection to ras2, run this: `sshuttle -r [username]@ras2.tjhsst.edu 0/0` where \[username\] represents your tj csl account username.  After running this, you will need to open a new terminal session to connect to other devices.  You also need to add .tjhsst.edu to every host that you want to connect to.  For example, you would use `ssh rkim@asm.tjhsst.edu` and NOT `ssh rkim@asm` like you would if you were on ras1 or ras2, unless you set up your resolv configuration to allow you to do so.

**Setting up resolv.conf to use hostnames without .tjhsst.edu**

If you would like to not append .tjhsst.edu, you can modify your resolv config.  To do this, you would need to add the line`search csl.tjhsst.edu tjhsst.edu` to `/etc/resolvconf/resolv.conf.d/base`  After that, you need to run `resolvconf -u` to make these changes to your computer

