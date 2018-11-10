# NSS LDAP

LDAP is the system currently used to store NSS \(Name Service Switch\) information for the UNIX passwd and group databases. It serves as a centralized supplement/replacement for the information stored in `/etc/passwd` and `/etc/group` including usernames, uids, homedirectory locations, group memberships, and login shells.

## History

Previously, the CSL used NIS to store network user information. However, when the decision was made to integrate CSL accounts and authentication with Windows Active Directory \(previously all CSL accounts were managed separately and required an application form to receive\), LDAP was chosen to replace NIS as the backend for the NSS database.

Integrated authentication using LDAP and [Kerberos](../kerberos.md) was initially deployed in lab 231 during the spring of 2006. Sun Directory Server 5.2 was used at the time, replicated from sol across what are now known as chuku and ekhi. During the summer following, LDAP was moved into a VMWare virtual machine known as daystar in order to run LDAP on a faster system. However, for reasons not completely understood, the VM subsequently developed problems during the fall of 2006 and resulted in NSS becoming painfully slow on both rockhopper \(at that time used for all of lab 231 and 16 LTSP nodes in the CSL\) and the rest of the CSL workstations. In order to remedy the situation, `/etc/passwd` was rapidly deployed as a flatfile across all affected systems. Hesiod was subsequently set up as the NSS database for the remainder of the school year and the beginning of the next.

During the winter of 2007-08, NSS was switched back to LDAP following various discussions. \(Need more information.\) LDAP was configured on chuku and mihr, running Sun Directory Server 6.

Upon reception of the [Sun Academic Excellence Grant](../../../machines/history/2008-sun-aeg.md), LDAP was moved into the LDOMs \(UltraSPARC-specific virtual machines\) ldap1 and ldap2 running on [Ohare](../../../machines/sun-servers/ohare.md) and Logan.

## Current

NSS LDAP is currently used by nearly all \*NIX systems managed by the CSL . It is run using OpenLDAP from the VMs [openldap1](../../../machines/vm-servers/galapagos.md) and [openldap2](../../../machines/sun-servers/vega.md).

## LDAP Server Software

OpenLDAP's slapd is currently run in one-way replication on openldap1 \(master\) and openldap2. It can integrate with nsswitch to provide all NSS databases although we currently only use it for the passwd and group databases. For detailed installation and configuration notes, see [OpenLDAP](../ldap.md).

### Service IP Caveats

The service IP for NSS LDAP is 198.38.16.59 \(ldap-sun.tjhsst.edu is the hostname; it remains what it is for historical reasons\). Note that neither openldap1 nor openldap2 will automatically grab this IP at boot. This IP must be manually added. This is so that the IP can be moved during maintenance; for example, if openldap1 is down for maintenance, the IP is moved to openldap2 so correctly configured clients will query openldap2 instead. If openldap1 is rebooted as part of maintenance, it will not also snag the IP as it comes up and cause a network service conflict.

In the event that there is a complete power outage and both systems are rebooted, obviously the IP will not be assigned to either of them at boot. Correctly configured clients will not be impacted as they will rapidly timeout 198.38.16.59 and fall back on openldap1 and openldap2 IP addresses.

## Directory Structure

* Everything below is relative to a root dn of `dc=csl,dc=tjhsst,dc=edu`.
* Groups are referenced by cn \(i.e. `cn=allaccess,ou=group,dc=csl,dc=tjhsst,dc=edu`\). Users are referenced by uid \(i.e. `uid=wyang,ou=2008,ou=students,ou=people,dc=csl,dc=tjhsst,dc=edu`\).

### ou=group

This subtree contains POSIX groups. Currently we have 3 major classes of groups:

* **Primary**: Each LDAP user's default group is one of these. There are a few subclasses:
  * Students/graduation year: The name of each group is TJ and then a two digit year, except in the case of 2000, which has name TJ2K. The gidnumber is the four digit graduation year.
  * "Adult" users: Staff members are assigned to group "faculty" with gidnumber 1984. Separating parents/external users into a separate group would be advisable, but they presently share the "faculty" group.
  * "users": This is presently used for all LDAP user entries in the ou=sysadmins subtree \(see below\).
