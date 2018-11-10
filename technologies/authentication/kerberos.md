# Kerberos

**Kerberos** is a network authentication service, with allows for authentication of passwords without transmitting the password itself. This page is not an explanation on Kerberos; for that, look at [RFC 4120](http://www.ietf.org/rfc/rfc4120.txt).

## Implementations

There are multiple implementations of Kerberos. In addition to the free ones, such as the MIT, Heimdal, and Shishi implementations, Microsoft has also made an implementation for Windows, and Sun Microsystems has an implementation for their Java programming language. Right now, all of the CSL's Kerberos servers run the MIT implementation.

## Server Layout

\[someone more knowledgeable fill this out please\]

### Outages

### Replication

## Cross-realm Authentication

Currently, we have a one-way cross-realm trust with the Windows Domain LOCAL.TJHSST.EDU \(which can be considered the same as a Kerberos Realm for purposes of this article\). This trust allows our systems \(which operate in the CSL.TJHSST.EDU realm\) to verify and trust user principals from the LOCAL Domain, but does not allow the LOCAL Domain Controllers to verify the identity of our systems \(which have keytabs in the CSL realm\). Note that this alone does not give access to AFS from windows tickets. For information on that, see [OpenAFS/cross-cell](https://github.com/tjcsl/gitbook/tree/bdcd86706fd3e9d4de4131330fe198fcc86f48ea/technologies/storage/afs/cross-cell/README.md).

To re-create this trust, first generate a long random string to use as a Trust Password. Then, on the primary KDC, create two principals, krbtgt/CSL.TJHSST.EDU@LOCAL.TJHSST.EDU and krbtgt/LOCAL.TJHSST.EDU@CSL.TJHSST.EDU. For security, it is recommended to add the attributes disallow-renewable, disallow-tgt-based, and disallow-forwardable to both principals. Finally, on a LOCAL domain controller, go to Admin Tools -&gt; Active Directory Domains and Trusts, and create a new two-way trust in the LOCAL domain using the same Trust Password. Alternatively, to setup the LOCAL domain side of the trust, you can use:

```text
netdom TRUST CSL.TJHSST.EDU /add /realm /passwordt:<password> /twoway
```

### History

Prior to the creation of the two-way trust, we had a one-way trust with the LOCAL.TJHSST.EDU Domain \(CSL.TJHSST.EDU trusting LOCAL.TJHSST.EDU\) to allow for users to login to CSL systems with their LOCAL domain passwords. This trust was created similar to the above example except with only the krbtbt/CSL.TJHSST.EDU@LOCAL.TJHSST.EDU principal being created on the primary KDC and only a one-way trust being created on the LOCAL domain controller. One downside to this system was that CSL systems needed to have computer accounts \(keytabs\) in the LOCAL domain \(since the LOCAL domain controller would not trust a CSL keytab to verify the identity of a client system\).

Around 2017, the two-way trust was destroyed by the school, citing security reasons \(What if sysadmin wanted to hack the school? They could more easily with the two-way trust maybe\).

In fall 2018, Cross-realm authentication was temporarily removed altogether due to concerns about the LOCAL domain being replaced by FCPS's Windows 10 machines. It was quickly reinstated after a massive flood of password reset requests.

## Client Configuration

### `/etc/krb5.conf`

This is the primary configuration file for a kerberos client. On Solaris systems, this file is located at `/etc/krb5/krb5.conf`. On OpenBSD systems, this file is located at `/etc/kerberosV/krb5.conf`.

Add or modify the following lines in the `[libdefaults]` section. This will set the default realm of the system to CSL.TJHSST.EDU and set a bunch of other desirable options. \(someone want to explain more idk why most of these are here\)

```text
[libdefaults]
    default_realm = CSL.TJHSST.EDU
    forwardable = true
    proxiable = true
    dns_lookup_realm = true
    dns_lookup_kdc = true
    udp_preference_limit = 1
```

Add the following lines to the `[realms]` section to allow the clients to find the KDCs for each server.

```text
[realms]
    CSL.TJHSST.EDU = {
        admin_server = kerberos.tjhsst.edu
        auth_to_local = RULE:[1:$1@$0](^.*@LOCAL\.TJHSST\.EDU$)s/@.*$//
        auth_to_local =  DEFAULT
    }
    LOCAL.TJHSST.EDU = {
        kdc = tj07.local.tjhsst.edu
        kdc = tjvdc1.local.tjhsst.edu
    }
```

Add the following lines to the `[domain_realm]` section to allow clients to map DNS names to kerberos realms.

```text
[domain_realm]
    tjhsst.edu = CSL.TJHSST.EDU
    .tjhsst.edu = CSL.TJHSST.EDU
    csl.tjhsst.edu = CSL.TJHSST.EDU
    .csl.tjhsst.edu = CSL.TJHSST.EDU
    local.tjhsst.edu = LOCAL.TJHSST.EDU
    .local.tjhsst.edu = LOCAL.TJHSST.EDU
```

Add the following to the `[appdefaults]` section so that the pam\_krb5 module will not attempt to authenticate system accounts.

```text
[appdefaults]
    pam = {
        minimum_uid = 1000
    }
```

### `/etc/krb5.keytab`

This file serves as an equivalent to a password to allow computers and services to authenticate themselves and acquire Kerberos tickets. On Solaris systems, this file is located at /etc/krb5/krb5.keytab and on OpenBSD systems, this file is located at /etc/kerberosV/krb5.keytab. The permissions on this file should be 0400 for security reasons since with this file it is possible to spoof the identity of the system to the KDCs.

A keytab can be generated by using the command:

```text
ktutil -k /etc/krb5.keytab get -p <username>/admin host/<FQDN>
```

where `<username>` is the username of a user with Kerberos admin privileges \(you know if you have them\), and `<FQDN>` is the fully qualified name of a host, for example `ras1.csl.tjhsst.edu`.

Alternatively, use the commands:

```text
kadmin
addprinc -randkey host/<FQDN>
ktadd -k /etc/krb5.keytab host/<FQDN>
```

Note that if you are generating a keytab for a system other than the one that you are on, you will want to change the path after -k to someplace besides the current system's keytab.

### PAM

_**WARNING:**_ editing PAM/login files without fully understanding what is going on carries a significant risk of either locking yourself out of the system or leaving the system open to unauthorized logins. If you are unsure, ask someone before making changes. In addition, always leave a root terminal open and test logging in and gaining root before completely logging out of the system to avoid locking yourself out. Most of this stuff should be already set up by netboot preseeds/managed by Ansible anyway, only reference this if you really need to.

In order to allow users to actually log in using their Kerberos passwords, pam\_krb5 must be added to the PAM Auth stack. We use a combination of raw `pam_krb5` and `sssd` to control logins. On Ubuntu systems, we modify `/etc/pam.d/common-auth`. On Arch/Gentoo/other systems, we modify `/etc/pam.d/system-auth`.

```text
auth    [success=3 default=ignore]       pam_krb5.so minimum_uid=1000
auth    [success=2 default=ignore]       pam_unix.so nullok_secure try_first_pass
auth    [success=1 default=ignore]       pam_sss.so use_first_pass

auth    requisite                        pam_deny.so
auth    required                         pam_permit.so
```

How this works is that in case any module succeeds, it skips directly to `pam_permit` as defined in the `success` parameter.

Same thing in `/etc/pam.d/common-password` or the root `password` directive module:

```text
password        requisite                        pam_pwquality.so retry=3
password        [success=3 default=ignore]       pam_krb5.so minimum_uid=1000 try_first_pass use_authtok
password        [success=2 default=ignore]       pam_unix.so obscure use_authtok try_first_pass sha512
password        sufficient                       pam_sss.so use_authtok

password        requisite                        pam_deny.so
password        required                         pam_permit.so
```

We also need to use the `pam_krb5.so` session module. Add this to `/etc/pam.d/common-session` on Ubuntu, or `/etc/pam.d/system-auth` or whatever has the root `session` directives on other systems:

```text
session optional pam_krb5.so minimum_uid=1000
session required pam_unix.so
session optional pam_sss.so
```

For backend systems which do not need to allow logins from the LOCAL.TJHSST.EDU Domain, \(I have no idea someone fill this in please\). In addition, on remotely accessible systems, the option fail\_pwchange should be added to the pam\_krb5 lines to prevent users from resetting their passwords remotely.

### `/root/.k5login`

In order to allow Admins to gain root on a system without needing to know the root password to it, we create /root principals and then add them to this file. They can then use ksu and the password to their /root principal to gain root on the system. The format of this file is a list of complete principal names, one per line. The following example is taken from [infosphere](../../services/cluster/):

```text
2018wzhang/root@CSL.TJHSST.EDU
2019okulkarn/root@CSL.TJHSST.EDU
2019djones/root@CSL.TJHSST.EDU
2019jduvall/root@CSL.TJHSST.EDU
pewhite/root@CSL.TJHSST.EDU
```

### `/etc/ssh/sshd_config`

To enable SSH passwordless login between systems, uncomment or add the following line to `/etc/ssh/sshd_config` and restart `sshd`:

```text
GSSAPIAuthentication yes
```

This will also allow users with `ksu` access to two systems to `ksu` or `kinit` on one and then ssh as root to the other system without requiring a password.

## Server Configuration

To configure a system as a Kerberos KDC; first configure /etc/krb5.conf and /etc/krb5.keytab as described in the \#Client Configuration Section. Then continue below.

\(Someone who knows about setting up kerberos should fill this section out, livedoc seems very out of date\)

### `/etc/krb5.conf`

### `/etc/krb5.keytab`

### `/etc/inetd.conf`

### Heimdal?

### Service Configuration

## Debugging

### Issues with SSH passwordless login

Normally, the GSSAPI mechanism is used to authenticate someone to a machine who has already authenticated to another machine in the lab. This uses their existing Kerberos credentials to prove who they are, and it also forwards their Kerberos tickets to the new machine, so they can use them for accessing [AFS](../storage/afs/). So far, this has been seen to work fine on workstations.

There have been issues in the past when a reverse-ip lookup returns a different host than that specified in the keytab. To get around this, make sure there is only one main entry in DNS for a single IP, and CNAME any other names to that address if you want more names.

### External Links

* [RFC for the Kerberos V Standard](http://www.ietf.org/rfc/rfc4120.txt)
* [MIT Kerberos Homepage](http://web.mit.edu/kerberos/www/)
* [The Moron's Guide to Kerberos](https://web.archive.org/web/20070427040044/http://www.isi.edu/~brian/security/kerberos.html)

