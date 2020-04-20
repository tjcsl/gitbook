# Mail

## Machines which provide this service

## History

The current **TJ email system** is an idea originally designed by Aman Gupta \(class of '04\). The original system was implemented by Andrew Deason \(class of '06\) with the help of several staff members, including Richard Washer and Susan Beasley. A second mail system, based on Zimbra, was configured by Lee Burton. The latest mail system was designed by Brandon Vargo.

Previously, the CSL only hosted mail for the CSL itself; mail that was only accesible to people with Systems Lab accounts. Today, lab-only mail does not exist anymore, and everyone gets their email off of the same system, all with @tjhsst.edu addresses.

## Overview

The current mail system consists of two primary email servers as well as a mailman server. The two primary email servers, casey and smith are named after the founders of UPS and FedEx, Jim Casey and Frederick Smith, respectively. The servers are virtually identical; only the hostname/mailname and network information is different. Each runs postfix, dovecot, and amavisd-new. The mailman server, lists, runs a standard mailman installation.

When mail is first received by the mail system, it is processed by postfix. If basic spam tests pass, the message is passed to amavisd-new for additional spam processing. If the message passes these tests, it is placed into another internal postfix queue. In general, mail will only be tagged instead of being discarded. At this point, postfix is configured to pass the message to dovecot for delivery. Both servers share a common NFS filestore. Dovecot is configured to handle locking.

## Authentication/Authorization

Authentication is handled as described in our [runbooks](../../general/documentation/runbooks.md).

## Postfix

{% page-ref page="postfix.md" %}

## Amavis

Amavisd-new is used to scan all incoming and outgoing mail for virii and spam. It serves as an interface between the MTA, Postfix, and the various scanning utilities: Clam Antivirus for virus protection, and SpamAssassin for spam detection.

When Postfix receives mail on port 25, it is passed to Amavis for screening on port 10024, and is re-injected into Postfix on port 10025 for final delivery.

### Clam Antivirus

The ClamAV package is being used to scan for virii in email. It functions in two modes: first as a daemon that runs all the time and scans data fed to a socket, and also as a standalone command line utility, clamscan. In most circumstances we will be using the former due to its increased performance, falling to the latter only if the daemon fails.

The virus definitions are updated about five times a day off the Internet through the freshclam daemon. In this way, new virii in the wild will be detected early and stopped from entering the TJHSST mail system.

### SpamAssassin

SpamAssassin is used to scan all mail coming into the lab for its likelihood of spam, and all messages are given points for certain key phrases that are common in spam. With our configuration, very little user-level configuration is available. Additional rule files have been added under /etc/spamassassin/. SpamAssassin can be run as a daemon, but the mail system is configured to use the spamassassin tool directly.

SpamAssassin can be trained - move spam mails to a folder and ham mails to another folder \(or keep them in Inbox\) and, for instance:

```text
sa-learn --no-sync --progress --spam /mail/[USERNAME]/Maildir/.Junk/{cur,new}
sa-learn --no-sync --progress --ham /mail/[USERNAME]/Maildir/{cur,new}
sa-learn --sync
```

### Configuration

Configuration of Amavis is relatively simple and commented well, thus its configuration file will not be repeated here. Virtually all configuration is found in the files in /etc/amavis/conf.d/. Files are processed in order.

### Running

Very little needs to be done to Amavis to get it running besides making sure that all necessary daemons are running, specifically clamd, freshclam, and amavisd-new. When Amavis receives a new mail on its socket, it will pass it through the virus scanner and then either discard the email or approve it and add an email header that indicates it was passed. It will then run the email through SpamAssassin and add any necessary headers indicating spam level. Currently, Amavis is configured to drop mail that was flagged as infected by ClamAV and deliver mail that have bad headers or have been flagged as spam by SpamAssassin \(to a spam folder, but that is handled by Dovecot\). Regardless, if there is any flag on the message, the message is copied to `/var/virusmails`.

Logging information is sent to syslog, which typically appends it to /var/log/mail.log. In this will be indicated whether the emails passed or were infected, and in the case of the latter, which virus/ii were involved.

## Dovecot

{% page-ref page="dovecot.md" %}

## Storage

Mail is stored as described in our [runbooks](../../general/documentation/runbooks.md).

