---
description: The TJ mail service
---

# Mail

Your TJHSST Email Account is accessible form anywhere in the world with an Internet connection. You can use any of the following methods to access it.

## TJ Webmail

The easiest way to access your TJ Email is to use the TJ Webmail System at [https://webmail.tjhsst.edu](https://webmail.tjhsst.edu). This is powered by the free and powerful RainLoop webmail system. To log in, simply enter your TJ username and password.

## From a Mail Client

An alternative way \(perferred by many sysadmins or people with multiple email accounts\) is to access your TJ Email from an external mail client. The following sections describe the settings your client will need.

### IMAP

IMAP is a protocol that allows mail clients to securely download mail from a mail server. It also allows mail clients to sync their state \(folders, read messages\) with the mail server and other IMAP devices.

You will need to use the following settings for IMAP:

| Field | Value |
| :--- | ---: |
| Server | imap.tjhsst.edu |
| Port | 993 |
| Encryption Type | SSL/TLS |
| User ID | TJ username without @tjhsst.edu at the end |
| Password Type | Normal Password Authentication |

### SMTP

In addition to IMAP, you will also need some way to send mail from your mail client. That's where SMTP comes in. SMTP \(stands for **S**imple **M**ail **T**ransfer **P**rotocol\) is a way for mail clients to securely send mail to a mail server.

You will need to use the following settings for SMTP:

| Field | Value |
| :--- | ---: |
| Server | smtp.tjhsst.edu |
| Port | 465 |
| Encryption Type | STARTTLS |
| User ID | TJ username without @tjhsst.edu at the end |
| Password Type | Normal Password Authentication |

### POP3

Some older mail clients only support POP3 as a replacement for IMAP. We recommend using IMAP whenever possible \(your email is automatically backed up\), but POP3 is also available using the following settings:

You will need to use the following settings for IMAP:

| Field | Value |
| :--- | ---: |
| Server | mail.tjhsst.edu |
| Port | 993 |
| Encryption Type | SSL/TLS |
| User ID | TJ username without @tjhsst.edu at the end |
| Password Type | Normal Password Authentication |

