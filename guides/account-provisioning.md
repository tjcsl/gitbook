# Account Provisioning

Even though we have integrated authentication for accounts, user provisioning still needs to occur in every system independently.

## Windows/Active Directory
The Windows IT staff takes care of this. At the time of writing, all users are deleted and recreated at the beginning of every school year. Sysadmins (starting with the graduating class of 2006) are moved into a separate ou (organizational unit) before this occurs and will have their accounts preserved, but passwords are still subject to expire annually.

## Unix accounts
The update_nssldap.sh script (authored by William Yang; see [NSS LDAP](../technologies/authentication/nss-ldap/README.md)) will handle user provisioning in [AFS](../technologies/storage/afs/README.md) and [NSS LDAP](../technologies/authentication/nss-ldap/README.md) It depends on [[Identity Synchronization for Windows]] (ISW) to be working properly.  Because of the way AD accounts are recreated every year, it is recommended to follow this procedure when it is time to create accounts every fall.  The idsync command below is part of ISW and is located in `/opt/SUNWisw/bin` on the ISW server (usually the primary NSS LDAP server).  All idsync commands are required to have options `-D -w -q`. For our purposes, we use `-D 'cn=Directory Manager' -w - -q -`

##### Requirements

* Have an /admin credential
* Be added as a user on the AFS servers (`bos adduser <server> <username>.admin`)
* Know/have ready the NSS Manager password

##### Account Creation
* Stop ISW synchronization (`idsync stopsync -D 'cn=Directory Manager' -w - -q -`)
* Reassociate accounts (`idsync resync -D 'cn=Directory Manager' -w - -q -`)
* Restart ISW synchronization (`idsync startsync -D 'cn=Directory Manager' -w - -q -`)
* Run the `update_nssldap.sh` script (make sure you've read the section on this script in [NSS LDAP](../technologies/authentication/nss-ldap/README.md)!). Make a backup (`backup.sh`) before and after running this script, just in case.

### Individual Account Provisioning

#### Simple Check for User Existence

A simple way to check if a user already has an account is to check to see if their home directory exists. If access denied is returned or you are able to change to their home directory, the home directory exists. Otherwise, the home directory probably doesn't exist.
```
cd ~<username>
```

#### Creating AFS User

First, you need to create an AFS user account. Make sure you are authenticated with your /admin principal.

```
pts createuser <username>
```

The command should give an output similar to:

```
User <username> has id 12345678
```

If the user already has an AFS user account, run the following command in order to obtain an ID.

```
pts examine <username>
```

#### Creating LDAP User

Next, you need to add the account to LDAP. First, generate an LDIF file using the guide at [NSS LDAP Templates](../technologies/aauthentication/nss-ldap/templates.md). Run the command below after you have created an LDIF file.

```
ldapadd -h openldap1 -Y GSSAPI -f <ldif file>
```

Below is an example LDIF file. Make sure you replace first name, last name, uidNumber, and graduation year!
```
dn: uid=2017ewang,ou=2017,ou=students,ou=people,dc=csl,dc=tjhsst,dc=edu
cn: Eric Wang
description: 2017
displayName: Wang, Eric
givenName: Eric
uid: 2017ewang
sn: Wang
objectClass: inetOrgPerson
objectClass: top
objectClass: organizationalPerson
objectClass: person
objectClass: posixAccount
uidNumber: 00000000
gecos: Eric Wang
gidNumber: 2017
homeDirectory: /afs/csl.tjhsst.edu/students/2017/2017ewang
loginShell: /bin/bash
```

#### Adding AFS Volume

```
cd /afs/csl.tjhsst.edu/.students/.20XX/
vos create -server openafs3 -partition vicepa -name 20XX.<username> -maxquota 1048576
vos backup 20XX.<username>
fs mkmount <username> 20XX.<username>
fs mkmount <username>/yesterday 20XX.<username>.<backup>
fs sa <username> <username> rlidwka
vos release students.20XX
```

### Mail

/mnt/mail/mail.py will update accounts and account attributes, including quotas, based on data in AD. It should be run as root on all mail servers (currently casey and smith). Currently, accounts disabled or deleted in AD are disabled on the mail servers. See [[Email]] for more information on how this works.
