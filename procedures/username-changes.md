# Username Changes

Use this page as a checklist for updating usernames \(such as in the case of a legal name change\)

The following items need to be updated when a username change is requested:

1. Active Directory \(handled by Windows IT staff\)
2. Rename [AFS](../technologies/storage/afs/) username \(pts\) \(ex: `pts rename bnhouston bnferrick`\)
3. Navigate to rewrite user location \(ex: `cd /afs/csl.tjhsst.edu/staff`\)
4. Remove old mountpoint \(ex: `fs rmmount bnhouston`\)
5. Rename [AFS](../technologies/storage/afs/) home directory volume name \(vos\) \(ex: `vos rename staff.bnhouston staff.bnferrick`\)
6. Remake [AFS](../technologies/storage/afs/) home directory mountpoint read-write \(ex: `fs mkmount bnferrick staff.bnferrick -rw`\)
7. Release read-write user location \(ex: `vos release staff`\)
8. \[NSS LDAP\] \(gecos, homeDirectory\) \(ex: look at `getent passwd bnferrick` and fix it if necessary\)
   * Change username \(uid\) and name \(cn, sn, givenName\) fields
   * If user in in any LDAP groups, make sure to change the memberUid fields to match the new username.
9. If the user is a sysadmin or has an active CSL principal for some other reason, update the name of that principal as well as any .k5login entries for gaining root
10. Mail \(the sync script will make a new account, but you have to manually move the old data to the new user's home directory on the mail system\)

