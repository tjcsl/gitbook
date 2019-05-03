# OpenVPN

Useful to access all the CSL machines from the comfort of your own home! Running [ansible](../tools/ansible.md) plays, ssh-ing without having to hop on [RAS](../../services/remote-access/) first, OpenVPN makes everything a breeze!

## History

We used to have unfettered OpenVPN access until ~~the Great Firewall of~~ FCPS decided to indiscriminately block all OpenVPN traffic through some sort of Packet Inspection starting in Fall 2018. Fortunately, we were able to get an exception for the CSL's OpenVPN server, so as of Spring 2019 it is back up.

## Troubleshooting

If you can connect to the VPN successfully, but are having issues accessing other machines, try running the following command on the server:

```text
/etc/init.d/net.tap0 restart
```

## Creating Certificates

{% hint style="warning" %}
2019djones changed the OpenVPN server to generate `.ovpn` files in Spring 2019.
{% endhint %}

Run the commands below, first setting the `$USERNAME` variable to the username of the person you are generating the certificates for.

```bash
USERNAME=<username>
cd /root/
./quick_add_vpn_user $USERNAME
```

After running the script and selecting the defaults, the `.ovpn` file will appear as `$USERNAME.ovpn`. That file is the only file necessary to connect with an OpenVPN client.
