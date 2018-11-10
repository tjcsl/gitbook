# LDAP

**OpenLDAP** is the LDAP server currently used to provide both NSS LDAP \(on openldap1 and openldap2\) as well as Iodine's student information database \(on iodine-ldap\). The actual OpenLDAP daemon is called slapd \(Stand-alone LDAP daemon\) and many of the OpenLDAP server management utilities begin with slap \(eg, slapcat, slapadd\) as a result.

## Installation

We currently run OpenLDAP with Kerberos and SASL support to allow for GSSAPI passwordless authentication. To install this configuration on Gentoo, run:

```text
echo "net-nds/openldap kerberos sasl overlays" >> /etc/portage/package.use
echo "dev-libs/cyrus-sasl kerberos ldap" >> /etc/portage/package.use
emerge -a openldap
```

Note that at the time of this writing, OpenLDAP will only compile with Kerberos support using the MIT implementation. Therefore, if the system you are using for an OpenLDAP server is running the Heimdal implementation of Kerberos, you will need to uninstall it and install the MIT implementation before installing Cyrus-SASL and OpenLDAP.

## Configuration

### slapd.conf

/etc/openldap/slapd.conf is the primary configuration file for the OpenLDAP server. The config file is well-commented, however, the following configuration options are notable. For security reasons, the exact values for some options are not listed here.

* sasl-realm: This should be set to the kerberos realm that the server is in; in our case, almost certainly CSL.TJHSST.EDU.
* sasl-host: This should be set to the hostname of the server.
* sasl-secprops: we set this to "noanonymous,noplain,noactive" to disable non-GSSAPI SASL authentication methods.
* sasl-regexp: These lines are used to map Kerberos principals to LDAP DNs as part of the SASL binding process. They are processed in order and the first matching regex is used.
* index: These lines indicate which Indexes should be maintained by OpenLDAP to speed up searches. Note that if these options are changed, slapd must be stopped and slapindex must be used to regenerate the database indexes or searches that attempt to use the new Indexes may fail.

### /etc/conf.d/slapd

/etc/conf.d/slapd is used by Gentoo to setup the environment for running the OpenLDAP server.

```text
#This tells slapd to listen for both regular and SSL connections.
OPTS="-h 'ldap:// ldaps://'"

#This allows slapd to find the keytab that it will use to validate GSSAPI authentication requests
#The keytab should contain the principal ldap/<Server FQDN> and should readable only by the user slapd runs as (by default, ldap)
export KRB5_KTNAME="FILE:/etc/openldap/ldap.keytab"</nowiki>

# increase the maximum number of open file descriptors
rc_ulimit="-n 4096"
```

### /var/lib/openldap-data/DB\_CONFIG

```text
#These options are used to tune the cache size and connection parameters
set_cachesize 0 2097152 0
set_lk_max_objects 1500
set_lk_max_locks 1500
set_lk_max_lockers 1500
#This flag means that slapd will automatically garbage-collect transaction logs
#Note that if this flag is set, it is critical to have regular backups or recovery of a corrupted database
#will be difficult or impossible
set_flags DB_LOG_AUTOREMOVE
```

### Replication

We currently use one-way replication to propagate a copy of the NSS LDAP information from openldap1 \(the master server or provider\) to openldap2 \(the slave server or consumer\). To configure replication, first add the following lines to slapd.conf on the master server in the appropriate sections:

```text
#This maps principals of the form ldap/<FQDN>@CSL.TJHSST.EDU to
#cn=<FQDN>,ou=consumers,dc=csl,dc=tjhsst,dc=edu
sasl-regexp             uid=ldap/([^,]+),cn=CSL.TJHSST.EDU,cn=gssapi,cn=auth
                        cn=$1,ou=consumers,dc=csl,dc=tjhsst,dc=edu

#Replication options
overlay syncprov
syncprov-checkpoint 100 10
syncprov-sessionlog 100
```

For each slave server, create an entry in `ou=consumers` using an LDIF similar to:

```text
dn: cn=openldap2,ou=consumers,dc=csl,dc=tjhsst,dc=edu
cn: openldap2
objectClass: top
objectClass: device
```

Then add the following lines to end of slapd.conf on the slave server:

```text
syncrepl rid=123
        provider=ldap://openldap1.tjhsst.edu:389
        type=refreshOnly
        interval=00:00:05:00
        searchbase="dc=csl,dc=tjhsst,dc=edu"
        schemachecking=off
        bindmethod=sasl
        saslmech=gssapi
        realm=csl.tjhsst.edu
        authcid="ldap/openldap2.tjhsst.edu@CSL.TJHSST.EDU"

#This line redirects any ldapadd/ldapmodify commands made against openldap2 to openldap1
updateref ldap://openldap1.tjhsst.edu
```

## See also

* [NSS LDAP Templates](nss-ldap/templates.md)
* [NSS LDAP](nss-ldap/)

