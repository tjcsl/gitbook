# Organization

## Teams

Work among the Sysadmins are split among a variety of teams, each working on a specific area of the Lab.

### Ion

The Ion team is responsible for the administration, maintenance, and development of [Ion](../services/ion/).

### Director

The Director team is responsible for the administration, maintenance, and development of [Director](../services/director/).  They also are responsible for ensuring the high availability of websites hosted on Director.

### Web Services

The Web Services \(or WWW\) team is responsible for maintaining the web presence of the Lab not supervised by other teams.  This includes tjhsst.edu and sysadmins.tjhsst.edu.  They are also responsible for \*.tjhsst.edu domains not supervised by other teams or by the Infrastructure Lead.  They are also responsible for managing TJ's proxy configuration file.

### Mail

The Mail team is responsible for maintaining TJ's mail servers, list servers, and webmail clients \(shared with Web Services\).

### Signage

The Signage team is responsible for maintaining TJ's [Signage displays](../services/signage/).  They work closely with the Ion team in this regard.

### Networking

The Networking team is responsible for managing the [CSL's network infrastructure](../technologies/networking/), including switches, networking connections, [OpenVPN](../technologies/networking/openvpn.md), [NTP](../technologies/networking/ntp.md), [DNS](../technologies/networking/dns/), and [DHCP](../technologies/networking/dhcp.md).  They are responsible for the smooth flow of network traffic.  They are also the point persons when diagnosing networking connections on CSL systems.

### Monitoring

The Monitoring team is responsible for [observability in the CSL](../technologies/monitoring/), including logging, alerts, and metrics.  They are responsible for maintaining systems that provide monitoring capability such as [Grafana](../technologies/monitoring/grafana.md) and Prometheus.

### Storage

The Storage team is responsible for the storage of data in the Lab including [Ceph](../technologies/storage/ceph/) and [OpenAFS](../technologies/storage/afs/openafs.md).  They are also responsible for the CSL's data backups.

### Documentation

The Documentation team is responsible for accurate, comprehensive, and well-written documentation for the Sysadmins.  They assist and strongly encourage other teams in documenting everything in both our [Runbooks](documentation/#runbooks) and this [Docsite](documentation/).

### Academic Services

The Academic Services team is responsible for maintaining software that is used by TJHSST classes.  This includes [Othello](../services/academic-services/othello/), the TJHSST AI Grader, and Tin.  Due to the presence of many services, there may be a sub-team for each service.

### Printing

The Printing team is responsible for [printing operations in the Lab](../services/printing/), including the CUPS server and the printers.

### Cluster

The Cluster team is responsible for maintaing [TJ's clusters](../services/cluster/) \(Borg and HPC\).

### Advanced Computing Hardware

The Advanced Computing Hardware Team is responsible for the maintenance of hardware within the Lab used for advanced computing, including GPUs.

### Understudy Coordinator

The Understudy Coordinator is responsible for leading the [Understudy program](understudies.md).  The Coordinator is responsible for primarily planning the structure and activities with the Understudy program.

## Infrastructure Lead

The Infrastructure Lead is one of the Lead Sysadmins who is responsible for broadly supervising all facets of the Lab's infrastructure.

The Infrastructure Lead is also responsible for:

* prioritizing work among the Sysadmins
* allocating work among the teams
* ensuring work is done in a timely manner
* ensuring best security practices and policies in the Lab
* setting abuse guidelines
* spearheading automation efforts
* maintaining the GitLab issue tracker 

The Infrastructure Lead provides recommendations and feedback on changes to the Lab's architecture or to substantial technical changes.

The Infrastructure Lead is **NOT** a person who takes on all responsibility.  Instead, the Lead delegates work. authority, and responsibility to other teams and people.

**Qualifications:**

* Has an extraordinary knowledge of the Lab and the relationship between its software, services, and technologies
* Has a broad range of expertise working with various aspects of the Lab's infrastructure
* Has shown an extraordinary level of dedication to the program and its mission/values
* Is organized

**Responsibilities**

* LDAP
* Kerberos
* VM servers
* iLOs
* Security
* CSL architectural decisions

## Lead Sysadmins

The Lead Sysadmins make up the Sysadmin Leadership Team together with the Faculty Sponsor and are the final decision-makers in the Sysadmins.  They make the final call with respect to team organization/membership, access requests, and all decisions related to the Lab.

They are appointed by the outgoing Lead Sysadmins with approval from the Faculty Sponsor.  

In another sense, the Lead Sysadmins are the Presidents.  They may appoint Junior Lead Sysadmins \(Vice Presidents\), if those people are expected to become Lead Sysadmins in the next year.

### Senior Sysadmins

Senior Sysadmins are sysadmins who are seniors.  By virtue of being a senior, they have no additional rights or responsibilities. Instead, by virtue of having served in the Lab for a long time, they often have the most experience in a specific area and offer a valuable perspective.

## Team Structure

**Lead\(s\)**

Leads are the Directly Responsible Individuals by default on a team.  They are responsible for serving as the primary point of contact with respect to the team.  If there is an incident relating to their team, the Lead\(s\) must be the one to report it.

Leads should:

* stay apprised of their team's work
* have extensive knowledge of the team's functional area
* supervise the work done by their team members
* report on their work to the broader Sysadmin team

> Apple coined the term "directly responsible individual" \(DRI\) to refer to the one person with whom the buck stopped on any given project. The idea is that every project is assigned a DRI who is ultimately held accountable for the success \(or failure\) of that project.
>
> They likely won't be the only person working on their assigned project, but it's "up to that person to get it done or find the resources needed."
>
> ... What's most important is that they're empowered.

> Source: [https://about.gitlab.com/handbook/people-operations/directly-responsible-individuals/](https://about.gitlab.com/handbook/people-operations/directly-responsible-individuals/)

**Deputy**

The Deputy \(or Deputy Lead\) is a backup to to the Lead\(s\) and defers to their opinion.  If the Lead\(s\) is/are not available, the Deputy should be able to temporarily take over.  A Deputy is only appointed if the Sysadmin has demonstrated competence and trust that would make him/her already qualified to be a Lead.

**Backup**

If their is no Deputy, a team has a Backup, who would be someone that can step in for a Lead in the Lead's absence.  The Backup is generally a Lead Sysadmin or a Sysadmin who has previously led that team.

**Team Members**

Team members are people who significantly contribute to a team's goals.  Passive involvement does not mean that someone is a team member.  They operate under the direction of the Team Lead.

