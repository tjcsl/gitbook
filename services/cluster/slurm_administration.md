# Cluster/Slurm Administration

This page is intended to serve as a guide for Sysadmins who need to administrate the Slurm system running on the HPC cluster. If you're a regular user, this information probably won't be very interesting to you.

## Accounts vs Users

The Slurm accounting system separates the ideas of Accounts and Users, which is slightly confusing at first. When you look at it from the higher-level functioning of Slurm though, these concepts make sense.

An **Account** is a method of controlling allowed resources and accounting for resources for a user or a group of users. For example, if you have a specific student group which you wanted to give elevated resource allowances to, you could create an Account for that group and attach their Users to that Account. Perhaps most importantly, names of Accounts are arbitrary, and don't have to match LDAP/Kerberos usernames.

In contrast, a **User** is purely meant to map Linux accounts (pulled from LDAP) to a Slurm account. **The usernames of Slurm Users MUST MATCH the person's username in LDAP**.

## Account/User Creation

All of the cluster nodes use standard CSL NSS-LDAP for authentication and authorization to cluster machines (the login node, compute nodes, and GPU node), but Slurm must have a User registered in its accounting system for that user to be able to run jobs using Slurm. Since Slurm authenticates users based on their Linux username, no extra passwords or LDAP configuration is necessary; once a user has their account added to the Slurm database, they should be able to seamlessly connect to the login node (infosphere) using their normal credentials and be able to run Slurm jobs without extra authentication.

`sacctmgr` is the tool used to manage Slurm users and accounts. To manage accounts, you must be root on infosphere or any other node of the cluster.

### Creating an Account

Right now, since different users may have different resource requirements, the current policy is to create a different Account for each User who wants to use the cluster. To do so, run the following:

```
sacctmgr add Account (username)
```

### Creating a User

After you've created a user's account, you can then add a User attached to that account:

```
sacctmgr add User Accounts=(username) (username)
```

### Creating a Cluster Home Directory

For speed, the cluster uses a separate user storage system than most other public-facing systems (which use AFS). On all cluster systems, user home directories are located under `/cluster`. Users' home directories are currently not automatically created due to issues with the pam_mkhomedir.so module and SELinux, so you have to manually create the user a home directory:

```
cp -r /etc/skel /cluster/(username)
chown -R (username) /cluster/(username)
```
