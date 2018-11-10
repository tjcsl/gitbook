# Setup

Pre-InstallationCUPS is a standards-basedn open-source printing system for Unix-like operating systems. You can read more about CUPS at its [Wikipdia page](https://en.wikipedia.org/wiki/CUPS). The Arch Wiki also has a [good article about it](https://wiki.archlinux.org/index.php/CUPS.).

## Installation

To setup a CUPS server, it is necessary to create a VM server. Follow the instructions found in the QEMU pages of these docs to create a VM server. It should be on the 1600 VLAN with DHCP and DNS configured.

You should run the Ansible play `cups.yml` to create the base VM config, install the CUPS server, and install appropriate drivers.

## Configuration on VM

After accessing the VM over SSH, you should add any users you wish to allow to administer the CUPS server to the `lpadmin` group with `usermod -aG lpadmin <USERNAME>` \(as root\).

You should edit the CUPS conf\(`/etc/cups/cupsd.conf`\) to allow access to the CUPS remote administration website for Sysadmin VPN IPs. This can be done by adding `Allow from <IP ADDRESS>` directives underneath `<Location />` and `<Location /admin>`.

You should also add `Listen <ADDRESS>` directives near the top of `cupsd.conf` to specify where the CUPS server should listen. In general, the CUPS server should listen on port 631.

{% hint style="info" %}
You must always restart the CUPS server after editing `cupsd.conf`. You can restart the server with `systemctl restart cups`.
{% endhint %}

## Web Configuration

To access the web interface, you should head over to the address of your CUPS server \(with `:631` specified at the end\). Once you reach the web interface, you should see a screen similar to the one below.

Click on the `Administration` tab at the top to view the main administrative interface. Most administrative actions can be performed here. On the screen, you can see various buttons and their functions are self-explanatory.

## Add Printer

To add a printer through the web interface, you should click on the `Add Printer` button.

You will most likely get prompted for your credentials. Enter either your root credentials or the credentials of any user in the `lpadmin` group. You will then proceed through a series of prompts that will ask for information about the printer.

* Under network printers, most likely you will select `App Socket/HP Jet Direct`. You should obtain this information from the printer manual.
* An example URI is `socket://printer202.csl.tjhsst.edu:9100`.  You should obtain this information from the manual.
  * Note: The printers should be on the workstation VLAN and connected to a port configured for that VLAN.
* For name, fill in a descriptive name for the printer.
* For description, fill in a description of the printer.
* For location, fill in the location of the printer.
* You should generally allow connection sharing.
* For Manufacturer and Model, select the appropriate ones for your printer.

Once your printer has been added, select the drop down menu and click `Print Test Page` to test if your connection is working. Repeat these steps for any other printers.

