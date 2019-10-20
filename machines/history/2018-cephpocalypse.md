# 2018 Cephpocalypse

The **Cephpocalypse** was an event occurring in the fall of the 2018-2019 school year, when the Ceph cluster which hosted our central network storage went completely offline. This incident demonstrated the capability of the Sysadmin team and prompted us to start thinking about ways to remove that one point of failure \(say, through a backup system\).

The purpose of this document is to record our mistakes and remedial actions so that future generations may learn from them.

## Background

After delays in obtaining approval for the new storage servers, we finally received these new G10 servers.  In anticipation for the eventual transfer of our Ceph cluster to these new servers, we mounted the servers and prepared the new servers. One of those preparations was a needed upgrade to our production Ceph cluster that went horribly wrong.

## Cause

On a Sunday in mid-September of 2018, the Storage Lead began the process of of upgrading the component servers of our production Ceph cluster to latest major version.  We had been running jewel and we needed to get to mimic. The Storage Lead, in quick succession, upgraded these servers up two major releases. A later independent review suggests that this rapid upgrade of two major releases was to blame for the Cephpocalypse.

## Reactions

### From Sysadmins

When we received UptimeRobot notifications, we initially thought that the Storage Lead would be able to fix his mistake fairly quickly. When a fix did not materialize, 

### From other students

We were featured [on tjToday](https://www.tjtoday.org/24197/showcase/the-system-to-saving-syslab/). For most students not in the SysLab, 

### From Staff

Most TJ staff did not notice much disruption in our services since we were able to quickly restore our most public service, Ion.

## Remedial Actions

### Trying to fix Ceph

### Un-Cephing everything

### Moving things back to new Ceph

## What we learned

* It is important to keep off-site backups.
* It is nice to have multiple people know how Ceph operates.
* We lack contigency plans.
* Teamwork is important.
* We lack documentation.

## Results

After more than two excruciating weeks, the Storage Lead recovered data from the old cluster \(albeit partially corrupted\) and we began the process of moving everything back onto Ceph. This process ensued for the upcoming months.



