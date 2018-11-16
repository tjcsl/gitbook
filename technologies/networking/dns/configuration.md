# DNS Configuration

## Getting DNS Configuration

The DNS configuration is stored in git and can be found on [gitlab]. For access to the repository ask any DNS admin. You will then want to fork the repository and clone it to your home directory.

## Configuration Layout

* db/ - contains standard Nameserver zone files
  * db/localhost - the zone file for the localhost zone
  * db/0.0.127.in-addr.arpa - the zone file for the 127.0.0.0/8 subnet
* named.ca - bootstraps the nameserver with the addresses of the root nameservers
* named.conf - the main named configuration file
* tjhsst/ - tjhsst forward and reverse zone files
* tjhsst.conf - included by named.conf; configuration for TJ zones
* tjpartnershipfund/ - zone information for the TJ partnership fund domains
* tjpartnershipfund.conf - included by named.conf; configuration for PF zones

## Editing Configuration

In tjhsst/ is where most changes will be made. The file named tjhsst.edu contains most of the forward records, A,AAAA,CNAME,TXT,SRV,AFSDB, etc. An example entry looks like this:

```
galapagos.csl                  IN      A       198.38.17.45
                               IN      AAAA    2001:468:cc0:1600:226:55ff:fe2c:2336
galapagos                      IN      CNAME   galapagos.csl
```

You will also then need to update the PTR records for those IPs. They are stored in files in tjhsst/revpub/ by netblock (/24 for an IPv4 PTR and /64 for an IPv6 PTR). So for galapagos, you would want to edit 17.38.198.in-addr.arpa and 1600.cc0.468.2001.ip6.arpa. IMPORTANT - do not forget the . at the end of the server's FQDN. Without this, BIND will automatically append the zone name to the end of the name given.

**17.38.198.in-addr.arpa**

```
42    IN    PTR    galapagos.csl.tjhsst.edu.
```

**1600.cc0.468.2001.ip6.arpa**

```
6.3.3.2.c.2.e.f.f.f.5.5.6.2.2.0 IN    PTR    galapagos.csl.tjhsst.edu.
```

## Committing Changes

Commit the file locally with git commit -a (commits all changes to files already in the index). Enter a useful commit message. If you are making many changes, consider making a series of commits. The best type of commit message starts with a short string representing what it is you changed followed by a colon, then a short description of what you've done to it. Example:

```
shodan: make CNAME shodan -> shodan.csl
```

Finally, you need to push your changes back to gitlab and make a merge request. One of the DNS admins will then review your changes and push them to the nameserver.

## Merging People's Changes to the Server

If everything checks out, you can merge the changes and push to gitlab. The gitlab hook should automatically check your changes and tell [ns1] to update . See `/root/update_ns1.sh` on ns1 for more details.

[ns1]: ../../../machines/other/pitcairn.md
[ns2]: ../../../machines/vm-servers/galapagos.md
