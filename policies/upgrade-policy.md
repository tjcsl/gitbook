# Upgrade Policy

## General

Sysadmins should be aware of the effects of failures to critical production services. Therefore, it is recommended \(and highly highly encouraged\) to restrict updates to these services to a minimum. Depending on the criticality of the service and the extent of the updates, these updates should be:

* restricted to periods of low usage
* restricted in scope as to not obstruct normal operation
* minimized as to not cause wide-scale damage should the upgrade fail

Additionally, for upgrades to highly critical production systems, a consultation should be made with the Lead Sysadmins and other Sysadmins relying on such systems.

Examples of critical production services include underlying systems that provide infrastructure for other services like Ceph, networking, and DNS.

## The Gist

Be careful upgrading production services.

