# Account Structure

There are three different accounts that each TJHSST student has:

* FCPS domain
* LOCAL domain
* CSL domain

## FCPS Domain

The FCPS domain is controlled by the FCPS IT department.  Students use their FCPS domain password to access:

* Blackboard
* SIS StudentVue
* FCPS Google Apps for Education
* FCPS computers \(those running Windows 10\)

Password resets on the FCPS domain are handled on the first day of school in homeroom \(on a teacher laptop\).  Questions should be directed to [techteam@tjhsst.edu](mailto:techteam@tjhsst.edu).

## LOCAL Domain

The LOCAL domain is controlled by the TJ Tech Team.  Students use the LOCAL domain to access:

* TJ computers \(those running Windows 7\)
* Proxy access \(zeus.tjhsst.edu\)

Sysadmins may use the LOCAL domain to authenticate to GitLab, Lounge, and other CSL services that do not have CSL Kerberos authentication enabled.  Passwords on the LOCAL domain can be reset on TJ workstations running Windows 7.  Questions should be directed to [techteam@tjhsst.edu](mailto:techteam@tjhsst.edu).

{% hint style="info" %}
The LOCAL domain password is **NOT** a password used for students to authenticate to Ion.   Prior to August of 2018, it was used as another form of authentication to Ion but LOCAL authentication was removed due to confusion about CSL passwords.  Teachers still authenticate to Ion with their LOCAL passwords.
{% endhint %}

## CSL Domain

The CSL domain is controlled by the TJHSST Student Systems Administrators.  We use Kerberos to handle authentications in the CSL realm \(CSL.TJHSST.EDU\).

{% page-ref page="../technologies/authentication/kerberos.md" %}

Students use their CSL domain password to access:

* Ion
* Director
* Webmail
* Remote Access Servers
* Workstations
* and other CSL resources

Passwords are prompted to be reset the first time you login to Ion.  Questions should be directed to [sysadmins@tjhsst.edu](mailto:sysadmins@tjhsst.edu).

