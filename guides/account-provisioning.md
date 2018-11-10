# Account Provisioning

Even though we have integrated authentication for accounts, user provisioning still needs to occur in every system independently.

## Windows/Active Directory

The Windows IT staff takes care of this. Sysadmins \(starting with the graduating class of 2006\) are moved into a separate ou \(organizational unit\) before this occurs and will have their accounts preserved, but passwords are still subject to expire annually.

## Unix accounts

We have a script called `create_user.sh` that provisions all necessary accounts.  It takes the username. first name. and last name as the arguments.

It:

* Generates an LDIF
* Export the LDIF
* Adds the LDIF to openldap1
* Creates an AFS home directory
* and resets the Kerberos principal password to the default

### Manual Provisioning

{% hint style="warning" %}
Use of the manual steps is not recommended.
{% endhint %}

#### Creating AFS User

First, you need to create an AFS user account. Make sure you are authenticated with your /admin principal.

```text
pts createuser <username>
```

The command should give an output similar to:

```text
User <username> has id 12345678
```

If the user already has an AFS user account, run the following command in order to obtain an ID.

```text
pts examine <username>
```

#### Creating LDAP User

Next, you need to add the account to LDAP. First, generate an LDIF file using the guide at [NSS LDAP Templates](https://github.com/tjcsl/gitbook/tree/0ed8086a38339b7cf231d8d987eae570d21ccd8f/technologies/aauthentication/nss-ldap/templates.md). Run the command below after you have created an LDIF file.

```text
ldapadd -h openldap1 -Y GSSAPI -f <ldif file>
```

Below is an example LDIF file. Make sure you replace first name, last name, uidNumber, and graduation year!

```text
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

```text
cd /afs/csl.tjhsst.edu/.students/.20XX/
vos create -server openafs3 -partition vicepa -name 20XX.<username> -maxquota 1048576
vos backup 20XX.<username>
fs mkmount <username> 20XX.<username>
fs mkmount <username>/yesterday 20XX.<username>.<backup>
fs sa <username> <username> rlidwka
vos release students.20XX
```

## Historical Provisioning

The update\_nssldap.sh script \(authored by William Yang; see [NSS LDAP](https://github.com/tjcsl/gitbook/tree/0ed8086a38339b7cf231d8d987eae570d21ccd8f/technologies/authentication/nss-ldap/README.md)\) handled user provisioning in [AFS](../technologies/storage/afs/) and [NSS LDAP](https://github.com/tjcsl/gitbook/tree/0ed8086a38339b7cf231d8d987eae570d21ccd8f/technologies/authentication/nss-ldap/README.md) It depends on Identity Synchronization for Windows \(ISW\) to be working properly. Because of the way AD accounts are recreated every year, it was recommended to follow this procedure when it was time to create accounts every fall. The idsync command below was part of ISW and was located in `/opt/SUNWisw/bin` on the ISW server \(usually the primary NSS LDAP server\). All idsync commands are required to have options `-D -w -q`. For our purposes, we used `-D 'cn=Directory Manager' -w - -q -`

**Requirements**

* Have an /admin credential
* Be added as a user on the AFS servers \(`bos adduser <server> <username>.admin`\)
* Know/have ready the NSS Manager password



**Account Creation**

* Stop ISW synchronization \(`idsync stopsync -D 'cn=Directory Manager' -w - -q -`\)
* Reassociate accounts \(`idsync resync -D 'cn=Directory Manager' -w - -q -`\)
* Restart ISW synchronization \(`idsync startsync -D 'cn=Directory Manager' -w - -q -`\)
* Run the `update_nssldap.sh` script \(make sure you've read the section on this script in [NSS LDAP](https://github.com/tjcsl/gitbook/tree/0ed8086a38339b7cf231d8d987eae570d21ccd8f/technologies/authentication/nss-ldap/README.md)!\). Make a backup \(`backup.sh`\) before and after running this script, just in case.



