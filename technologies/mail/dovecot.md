# Dovecot

Dovecot proves IMAPs, POPs, and sieve support, in addition to being the local mail delivery agent. Dovecot receives mail from postfix and is responsible for delivering it to the user's mail directory. If dovecot is not running, postfix will queue mail.

Dovecot also supports quotas. If a user is over their soft quota, dovecot will give a warning but still store the message. If a user exceeds their hard quota, the mail message will be rejected.

The Dovecot documentation can be found at https://www.dovecot.org/documentation.

Source code for Dovecot can be found [on GitHub](https://github.com/dovecot/core).
