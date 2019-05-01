# Administration

Everything on the cluster is managed through [Ansible](../../technologies/tools/ansible.md) plays. This guide will show you how to use those plays, and how to add new ones.

## Organization

There's an existing organization to the play structure that helps keep everything simpler. It might not be the best organization, so it can be changed, but probably shouldn't be unless you want to re-write everything.

### Hosts and Roles

There are different host groups for each section of the cluster. `clustermaster` applies to infosphere, `hpc` applies to the HPC cluster, and `ibm` applies to the borg cluster. There are also `hpcgpu` and `ibmgpu` host groups that apply to the one gpu-enabled node in their respective clusters, and are not part of the regular cluster groups.

All machines part of the cluster, including infosphere, have the special `cluster` role applied to them in addition to the standard `common` and `auth` roles. `clustmaster` and `clustergpu` roles are responsible for installing more specific software, but the `cluster` role does all the heavy lifting.

There is also a `cluster.yml` play that applies all the basic roles to everything, but you should still manage parts of the cluster separately.

### `cluster` role

This role:

* Installs the base packages that are useful on the cluster
* Mounts cluster-specific NFS directory
* Copies correct cluster krb5.conf

What it doesn't do anymore:

* Installs and builds custom cluster software

All the software installation routines have been broken up into their own roles, all prefixed like `cluster-*`.

#### Custom cluster software

This is needed because the latest versions \(with necessary functionality\) aren't available in the CentOS repos.

* slurm \(18.08.4\)
* openmpi \(3.1.3\)
* opencv \(3.4.5\)
* abinit \(8.10.1\)

#### Custom Software install process

The best way to make a new play to install a new custom software is by copying one of the existing plays \(I like to copy openmpi\). The general process goes like this:

* Install dependencies
* Download tarball
  * If it's been downloaded already, skip the rest of steps.
* Configure software
* Compile and install
* Set up environment

More details can be found by reading the plays themselves at [gitlab repo](https://gitlab.tjhsst.edu/sysadmins/ansible).

Please note, when adding new software to be installed, you make a new role for it and add the corresponding tags

### Flags to control custom software install

Sometimes, you just want to force reinstallation of the custom software, but are too lazy to edit the ansible play and change it back to normal afterwards. Don't worry; past sysadmins had that problem too! There a special command-line flag to force installation of a single package! Looks like this:

```bash
ansible-playbook -i hosts --tags=install_{package} [hostgroup].yml
```

Make sure you run the regular ansible play before installing software; you don't want to compile against out-of-date packages.

### Regular Ansible playbook run

After becoming root on infosphere:

```bash
cd ~/ansible
ansible-playbook -i hosts --ask-vault-pass hpc.yml --skip-tags=install
ansible-playbook -i hosts --ask-vault-pass ibm.yml --skip-tags=install
ansible-playbook -i hosts --ask-vault-pass clustermaster.yml --skip-tags=install
```

## Current concerns

These are problems with the cluster present at the time of documentation \(March 2019\). If you can fix them, good job!

* The cluster plays fail on new machines, needing to be run multiple times before going all the way through. Try to be better.
* Sometimes the mounts come offline, and an extra ansible command needs to be run on reboot \(`ansible {hostgroup} [-k] -m command -a "mount -a"`\).
* Some borg nodes can't netboot. Minimal issue, as a regular [CentOS](../../technologies/servers/centos.md) install stick works fine.
* Some borg nodes now aren't getting DHCP either.
* Graphics card compatability on Borg nodes is super spotty. Moving around cards until they work in a node.
* Dylan said to look at DHCP forwarding on [Imply](../../machines/switches/imply.md) to attempt to fix HPC issue.
* `zoidberg` is currently being routed through the [Workstation](../workstations/) VLAN in order to get networking due to the below problem.
* The HPC rack can't get DHCP addresses, ~~and most attempts at static IPs fail as well.~~ but it looks like setting a static ip through `/etc/sysconfig/network/ifcfg-bond0` works. The nodes currently up are fine as long as they don't get rebooted.
* `hpc7` and `hpc11` are super offline \(have been for a while now\), `hpc9`is offline due to the above issue \(was rebooted\).
* X11 forwarding doesn't work, problem with slurm. See [this bug report](https://bugs.schedmd.com/show_bug.cgi?id=5692).

