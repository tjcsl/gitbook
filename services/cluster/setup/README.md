# Setup

To set up a new Cluster node, you should follow these steps:

## Network it

Do the standard steps to add new entries for a node (or block of nodes) to DHCP and DNS.

{% content-ref url="../../../technologies/networking/dns.md" %}
[dns.md](../../../technologies/networking/dns.md)
{% endcontent-ref %}

{% content-ref url="../../../technologies/networking/dhcp.md" %}
[dhcp.md](../../../technologies/networking/dhcp.md)
{% endcontent-ref %}

Also make sure the switches/routers/cables are set up right.

## Install Ubuntu

The preferred method is to use Netboot, but a regular Ubuntu install stick works as well. If installing from an USB stick make sure your hostname matches the one specified in DNS/DHCP.

## Setup SSH

See [SSH Setup](./#setup-ssh) page to learn more on how to setup SSH for the clusters.

## Run the Ansible play

First, make sure you add the node to an existing/new host group that has the `cluster` role. Then, you can just run `ansible-playbook`, sit back, kick your feet up, and wait for the install to finish.
