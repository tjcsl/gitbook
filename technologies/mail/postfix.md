# Postfix

At the heart of the mail system is the excellent mail server Postfix. It was chosen over alternatives for its robustness, ease of configuration, and ease of use.

## Upstream Documentation

[http://www.postfix.org/docs.html](http://www.postfix.org/docs.html) contains a variety of how-tos and FAQS. In general, this documentation and reputable resources on the Internet are enough to diagnose any issue with Postfix.

Source code for postfix is available at [https://github.com/vdukhovni/postfix](https://github.com/vdukhovni/postfix).

## Configuration

Postfix configuration is accomplished through several files in `/etc/postfix/`. Here we will outline their general structure. After changing most configuration files, Postfix must reload its configuartion files with the command `postfix reload`.

### main.cf

This configuration file tends to be the longest, as it houses nearly all of the options one might be concerned with. As this file is commented, there is no need to duplicate its function here.

### master.cf

This configuration file describes how the Postfix daemon should run and determines which ports it should be listening on for connections and how to handle those connections. The default format of this file is perfectly fine, with only two additions necessary in our setup.

The first is the amavis line, which is configured for spam filtering. The second is the line beginning with 127.0.0.1. This second line accepts mail on an alternate port and delivers the mail directly, bypassing spam filtering. This is the port to which amavisd-new delivers mail. If amavisd-new delivered mail to the standard postfix port, all messages would be put into an endless loop of spam filtering.

### aliases

This configuration file describes aliases, and is located in `/etc/aliases`. When an alias is defined, mail is no longer delivered to a mailbox \(if there is one\), and is instead forwarded to the addresses listed in the aliases file.

To update this file, edit it, `scp` it to other mail servers, then run `newaliases` on all mail servers. Changes take effect immediately upon running `newaliases`.

## Certificate changes

Change the cert for postfix/dovecot and restart \(not just reload\).

## Running

Running Postfix is relatively easy, with most utility coming from `postfix` and `systemd`. The following `postfix` commands are recognized:

* `check`

Checks the configuration files without starting or stopping Postfix. Useful to check if you've made mistakes on a production machine.

* `flush`

Flushes the mail queue, forcing Postfix to try delivering all mail that has accumulated in the queue.

* `reload`

Causes Postfix to reload its configuration files and restart processes nicely.

Typically the daemon is started and stopped with `systemctl start postfix` and `systemctl stop postfix`.

Other commands of interest include:

* `mailq`, which lists all mail currently stored in the queue. You can also get this list with `postqueue -p`. If you want this list in json, you can run `postqueue -j`.
* `postmap -s`, which allows administrators to view all database elements
* `postalias -s /etc/aliases`, which shows a list of all aliases in `/etc/aliases`

## Logging

Postfix sends all its logging information to syslog, which usually appends it to the file /var/log/mail.log. Examining this file is often key to troubleshooting issues with the mail system. This log is very helpful for mail administrators.

