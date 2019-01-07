# Administration

Everything on the cluster is managed through [Ansible](../../technologies/tools/ansible.md) plays. This guide will show you how to use those plays, and how to add new ones.

## Organization

There's an existing organization to the play structure that helps keep everything simpler. It might not be the best organization, so it can be changed, but probably shouldn't be unless you want to re-write everything.

### Hosts and Roles

There are different host groups for each section of the cluster. `clustermaster` applies to infosphere, `hpc` applies to the HPC cluster, and `ibm` applies to the borg cluster. There are also `hpcgpu` and `ibmgpu` host groups that apply to the one gpu-enabled node in their respective clusters, and are not part of the regular cluster groups.

All machines part of the cluster, including infosphere, have the special `cluster` role applied to them in addition to the standard `common` and `auth` roles. `clustmaster` and `clustergpu` roles are responsible for installing more specific software, but the `cluster` role does all the heavy lifting.

### `cluster` role

This role:

* Installs the base packages that are useful on the cluster
* Mounts cluster-specific NFS directory
* Copies correct cluster krb5.conf
* Installs and builds custom cluster software

#### Custom cluster software

This is needed because the latest versions \(with necessary functionality\) aren't available in the CentOS repos.

* slurm \(18.08\)
* openmpi \(3.1.0\)
* opencv \(3.4.4\)
* abinit \(8.8.4\)

#### Custom Software install process

The best way to make a new play to install a new custom software is by copying one of the existing plays \(I like to copy openmpi\). The general process goes like this:

* Install dependencies
* Download tarball
  * If it's been downloaded already, skip the rest of steps.
* Configure software
* Compile and install
* Set up environment

More details can be found by reading the plays themselves at \(gitlab repo\).

### Flags to control custom software install

Sometimes, you just want to force reinstallation of the custom software, but are too lazy to edit the ansible play and change it back to normal afterwards. Don't worry; past sysadmins had that problem too! There a special command-line flag to force installation of a single package! Looks like this:

```text
ansible-playbook [other args] --tags={package} --extra_vars="force_{package}=yes" [hostgroup].yml
```

### Regular Ansible playbook run

After becoming root on infosphere:

```text
cd ~/ansible
ansible-playbook -i hosts --ask-vault-pass hpc.yml
ansible-playbook -i hosts -k --ask-vault-pass ibm.yml
ansible-playbook -i hosts --ask-vault-pass clustermaster.yml
```

## Current concerns

These are problems with the cluster present at the time of documentation \(Jan 2019\). If you can fix them, good job!

* X11 forwarding doesn't work, problem with slurm. See [this bug report](https://bugs.schedmd.com/show_bug.cgi?id=5692).
* Sometimes the mounts come offline, and an extra ansible command needs to be run on reboot \(`ansible {hostgroup} [-k] -m command -a "mount -a"`\).
* Some borg nodes can't netboot. Minimal issue, as a regular [CentOS](../../technologies/servers/centos.md) install stick works fine.
* `zoidberg`Sis currently being routed through the [Workstation](../workstations/) VLAN in order to get networking due to the below problem.
* The HPC rack can't get DHCP addresses, and most attempts at static IPs fail as well. The nodes currently up are fine as long as they don't get rebooted.
* `hpc7` and `hpc11` are super offline \(have been for a while now\), `hpc9`is offline due to the above issue \(was rebooted\).

