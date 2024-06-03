# AlmaLinux

[AlmaLinux](https://almalinux.org) is a stable Linux distribution derived from the sources of the popular [RedHat Enterprise Linux](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux).

AlmaLinux is used for our FreeIPA servers because the Debian family doesn't offer the server packages (yet...)

## Notable Features

* It supports FreeIPA Server.

## Notable CSL Modifications

* We use [Netboot](../networking/netboot.md) to install it.

## Trivia

* AlmaLinux comes with a firewall set up by default, which is nice for security reasons given how critical our Alma machines are.&#x20;
* The AlmaLinux servers have automatic updates set up. They patch themselves at 3AM on weekends. We never have to do anything ourselves.
