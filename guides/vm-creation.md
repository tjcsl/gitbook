---
description: Or how I learned to stop worrying and love the machine
---

# VM Creation

## Prerequisites&#x20;

* Root permission on a [VM Host](../machines/vm-servers/) server.&#x20;
* Read (at least) access to the DNS and DHCP gitlab repos

## Steps

### Network

Each VM needs to connect to the network.  Determine which VLAN the VM should be on based on its purpose.  Most should be on [1600](../machines/vlans.md#1600), but publicly accessible services go in [1802](../machines/vlans.md#1802). L2 VLANs are all associated with exclusive L3 subnets, so be sure to note the IP address range that goes with the VLAN.

{% hint style="info" %}
For a list of all VLANs [see VLANs](../machines/vlans.md).
{% endhint %}

Pick an IP Address that is unused from the correct subnet.  **Do Not Reuse IP Addresses**.&#x20;

> The internet doesn't like it when DNS goes down or you change IPs&#x20;
>
> \-- Mr. Morasca

Be sure that an IP is not a conflict:

```
remote.tjhsst.edu> host <IP>
Host <IP>.in-addr.arpa. not found: 3(NXDOMAIN)
remote.tjhsst.edu> ping <IP>
PING <IP> ([ip]) 56(84) bytes of data.
^C
--- <IP> ping statistics ---
2 packets transmitted, 0 received, 100% packet loss, time 1028ms
```

This is a good result.  Now add this IP to DNS.  Make an `A` record for IPV4, `AAAA` for IPV6, and `PTR` record for reverse DNS (one for v4, one for v6).

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}

With this IP, we can now generate a MAC address.  Use the `create_mac.sh` script, passing the IP as the only argument. It takes the IP, removes all `.` to make it one number, then converts that to hex and adds `02:00:` to the beginning.

Add this MAC to [DHCP](../technologies/networking/dhcp.md).  Make sure it is added in the correct subnet. For example, the IP 198.38.16.150 should go inside this subnet block:

```
## Server subnet ##
subnet 198.38.16.0 netmask 255.255.254.0 {
```

### Define VM

Connect to a [VM Host ](../machines/vm-servers/)server as root.

First we need to create the RBD image

```
rbd --user libvirt create --pool virtual-machines --size <SIZE> <IMGNAME>
```

Specify size in megabytes/gigabytes/terabytes with M/G/T suffixes, respectively.  A good default size is 50G

XML files are stored in a git repo at `/qemuconfig/<HOST NAME>/`.  `cd` into this directory and dump the config for an existing VM to use as the base for the new VM.  `virsh dumpxml <EXISTING> > <NEW>.xml`

Generate a UUID by running `uuidgen` and replace the UUID in the XML file with this new one. **Don't** change the UUID for Ceph.  While editing the file, also make any changes needed to CPU count or RAM for the VM.  Also change the RBD image to the one you just made.

```markup
<name>[NAME]</name>
<uuid>[UUID]</uuid>
[...]
<devices>
    <emulator>/usr/bin/qemu-system-x86_64</emulator>
    <disk type='network' device='disk'>
        <driver name='qemu' type='raw'/>
        <auth username='libvirt'>
            <secret type='ceph' uuid='{private-do not change}'/>
        </auth>
        <source protocol='rbd' name='virtual-machines/[IMGNAME]'>
            <host name='198.38.17.84' port='6789'/>
        </source>
        [...]
    </disk>
    [...]
    <interface type='bridge'>
        <mac address='[MAC]'/>
        [...]
    </interface>
</devices>
<seclabel type='dynamic' model='armor' relabel='yes'>
    <label>libvirt-[UUID]</label>
    <imagelabel>libvirt-[UUID]</imagelabel>
</seclabel>
```

Add and commit the changes in git.

Start the VM with `vish define <NAME>.xml && virsh start <NAME>`

### Install OS

Run `virsh domdisplay <NAME>` on the host to find what port VNC is using (you will need to add 5900 to the port displayed, so if the result is `vnc://localhost:4` that really means `:5904)`

```
your-computer> ssh -L <PORT>:localhost:<PORT> <user>@remote.tjhsst.edu
remote.tjhsst.edu> kinit <user>/root
remote.tjhsst.edu> ssh -L <PORT>:localhost:<PORT> root@<VM HOST>
```

Now connect to `localhost:<PORT>` with your preferred VNC client.

If your network configuration works, it should [netboot](../technologies/networking/netboot.md).  Navigate the menu to install the latest Ubuntu release the lab is using.  The preseed URL is `steeltoe.tjhsst.edu/sp`

### Postinstall

Edit `/etc/ssh/sshd_config`:

```yaml
PermitRootLogin yes
```

`systemctl restart sshd` to apply changes.

Once the OS is installed, properly passcard it
