# Borg

Borg is experimental, so documentation is limited for now.

Pertinent things to know:

* We have 40 \(`borg[01-40]`\), but can only run 12 \(`borg[01-12]`\)
* They are named after constellations or, in rare cases, `borgw[01-40]`
* They run the same OS \([CentOS](../../technologies/operating-systems/centos.md)\) and use the same [Ansible](../../technologies/tools/ansible.md) play as the rest of the cluster.
* We are currently transitioning to Ubuntu Server.

We are currently in the process of moving the CentOS nodes to Ubuntu Server in order to become standard with the rest of the Lab and avoid. This effort is currently in progress but requires a significant rewrite of many current cluster [Ansible](../../technologies/tools/ansible.md) plays.

