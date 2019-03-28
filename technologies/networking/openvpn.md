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
2019djones changed the 
{% endhint %}

Run the commands below, first setting the `$USERNAME` variable to the username of the person you are generating the certificates for.

```text
USERNAME=<username>
cd /root/
./add_vpn_user --no-passphrase $USERNAME
mkdir $USERNAME
cp /etc/ssl/keys/$USERNAME.crt $USERNAME/
cp /etc/ssl/keys/$USERNAME.key $USERNAME/
cp 2017ewang/ca.crt $USERNAME/
cp 2017ewang/ta.key $USERNAME/
cp 2017ewang/openvpn.conf $USERNAME/
```

Edit openvpn.conf and replace 2017ewang with the username of the person you are generating the certificates for. Then,

```text
zip -r $USERNAME.zip $USERNAME
```

Finally, give USERNAME.zip to the user.
