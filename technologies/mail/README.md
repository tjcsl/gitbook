# Mail

## Machines which provide this service

## History

The current **TJ email system** is an idea originally designed by Aman Gupta \(class of '04\). The original system was implemented by Andrew Deason \(class of '06\) with the help of several staff members, including Richard Washer and Susan Beasley. A second mail system, based on Zimbra, was configured by Lee Burton. The latest mail system was designed by Brandon Vargo.

Previously, the CSL only hosted mail for the CSL itself; mail that was only accesible to people with Systems Lab accounts. Today, lab-only mail does not exist anymore, and everyone gets their email off of the same system, all with @tjhsst.edu addresses.

## Overview

The current mail system consists of two primary email servers as well as a mailman server. The two primary email servers, casey and smith are named after the founders of UPS and FedEx, Jim Casey and Frederick Smith, respectively. The servers are virtually identical; only the hostname/mailname and network information is different. Each runs postfix, dovecot, and amavisd-new. The mailman server, lists, runs a standard mailman installation.

When mail is first received by the mail system, it is processed by postfix. If basic spam tests pass, the message is passed to amavisd-new for additional spam processing. If the message passes these tests, it is placed into another internal postfix queue. In general, mail will only be tagged instead of being discarded. At this point, postfix is configured to pass the message to dovecot for delivery. Both servers share a common NFS filestore. Dovecot is configured to handle locking.

## Authentication/Authorization

Authentication is handled by the kerberos PAM module, similar to most other systems in the lab. Dovecot is configured to user PAM.

Instead of using the Lab's LDAP tree for user information, account information is stored locally on each server in the form of standard UNIX accounts. A mail management script, described below, synchronizes the AD user information with the local UNIX accounts. The UID for the local UNIX account is a hash of the username. This allows each server to run the mail synchronization script independently and not worry about UID conflicts on the shared NFS storage.

## Postfix

## Amavis

Amavisd-new is used to scan all incoming and outgoing mail for virii and spam. It serves as an interface between the MTA, Postfix, and the various scanning utilities: Clam Antivirus for virus protection, and SpamAssassin for spam detection.

### Clam Antivirus

The ClamAV package is being used to scan for virii in email. It functions in two modes: first as a daemon that runs all the time and scans data fed to a socket, and also as a standalone command line utility, clamscan. In most circumstances we will be using the former due to its increased performance, falling to the latter only if the daemon fails.

The virus definitions are updated about five times a day off the Internet through the freshclam daemon. In this way, new virii in the wild will be detected early and stopped from entering the TJHSST mail system.

### SpamAssassin

SpamAssassin is used to scan all mail coming into the lab for its likelihood of spam, and all messages are given points for certain key phrases that are common in spam. With our configuration, very little user-level configuration is available. Additional rule files have been added under /etc/spamassassin/. SpamAssassin can be run as a daemon, but the mail system is configured to use the spamassassin tool directly.

### Configuration

Configuration of Amavis is relatively simple and commented well, thus its configuration file will not be repeated here. Virtually all configuration is found in the files in /etc/amavis/conf.d/. Files are processed in order.

### Running

Very little needs to be done to Amavis to get it running besides making sure that all necessary daemons are running, specifically clamd, freshclam, and amavisd-new. When Amavis receives a new mail on its socket, it will pass it through the virus scanner and then either discard the email or approve it and add an email header that indicates it was passed. It will then run the email through SpamAssassin and add any necessary headers indicating spam level. No spam will automatically be discarded; it is up to the user to filter as s/he wishes. Dovecot will automatically put spam in a spam folder.

Logging information is sent to syslog, which typically appends it to /var/log/mail.log. In this will be indicated whether the emails passed or were infected, and in the case of the latter, which virus/ii were involved.

## Mailman

E-mail distribution lists are provided on the domain lists.tjhsst.edu by Mailman, a highly popular software package. Most list administration takes place through an easy online interface at [https://lists.tjhsst.edu/](https://lists.tjhsst.edu/), though many functions are also available through email, such as list subscription and some message handling.

### Configuration

Configuration for this software package is minimal; the main parameters that must be specified are the urls and domains to be used. A modified version of the main postfix+amavisd-new configuration files are used. Integration with Postfix is achieved through the postfix-to-mailman.py script, which provides the necessary functionality.

### Running

Postfix pipes any email with a destination of lists.tjhsst.edu to this script, which runs the necessary Mailman commands to deliver the email. Mailman, of course, ends up sending mails back to Postfix, destined for the actual recipients. Postfix will then deliver these mails to the mail mail systems or forward them to the appropriate mail server.

A few daemons must be running for Mailman to function properly. The first is `qrunner`, which processes any mail in Mailman's queue. If this is not running, e-mails to lists will be added to the queue but will never be delivered to recipients. This is started by the mailman init script. The other necessary daemon is `apache`, which provides the necessary online interface for list administration.

## Dovecot

## Account Management

See [Account provisioning](https://github.com/tjcsl/gitbook/tree/b18eaea16346c14456040c32aa7980539eedfbc2/guides/account-provisioning.md) for information on running the synchronization script.

The `mail.py` script is a python script responsible for syncing the local user information with the Active Directory user database. This script should be run on all mail servers, even though there is a common file storage.

Users are stored in groups in AD according to whether a mail account is required and the desired quota. This information is compared to a local user database, `/etc/mailmanage/users`, a CSV file that lists all mail users past and present. Users in AD that are not in the local database will have accounts created. Users in the local database but not in AD will have their account disabled unless an override is specified. Currently it is necessary to edit this file directly to override a quota or keep an account enabled.

CSV fields:

1. The username
2. The quota, before the multiplication value is taken into effect
3. Source of the user account
   * There are two values: ldap and override. A ldap source means the user comes from ldap and user attributes should be overridden on sync. A type of override means the local values should not be replace by those in AD. This is useful for overriding quotas or keeping accounts enabled that would otherwise be disabled.
4. Enabled/disabled state

The script is commented, so its internal workings will not be copied here. However, of particular note is the `perform_actions` variable at the top of the parameters section of the script. This variable should be set to 0 for an initial test run, in order to perform a sanity check of the output. The script will print all actions it will perform, including exact commands that it will run. Setting this variable to 1 will result in the script actually running these commands.

## Storage

They're stored somewhere, that's for sure. During the \[\[Cephpocalypse\]\] they were put in local storage on Casey. If you happen to know where all the mail files are stored now, please help by filling this section out!

