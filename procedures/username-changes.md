# Username Changes

Use this page as a checklist for updating usernames \(such as in the case of a legal name change\)

{% hint style="warning" %}
Only use this page as a cheklist for updating usernames such as in the case of a legal name change
{% endhint %}

The following items need to be updated when a username change is requested:

1. Active Directory \(handled by Windows IT staff\) \(LOCAL domain\)
2. Rename [AFS](../technologies/storage/afs/) username \(pts\) \(ex: `pts rename bnhouston bnferrick`\)
3. Navigate to rewrite user location \(ex: `cd /afs/csl.tjhsst.edu/staff`\)
4. Remove old mountpoint \(ex: `fs rmmount bnhouston`\)
5. Rename [AFS](../technologies/storage/afs/) home directory volume name \(vos\) \(ex: `vos rename staff.bnhouston staff.bnferrick`\)
6. Remake [AFS](../technologies/storage/afs/) home directory mountpoint read-write \(ex: `fs mkmount bnferrick staff.bnferrick -rw`\)
7. Release read-write user location \(ex: `vos release staff`\)
8. \[NSS LDAP\] \(gecos, homeDirectory\) \(ex: look at `getent passwd bnferrick` and fix it if necessary\)
   * Change username \(uid\) and name \(cn, sn, givenName\) fields
   * If user in in any LDAP groups, make sure to change the memberUid fields to match the new username.
9. Rename the user's Kerberos principal \(`rename_principal bnhouston bnferrick` in the `kadmin` interface\)
10. If the user is a Sysadmin, update any `.k5login` entries for gaining root
11. Postfix/dovecot should create a new mail directory for the user. Migrate old data as necessary.
12. Update the Ion database to use the new username
13. Update Director to use the new username

