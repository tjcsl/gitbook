# Supervisord

**supervisord** is a system that allows its users to control a number of processes on UNIX-like operating systems.

[GitHub repo](https://github.com/Supervisor/supervisor)

## Managing Configuration

**supervisord** is managed via a configuration file at `/etc/supervisor/supervisord.conf`  \(or applicable sub directories\).

**supervisor** has a CLI interface which administrators can use to control programs managed by **supervisord.**

{% hint style="warning" %}
It is important to understand the distinction between `reread` and `update` . These commands have DIFFERENT FUNCTIONS. `reread` scans a configuration file for changes, but an `update` IS needed afterwards.
{% endhint %}

