# DNS

DNS (Domain Name System) is a system for resolving domain names to IP addresses, Kerberos Realms, and other information. It is currently used in the Computer Systems Lab to provide forward and reverse name resolution as well as location of Kerberos and AFS realms.

## Structure

DNS is structured as a hierarchical tree. At the root of the tree is a DNS Zone called . or the root zone. The root zone is managed by thirteen nameservers (creatively called a-m.root-servers.net) about half of which are anycasted to locations around the world.

While they form the root of DNS; the root nameservers don't actually know how to answer a query for say www.tjhsst.edu (198.38.18.202). Rather, they know where to go to find the answer. In the case of www.tjhsst.edu, the answer is to go to the EDUCAUSE nameservers which manage the .edu TLD (Top-Level Domain). The .edu nameservers in turn, still don't know the answer, but they know where the answer is, in this case, with ipa1 and ipa2, which are the authoritative nameservers for the tjhsst.edu domain. Finally, either ipa1 or ipa2 will tell you that www.tjhsst.edu has the IP 198.38.18.202 (assuming that is the information you were asking for).

### Recursive vs Authoritative Nameservers

There are two types of nameservers that are generally involved in DNS. A single DNS Server can have one or both of these roles although it is considered best practice to separate them for security.

An authoritative nameserver has a database of some type which contains the records for a domain. In general, a domain will have one master nameserver on which the database is updated manually, and one or more slave nameservers which automatically transfer changes from the master.

A recursive nameserver has no knowledge or records of its own, but rather, resolves records for other systems and then caches the response so that it can respond to other queries about the same record without rerunning the lookup sequence outlined above. The amount of time records are cached is controlled by the TTL (Time To Live) which is specified by the Authoritative nameserver for the zone. A higher TTL places less load on the Authoritative nameservers but also means changes take longer to propagate. It is considered good practice to lower the TTL for a domain to around 5 minutes before making major changes to minimize the amount of time a mistake would be cached.

## CSL Layout

Both recursive and authoritative DNS for tjhsst.edu are currently provided by ipa1 and ipa2. ipa1 should always be a physical machine (we learned that the hard way), ipa2 can be a VM. The both run ISC BIND (Berkeley Internet Name Daemon), sometimes called `named` which is the name of the daemon.&#x20;

## Record Types

There are a number of different record types for the various types of information that can be stored in DNS.

### A

One of if not the most common record type, an A record simply maps a domain name to an IP address.

```
www.tjhsst.edu.    IN    A    198.38.18.202
```

One domain name can have more than one A record in which case most DNS servers will alternate which IP address they return. This is called a round-robin and is frequently used for load-balancing.

```
mail.tjhsst.edu.    IN    A    198.38.16.130
mail.tjhsst.edu.    IN    A    198.38.16.131
```

### AAAA

An AAAA record is almost identical to an A record except that it is used for IPv6 addresses instead.

```
www.tjhsst.edu.    IN    AAAA    2001:468:cc0:1802::202
```

### PTR

A PTR record functions in the reverse direction to an A record; it is used to map an IPv4 or IPv6 address back to a domain name. PTR records use a special domain (in-addr.arpa for IPv4 and ip6.arpa for IPv6) which is preceeded by the reversed IP address.

```
202.18.38.198.in-addr.arpa.    IN    PTR    www.tjhsst.edu.
2.0.2.0.0.0.0.0.0.0.0.0.0.0.0.0.2.0.8.1.0.c.c.0.8.6.4.0.1.0.0.2.ip6.arpa.    IN    PTR    www.tjhsst.edu.
```

### CNAME

A CNAME (Canonical Name) is used to alias one domain record to another. This is frequently used together with apache name-based virtual hosting. IMPORTANT NOTE - unlike many other record types which can share the same record (for example, tjhsst.edu has A, MX, NS, and SOA records), CNAMEs cannot share a record with any other record type and will override any other record defined. This is why tjhsst.edu has an A record for www's IP instead of being CNAME'd to www.

```
webmail.tjhsst.edu.    IN    CNAME    www.tjhsst.edu.
```

### NS

An NS (NameServer) record is used to delegate control of a subdomain to another nameserver or nameservers. For historical reasons, ipa1 and ipa2 are referred to as ns1 and ns2 in DNS records.

```
tjhsst.edu.    IN    NS    ns1.tjhsst.edu.
tjhsst.edu.    IN    NS    ns2.tjhsst.edu.
```

### MX

An MX record is used to tell sending mailservers where they can find the mailservers for a particular domain. Without MX records, the default assumption is to send mail to the same address as the domain which is frequently not wanted. MX Records contain both a domain name and a priority which can be used to setup a backup mailserver while still ensuring that mail is delivered to the main server if it is up. Mail Servers with equal priority are rotated similar to A record round-robins.

```
tjhsst.edu.    IN    MX    10 casey.tjhsst.edu.
tjhsst.edu.    IN    MX    10 smith.tjhsst.edu.
tjhsst.edu.    IN    MX    10 ponyexpress.tjhsst.edu.
```

### SRV

SRV (Service) records are used to map domain names to the servers and ports which provide a service under that domain name. Microsoft Active Directory for example, uses SRV records heavily to allow clients to locate the various Domain Services. In the CSL, we use SRV records to help systems locate our Kerberos services. Notice that these records do not have the IN (Internet) type.

```
_kerberos._tcp.csl.tjhsst.edu.        SRV 0 0 88 ipa1.tjhsst.edu.
_kerberos._tcp.csl.tjhsst.edu.        SRV 0 0 88 ipa2.tjhsst.edu.
```

### TXT

TXT records are simply used to store text. These are frequently used to test new record ideas before an official record type is created. For example, many SFP records are still provided as TXT records.

```
_kerberos.csl.tjhsst.edu.             TXT "CSL.TJHSST.EDU"
```

```
tjhsst.edu.    IN    TXT    "v=spf1 mx ~all"
```

### SOA

The SOA record for a domain provides administrative information for the domain. These include the serial number (used to help slave nameservers know when to update from the master), the default record TTL and expiration time, and the master nameserver address and domain administrator email address.

```
ns1.tjhsst.edu. hostmaster.tjhsst.edu. 1710301666 3600 600 1209600 86400
```
