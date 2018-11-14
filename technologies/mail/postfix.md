# Postfix

At the heart of the mail system is the excellent mail server Postfix. It was chosen over alternatives for its robustness, ease of configuration, and ease of use.

## Configuration
Postfix configuration is accomplished through several files in /etc/postfix/. Here we will outline their general structure. After changing most configuration files, Postfix must reload its configuartion files with the command postfix reload.

### main.cf
This configuration file tends to be the longest, as it houses nearly all of the options one might be concerned with. As this file is commented, there is no need to duplicate its function here.

### master.cf
This configuration file describes how the Postfix daemon should run and determines which ports it should be listening on for connections and how to handle those connections. The default format of this file is perfectly fine, with only two additions necessary in our setup. The first is the amavis line, which is configured for spam filtering. The second is the line beginning with 127.0.0.1. This second line accepts mail on an alternate port and delivers the mail directly, bypassing spam filtering. This is the port to which amavisd-new delivers mail. If amavisd-new delivered mail to the standard postfix port, all messages would be put into an endless loop of spam filtering.

### aliases
This configuration file describes aliases, and is located in /etc/aliases. When an alias is defined, mail is no longer delivered to a mailbox (if there is one), and is instead forwarded to the addresses listed in the aliases file.

To update this file, edit it, `scp` it to other mail servers, then run newaliases on all mail servers. Changes take effect immediately upon running `newaliases`.

## Certificate changes
Change the cert for postfix/dovecot and restart (not just reload)

## Running
Running Postfix is relatively easy, with most utility coming from postfix <command>. The following commands are recognized:

* check

Checks the configuration files without starting or stopping Postfix. Useful to check if you've made mistakes on a production machine.

* start

Starts the Postfix mail system.

* stop

Stops the Postfix mail system in a nice way. Processes stop naturally.

* abort

Stops the Postfix mail system abruptly. This one should be avoided if possible.

* flush

Flushes the mail queue, forcing Postfix to try delivering all mail that has accumulated in the queue.

* reload

Causes Postfix to reload its configuration files and restart processes nicely.

Typically the daemon is started and stopped with `/etc/init.d/postfix start` and `/etc/init.d/postfix stop`.

Other commands of interest include `mailq`, which lists all mail currently stored in the queue. This can be flushed using `postfix flush`, as indicated above.

Postfix sends all its logging information to syslog, which usually appends it to the file /var/log/mail.log. Examining this file is often key to troubleshooting issues with the mail system.
