# Updating

GitLab releases updates to its Enterprise Edition software very regularly.  A major upgrade is always released the 22nd of every month.

{% hint style="warning" %}
Please keep in mind the CSL[ upgrade guidelines](../../../policies/upgrade-policy.md) for production systems when deciding whether to upgrade GitLab.  When in doubt, ask someone more experienced and BACKUP DATA.
{% endhint %}

To perform an upgrade,

{% hint style="info" %}
During an `apt upgrade`, GitLab performs a backup of the SQL database.
{% endhint %}

* Schedule a maintenance period
* Perform a backup of data either via a Ceph snapshot or gitlab-rake

  * gitlab-rake:

  As root on `gitlab`, run



  ```bash
  gitlab-rake gitlab:backup:create STRATEGY=copy
  ```

  to backup all repositories to /var/opt/gitlab/backups

  * Ceph snapshot:

  As root on a Ceph monitor, run the following to perform a snapshot of the Ceph image

  ```bash
  rbd snap create virtual-machines/gitlab@<SNAPSHOT NAME>
  ```

* Run the following to upgrade GitLab

```text
apt update && apt upgrade -y
```

* The above upgrade will take time to perform.  When the upgrade finishes, it should give you a success message.  After the upgrade successfully completes, it will take a few minutes for all workers to start up so in that time frame you may see a 502 when accessing the site.  

{% hint style="info" %}
LDAP login may temporarily not work in the minutes after a login. _**This is fine.**_ If more than 15 minutes have passed and GitLab is not working, perform debugging to fix the problem or rollback the upgrade as per the GitLab docs.
{% endhint %}

