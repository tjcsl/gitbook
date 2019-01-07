# Troubleshooting

A big handy FAQ for all y'alls' Qs.

## Using

### What the heck is this?

The cluster is a cool bunch of computers wired together to have high parallel computing capability.

### How the heck do I use it?

Read [the main guide](slurm.md)

### I can ssh to infosphere but...

* Don't have a home directory
* Can't use slurm

  Then you need to ask a sysadmin for a cluster account.

### I want to run \[generic mpi executable\] but get an error!

```text
salloc -n[numprocs] [other_args] mpiexec [your_program]
```

### I want some software on the cluster

Are you sure it will benefit from running in parallel? Each cluster machine by itself is actually slower than the average laptop.

#### Yes I'm sure

Ok, you'll have to ask a sysadmin.

## Administration

This part is meant for people administrating the cluster.

### Slurm doesn't work

1. Try to reinstall slurm from package built in `/root/slurm-{version}/`
2. Restart `slurmd` on nodes and `slurmctld` on infosphere
3. Reboot everything \(`infosphere` is a VM, need to `virsh destroy infosphere && virsh start infosphere` on the `galapagos` VM server\).
4. Look at logs, Google

### Can't find /cluster directory

You probably rebooted recently. Run `mount -a` as root on the affected node.

### Someone just asked me for an account. How do?

```text
ssh root@infosphere
echo {username} | ./make_new_users.sh
```

### Someone just asked me to install custom software!

First, read up on the software to make sure it is a\) useful and b\) mostly ok to install. Then, follow [this guide](administration.md), copy existing ansible play and modify it.

### Another problem

Unfortunately, you're on your own.

