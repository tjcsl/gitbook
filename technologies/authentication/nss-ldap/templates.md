# NSS LDAP Templates

Below are example command and LDIFs for various common NSS LDAP operations. To run the below examples, you can either run the command and paste the LDIF contents into the terminal or you can place the contents into a file and run the command with the `-f <filename>` argument.

## Listing current group members

```text
getent group <groupname>
```

To get and LDAP entry, you can use the following command:

```text
ldapsearch -h openldap1 -Y GSSAPI -b <dn>
```

## Additions

The following templates are used for creating new instances of various types of objects. These examples should be run with the following command:

```text
ldapadd -h openldap1 -Y GSSAPI
```

### General User Account

This template is for creating a general-purpose user account; for example a new staff member or a guest account. By convention, we normally use the ID of the account's AFS user \(if applicable\) as the uidNumber. This can be retrieved using:

```text
pts examine ahamilto
```

```text
dn: uid=ahamilto,ou=2009,ou=students,ou=people,dc=csl,dc=tjhsst,dc=edu
cn: Andrew Hamilton
description: 2009
displayName: Hamilton, Andrew
givenName: Andrew
uid: ahamilto
sn: Hamilton
objectClass: inetOrgPerson
objectClass: top
objectClass: organizationalPerson
objectClass: person
objectClass: posixAccount
uidNumber: 1748
gecos: Andrew Hamilton
gidNumber: 2009
homeDirectory: /afs/csl.tjhsst.edu/students/2009/ahamilto
loginShell: /bin/bash
```

If the account is for a staff member/non-student, put 1984 \(faculty group\) as the gidNumber.

### Server User Account

This template is similar to the previous one except this user account is valid for servers and other restricted-access systems. By convention, we use the same uidNumber for this account as for the user's general access account.

```text
dn: uid=ahamilto,ou=sysadmins,dc=csl,dc=tjhsst,dc=edu
objectClass: posixAccount
objectClass: shadowAccount
objectClass: account
objectClass: top
uid: ahamilto
cn: ahamilto
uidNumber: 1748
gidNumber: 100
homeDirectory: /home/ahamilto
loginShell: /bin/bash
gecos: Andrew Hamilton
```

### Group

This template is for creating a new group. These groups exist on both general and restricted access systems.

```text
# allaccess, group, csl.tjhsst.edu
dn: cn=allaccess,ou=group,dc=csl,dc=tjhsst,dc=edu
memberUid: root
memberUid: ahamilto
gidNumber: 1337
cn: allaccess
objectClass: posixGroup
objectClass: top
```

### Organizational Unit \(OU\)

This template is for creating a new organizational unit.

```text
dn: ou=2006,ou=students,ou=people,dc=csl,dc=tjhsst,dc=edu
objectClass: top
objectClass: organizationalUnit
ou: 2006
```

## Modifications

The following templates are for modifying existing objects. These examples should be run with the following command:

```text
ldapmodify -h openldap1
```

### Adding a User to a Group

This template is for adding a user to an existing group.

```text
dn: cn=allaccess,ou=group,dc=csl,dc=tjhsst,dc=edu
changetype: modify
add: memberUid
memberUid: ahamilto
```

### Removing a User from a Group

This template removes a single user from a group. NOTE - be very careful when using this template via copy-paste as if you accidentally miss the last line, you will delete all of the memberUid attributes instead of the single targetted instance.

```text
dn: cn=allaccess,ou=group,dc=csl,dc=tjhsst,dc=edu
changetype: modify
delete: memberUid
memberUid: ahamilto
```

### Changing a User Attribute

If you accidentally insert a wrong attribute when creating an LDAP entry, you can use the following to change an attribute:

```text
dn: uid=2017ewang,ou=2020,ou=students,ou=people,dc=csl,dc=tjhsst,dc=edu
changetype: modify
replace: sn
sn: Wang
```

## Deletions

To delete an object from LDAP, use the following command and LDIF.

```text
ldapdelete -h openldap1
```

```text
cn=allaccess,ou=group,dc=csl,dc=tjhsst,dc=edu
```

