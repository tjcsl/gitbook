# CentOS

[CentOS](https://www.centos.org/) is a stable Linux distribution derived from the sources of the popular [RedHat Enterprise Linux](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux).

The CSL only has a few servers running on CentOS; namely, [The Cluster](../../cluster/README.md). The rest run on [Ubuntu Server](./ubuntu-server.md).

## Notable Features

* Slightly higher stability than Ubuntu
* Has a different package manager (`yum` + `rpm`)
* Slightly smaller package ecosystem, made up for by large 3rd party repositories.

## Notable CSL Modifications
* We use [Netboot](../netboot.md) to install it

## Trivia
* All of the system configuration and log files are named slightly differently than Ubuntu, making things slightly annoying.
* CentOS actually beats out Ubuntu in how old its Python is; there is no Python 3, only Python 2.7.5 in the official repository!
* You can pry my CentOS-specific Ansible plays from my cold, graduated hands.
