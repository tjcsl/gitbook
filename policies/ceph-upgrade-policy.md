# Ceph Upgrade Policy

## Policy

Due to the events of the 2018 Data Incident \(Cephpocalyse\), no Sysadmin shall upgrade any machine serving as an OSD or monitor for the Ceph production cluster unless all the following conditions are met:

* They have recorded a specific plan for the Ceph upgrade consistent with the Plan section below and posted it in the sysadmins channel on [Slack](../technologies/tools/slack.md).
* The plan has received approval from the Faculty Sponsor.
* The plan has received approval from the Lead Sysadmins.
* They have consulted with all other domain leads that rely on the Ceph cluster.
* The upgrade will take place during a period of time when CSL services are not being used heavily \(a school break, etc\) \(or a waiver has been granted\).
* They have ensured that any or all backups have been taken before proceeding with the upgrade \(or a waiver has been granted\).

### Waiver

For the purposes of the above conditions, a waiver may only be granted by all Lead Sysadmins and the Faculty Sponsor, jointly.  This waiver should be in the form below:

```text
Nonwithstanding the Ceph Upgrade Policy, the described Plan may proceed without [list condition from above].  We acknowledge the risks that this may entail.

- <FACULTY SPONSOR APPROVAL>
- <LEAD SYSADMIN 1>
- <LEAD SYSADMIN 2>
```

### Plan

A Plan should contain the following information:

* The name of the Sysadmin\(s\) performing the upgrade
* The Sysadmin\(s\) role in the upgrade
* A completely detailed listing of specific steps being done to perform the upgrade including all commands being applied
* Detailed contingencies in case of the failure of the upgrade
* Measurements of success
* A statement of impact should the upgrade fail
* Description of any or all backups of data on the Ceph cluster
* Timeframe for the upgrade
* The individuals who should be made aware of the upgrade



