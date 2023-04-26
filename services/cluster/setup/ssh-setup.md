---
description: Goes over setting up SSH in a cluster node
---

# SSH Setup

In order to remotely access a cluster from a local machine, or collect info from the node, you need to setup up SSH. This setup is crucial, but easy once you get used to it.

## Pre-installation

1. Make sure that the monitor and keyboard is **plugged into** the correct cluster.
2. If it is on, turn off the cluster, and then turn it back on in order for us to do the next step.
3. Spam F1 when the 'IBM Booter' comes up, it should open the Task Manager system.
4. Go to `Boot Options` and press enter on `ubuntu`. This should boot up Ubuntu. (This step assumes that you have installed Ubuntu 20.04 LTS, if not, you need to install Ubuntu using a USB Drive)
5. If booted up properly, it should send you the login page. Ask a clusters lead on what the username and password is for the node.

## SSH Installation

1. Make sure that there is internet by pinging into `8.8.8.8`. if not, inform a cluster lead.
2. Run `sudo apt install ssh` to install SSH into the cluster.
3. Run `sudo vim /etc/ssh/sshd_config` to edit this file using Vim.
4. Change this line from #PermitRootLogin:

```
#PermitRootLogin restrictpassword
```

to...

```
PermitRootLogin yes
```

5. Save the file (`:w` or `:wq`)
6. Run `sudo service ssh restart` to restart the ssh server within the cluster.
7. Once after you ran the previous command without any errors, run `sudo service sshd restart`. Don't worry about the throw errors, that's normal.
8. Verify that you can ping into the borg cluster by typing the command `ping borgXX.csl.tjhsst.edu`, where the XX is replaced by the number for the borg (i.e borg37)

If all passes, congrats! You just successfully configured SSH on a borg cluster! Next is usually to run `ansible` to make sure that the borg gets it correct dependencies.
