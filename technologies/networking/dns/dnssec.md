---
description: Domain Name System Security Extensions (DNSSEC)
---

# DNSSEC

## Background {#background}

DNSSEC uses [PKI](https://en.wikipedia.org/wiki/Public_key_infrastructure) to ensure the authenticity of DNS results by signing all DNS records for a domain with that domain's DNSSEC certificates.

DNSSEC requires the following DNS Resource Records \(RRs\):

> * **DNSKEY** Holds the public key which resolvers use to verify.
> * **RRSIG** Exists for each RR and contains the digital signature of a record.
> * **DS** - Delegation Signer – this record exists in the TLD's nameservers. So if example.com was your domain name, the TLD is "com" and its nameservers are `a.gtld-servers.net.`, `b.gtld-servers.net.` up to `m.gtld-servers.net.`. The purpose of this record is to verify the authenticity of the DNSKEY itself.

Source: [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-setup-dnssec-on-an-authoritative-bind-dns-server--2)​

## Setup {#setup}

### Master Server {#master-server}

Add the following lines to `/etc/bind/named.conf.options` within the `options` block:

{% code-tabs %}
{% code-tabs-item title="/etc/bind/named.conf.options \(excerpt\)" %}
```text
dnssec-enable yes;
dnssec-validation yes;
dnssec-lookaside auto;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Run `/etc/bind/generate_production_keys.sh`

{% code-tabs %}
{% code-tabs-item title="generate\_production\_keys.sh" %}
```bash
#!/bin/bash
# Keegan Lanzillotta, Oct. 2018 -- based on generate_testing_keys.sh
cd /etc/bind
if test "$(ls keys/)"
then
    echo Keys already exist in the keys/ directory.  Exiting...
    exit
fi
​
for f in tjhsst/tjhsst.edu tjhsst/revpub/*.arpa; do
    zone=$(basename $f)
    mkdir keys/$zone
    dnssec-keygen -a RSASHA256 -b 2048 -r /dev/urandom -K keys/$zone -n ZONE -f KSK $zone
    dnssec-keygen -a RSASHA256 -b 1024 -r /dev/urandom -K keys/$zone -n ZONE $zone
    dnssec-signzone -S -K keys/$zone -o $zone $f
done
```
{% endcode-tabs-item %}
{% endcode-tabs %}

This generates a Key Signing Key \(KSK\) keypair on line `13` and Zone Signing Key \(ZSK\) keypair on line `14` for each zone \(`tjhsst/tjhsst.edu` and `tjhsst/revpub/*.arpa`\). Line `15` then signs the Zone file, producing a `*.signed` file for each zone.

```bash
ns1 bind (master*) # ll tjhsst/revpub
total 1.9M
-rw-r--r-- 1 root bind 1.2K Sep 29 15:15 0000.cc0.468.2001.ip6.arpa
-rw-r--r-- 1 root bind 6.8K Oct  3 22:45 0000.cc0.468.2001.ip6.arpa.signed
-rw-r--r-- 1 root bind 5.5K Sep 29 15:15 1600.cc0.468.2001.ip6.arpa
-rw-r--r-- 1 root bind  57K Oct  3 22:45 1600.cc0.468.2001.ip6.arpa.signed
-rw-r--r-- 1 root bind 4.1K Sep 29 15:15 16.38.198.in-addr.arpa
-rw-r--r-- 1 root bind  58K Oct  3 22:45 16.38.198.in-addr.arpa.signed
[...continued...]
```

These `*.signed` files contain RRSIG records. We need to tell BIND to serve these records. To do this, edit `/etc/bind/tjhsst.conf` and change the `file` attribute for each zone to point to the signed file. For 

{% code-tabs %}
{% code-tabs-item title="example:/etc/bind/tjhsst.conf \(old\)" %}
```text
zone "tjhsst.edu" {
    type master;
    file "/etc/bind/tjhsst/tjhsst.edu";
    allow-query { any; };
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="/etc/bind/tjhsst.conf \(new\)" %}
```text
zone "tjhsst.edu" {
    type master;
    file "/etc/bind/tjhsst/tjhsst.edu.signed";
    allow-query { any; };
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Run `rndc reload` to load the edited configuration. To confirm that the RRSIG records are being served properly, `dig A ion.tjhsst.edu. @localhost +dnssec +multiline`. The output's `;; ANSWER SECTION` should contain a `RRSIG` record response below the `A` record response.

### Slave Server {#slave-server}

### Domain Name Registrar {#domain-name-registrar}

For the PKI element of DNSSEC, the keys we generated need to be signed by the domain registrar.

{% hint style="info" %}
This step only applies to the tjhsst.edu zone, because the other zones are not related to the domain registrar
{% endhint %}

When `/etc/bind/generate_production_keys.sh` ran, `dnssec-signzone` created files called `dsset-*` in `/etc/bind/`. The only one we need is `/etc/bind/dnsset-tjhsst.edu.`

{% code-tabs %}
{% code-tabs-item title="/etc/bind/dnsset-tjhsst.edu." %}
```text
tjhsst.edu.		IN DS 8092 8 1 0FFA63442D1158C84D789D1D612E78100B86B616
tjhsst.edu.		IN DS 8092 8 2 7EDA9DB2804E4D0F17E7C66C526234624F6D1755D127EC2C05F97A89 2502A010
```
{% endcode-tabs-item %}
{% endcode-tabs %}

These are the DS records for `tjhsst.edu`, and they need to be sent to the domain registrar to establish the trust tree.

