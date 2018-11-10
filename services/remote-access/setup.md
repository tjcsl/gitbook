# Setup

## Installation

To setup a RAS server, it is necessary to create a VM server. Follow the instructions found in the QEMU pages of these docs to create a VM server. It should be on the 1802 VLAN with DHCP and DNS configured.

You should run the Ansible play `ras.yml` to create the base VM config, install/configure the OpenAFS client, install Python utilities, and configure fail2ban.

You should not have to take any other action than allowing SSH through the firewall, adding a DNS entry for the server, adding a DHCP entry for the server, and generating a keytab for the server.

