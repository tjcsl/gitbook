# 2017 VM Disaster

## What Happened

We ran too much on VMs, so when we rebooted a VM server to update it the entire lab went down. Specifically, NS1 was the main point of failure.

## What resulted

* NS1 is on a physical machine
* We use IPs instead of hostnames in QEMU configs

