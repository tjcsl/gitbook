# Administration

## IP Banning

fail2ban is sometimes a bit too aggressive.  After being fully banned, the ban lasts in the database for about 24 hours.  

fail2ban is a systemd service and can be restarted via the regular systemctl commands.

Run

```text
iptables -L f2b-sshd
```

to retrieve a list of all banned IP addresses.  You can view a log of fail2ban activity at `/var/log/fail2ban.log`   .

To unban an IP, run

```text
fail2ban-client set sshd unbanip <IP>
```

To ignore an IP until the next fail2ban restart, run

```text
fail2ban-client set sshd addignoreip <IP>
```

To ignore an IP permanent, edit the ignoreip directive in the tjcsl.conf file within the `ras` role on Ansible.  You can then deploy the edited file via Ansible.

## Updating

{% hint style="warning" %}
Please keep in mind the CSL [upgrade guidelines](../../policies/upgrade-policy.md) for production systems when deciding whether to upgrade RAS. When in doubt. ask someone more experienced and BACKUP DATA.
{% endhint %}

The remote access servers are like any other Ubuntu Server and can be upgraded via a regular `apt update && apt upgrade`.  It is recommended to upgrade the RAS servers via Ansible and to do so one at a time so that failed upgrades do not completely break access to the Lab.

