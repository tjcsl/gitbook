---
description: The CSL's remote access servers
---

# Remote Access

The CSL employs a remote access functionality called **Remote Access Servers** (RASes for short) for students and staff to access their CSL files outside school grounds. Currently, we have two remote access servers (`ras1` and `ras2`) which are both VMs that are the only machines beyond a select few that provide incoming SSH from outside the CSL network.

We use [fail2ban](http://www.fail2ban.org/wiki/index.php/Main\_Page), an intrusion detection software, to block repeated mass authentication attempts against the remote access servers.

Issues related to the Remote Access Servers should be directed to the [Infrastructure Lead](../../general/sysadmins-list.md#current-leads).

