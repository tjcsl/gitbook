# Updating DNS Records

TJ's DNS records are served by our primary nameserver `ns1.tjhsst.edu (198.38.16.40)`
and our secondary nameserver `ns2.tjhsst.edu (198.38.16.41)`. The records
themselves are stored in a git repository on [GitLab](gitlab.tjhsst.edu).

To update a DNS record, you must go through the following steps:

* Clone the git repository
* Create a new branch with the name of the record you are updating
    * If you are changing RDNS then just do `[record]_rdns`
    * For example, `ion.tjhsst.edu_rdns`
* Edit the correct zone file (most likely tjhsst/tjhsst.edu)
* Push your branch and verify that the build passes
* Create a merge request and mark the lead sysadmins + faculty sponsors as reviewers
* If your merge request looks good, it will be merged as soon as possible
    * If 24 hours passes with no response, ping the reviewers on Slack

Happy updating!
