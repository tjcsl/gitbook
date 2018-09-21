# Troubleshooting

Google. idk I'll add more when I encounter stuff.

## Slurm doesn't work

1. Try to reinstall slurm from package built in `/root/slurm-{version}/`
2. Restart `slurmd` on nodes and `slurmctld` on infosphere
3. Reboot everything \(`infosphere` is a VM, need to `virsh destroy infosphere && virsh start infosphere` on the `galapagos` VM server\).
4. Look at logs, Google

## X11 forwarding doesn't work

I know. It sucks.

## Can't find /cluster directory

You probably rebooted recently. Run `mount -a` as root on the affected node.

## Another problem

lol you're on your own.

