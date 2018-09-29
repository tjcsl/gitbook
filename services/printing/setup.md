# Setup

CUPS is a standards-based. open-source printing system for Unix-like operating systems.  You can read more about CUPS at its [Wikipdia page](https://en.wikipedia.org/wiki/CUPS).  The Arch Wiki also has a [good article about it](https://wiki.archlinux.org/index.php/CUPS.).

## Installation

To setup a CUPS server, it is necessary to create a VM server.  Follow the instructions found in the QEMU pages of these docs to create a VM server.  It should be on the 1600 VLAN with DHCP and DNS configured.

You should run the Ansible play `cups.yml` to create the base VM config, install the CUPS server, and install appropriate drivers.  

## Configuration on VM

After accessing the VM over SSH, you should add any users you wish to allow to administer the CUPS server to the `lpadmin` group with `usermod -aG lpadmin <USERNAME>` (as root).  

You should edit the CUPS conf(`/etc/cups/cupsd.conf`)to allow access to the CUPS remote administration website for Sysadmin VPN IPs.

{% hint style="info" %}
You must always restart the CUPS server after editing `cupsd.conf`.  You can restart the server with `systemctl restart cups`.
{% endhint %}