* **Host/VM access control**: Each of these groups \(gid starting at 1000\) has the hostname of the system that it is designed to control access for. The group has a list of users that should be granted login access to that system; however, some of the systems listed may not necessarily have group control implemented, rendering the LDAP group's membership moot. The group is not used to grant root access on a system; that is controlled independent of LDAP at this time. The members of the "allaccess" group \(gid 1337\) are granted login access on all systems that have the LDAP group access control scheme configured.
* **System/service groups**: These are primarily used by the Solaris systems, but can also be used by others. It is sometimes easier to ensure that any particular group used by a system service will have the same ID across multiple systems by adding it to LDAP, hence the existence of this class of groups. These have gids starting at 5000.

There are some miscellaneous groups scattered in there.

### ou=people

This is the subtree used for the NSS passwd database on all general use systems. It is itself made up of several more sub-ous. The major ones are listed below. Others may be created as needed \(for example, the josti2008 ou contains temporary users that were created for a JOSTI 2008 interactive user experience in one particular presentation; although the users are still in LDAP, they cannot login because their Kerberos credentials are expired\).

#### ou=legacy

Most NIS users were loaded into this ou so legacy accounts could still have their uids looked up, and previous admins that retained their Kerberos accounts can still log in. Some NIS users were not imported because their username already existed in one of the other ous.

#### ou=parents

Parents that have login access for website editing or for whatever other purpose go here. Some parents may actually be in ou=legacy if they were created during the NIS era.

#### ou=special

This contains things that don't belong in any other category. Some system service users go here \(see "System/service groups" in the ou=group section above for why we do this\).

* **IMPORTANT**: Most users that are added here should also be added to the ou=special in ou=sysadmins \(see below\).

#### ou=staff

Well yes...faculty and staff have their own ou. Naturally.

#### ou=students

No student users go directly in here; they go into another subou under this ou by graduation year first. At time of writing, ous exist for 2006 through 2017.

### ou=sysadmins

This contains...well...sysadmins. The distinction from ou=people is primarily for access control on non-general-use servers.

#### ou=special

This is basically a copy of ou=special from above. The difference is that non-general-use servers are configured to see this ou=special, while general-use systems see the one in ou=people.

## Configuring LDAP on clients

### Ubuntu

We use `sssd`, configuration file to be added.

### Admin-only access

Follow the above directions, but wherever you see an `ou=people`, replace it with an `ou=sysadmins`. Only sysadmins with LDAP entries in ou=sysadmins will be able to access that system. Note that additional access control can, and is often, managed by using hostname groups \(i.e. the LDAP POSIX group named after the hostname of the system\). This is currently not done on Solaris systems, but is done on most Linux systems.

Note that if you are doing this, you will probably want to enable the pam\_mkhomedir module to automatically create home directories the first time an admin logs in. This can be done by adding the following line to `/etc/pam.d/system-auth` on linux systems:

```text
session     optional      pam_mkhomedir.so
```

On Centos, this can also be done using authconfig:

```text
authconfig --enablemkhomedir --update
```

## Administration/Management

Administration and management of NSS data is primarily accomplished through the LDAP command line utilities. In order to gain administrative access to NSS LDAP, four things are necessary:

* You must have an `ou=sysadmins` user entry \(eg: `uid=ahamilto,ou=sysadmins,dc=csl,dc=tjhsst,dc=edu`\)
* You must have a Kerberos /admin principal \(eg: `ahamilto/admin`\)
* The dn of your `ou=sysadmins` entry must be added as a member of `cn=ldapadmins,dc=csl,dc=tjhsst,dc=edu`
* The ldap utilities on the system you are using must support SASL GSSAPI binding

Once you have the above four items, you can kinit to your /admin principal and use the standard LDAP command line utilities to make changes. For examples of various changes see [NSS LDAP Templates](templates.md).

## See Also

Add links later

* [OpenLDAP](../ldap.md)
* [NSS LDAP Templates](templates.md)

