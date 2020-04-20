# Updating

TJ's DNS records are served by our primary nameserver `ns1.tjhsst.edu (198.38.16.40)` and our secondary nameserver `ns2.tjhsst.edu (198.38.16.41)`. The records themselves are stored in a Git repository on [GitLab](https://github.com/tjcsl/gitbook/tree/935f10b2595581ad854feca84c606b0a3940f668/services/dns/updating/gitlab.tjhsst.edu).

## For Sysadmins

To update a DNS record, you must go through the following steps:

* Clone the git repository
* Create a new branch with the name of the record you are updating
  * If you are changing RDNS then just do `[record]_rdns`
  * For example, `ion.tjhsst.edu_rdns`
* Edit the correct zone file \(most likely `tjhsst/tjhsst.edu`\)
* Bump the serial number. The serial is in the form `YYYYMMDDNN`
  * The serial should reflect the day of the change and a two digit number indicating the change number on that day. The change number begins with `00` and increments by 1 with every change. This allows up to 100 changes to DNS per day.
  * **It is important you follow the convention** because otherwise our DNS resolution could fail.
* Push your branch and verify that the build passes
* Create a merge request and mark the Lead Sysadmins + Faculty Sponsor as reviewers
* If your merge request looks good, it will be merged as soon as possible
  * If 24 hours passes with no response, ping the reviewers on Slack

Happy updating!

## For DNS Admins

* Once you merge to master, GitLab CI will go through a process to test your change and deploy it to `ns1`.
* If there is a green checkmark, then the CI steps have completed and the changes have been deployed to `ns1`.

