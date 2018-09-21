# Setup

To set up a new Cluster node, you should follow these steps:

## Network it

Do the standard steps to add new entries for a node (or block of nodes) to DHCP and DNS (theo: see other documentation?).

Also make sure the switches/routers/cables are set up right.

## Install CentOS

The preferred method is to use Netboot, but a regular CentOS 7 install stick works as well. If installing from a stick make sure your hostname matches the one specified in DNS/DHCP.

## Install SSH Server

`yum install openssh openssh-server`. 'Nuf said.

## Run the ansible play

First, make sure you add the node to an existing/new host group that has the `cluster` role. Then, you can just run `ansible-playbook`, sit back, kick your feet up, and wait for the install to finish.
