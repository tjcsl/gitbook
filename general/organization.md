# Organization

## Hierarchy

### Lead Sysadmins

The Lead Sysadmins make up the Sysadmin Leadership Team together with the Faculty Sponsor and are the final decision-makers in the Sysadmins. They make the final call with respect to team organization/membership, access requests, and all decisions related to the Lab.

They are appointed by the outgoing Lead Sysadmins with approval from the Faculty Sponsor.

In another sense, the Lead Sysadmins are the Presidents. They may appoint Junior Lead Sysadmins (Vice Presidents), if those people are expected to become Lead Sysadmins in the next year.

Lead Sysadmins are also responsible for coordinating the [Understudy program](understudies.md).

### **Team Lead(s)**

Leads are the Directly Responsible Individuals by default on a team. They are responsible for serving as the primary point of contact with respect to the team. If there is an incident relating to their team, the Lead(s) must be the one to report it.

Leads should:

* stay apprised of their team's work
* have extensive knowledge of the team's functional area
* supervise the work done by their team members
* report on their work to the broader Sysadmin team

> Apple coined the term "directly responsible individual" (DRI) to refer to the one person with whom the buck stopped on any given project. The idea is that every project is assigned a DRI who is ultimately held accountable for the success (or failure) of that project.
>
> They likely won't be the only person working on their assigned project, but it's "up to that person to get it done or find the resources needed."
>
> ... What's most important is that they're empowered.
>
> Source: [https://about.gitlab.com/handbook/people-operations/directly-responsible-individuals/](https://about.gitlab.com/handbook/people-operations/directly-responsible-individuals/)

### **Team Members**

Team members are people who significantly contribute to a team's goals. Passive involvement does not mean that someone is a team member. They operate under the direction of the Team Lead.

Team Members are often Junior Admins, but could also be other Team Leads interested in helping.

## Roles

These delegate responsibility for important tasks that apply to the Lab as a whole.

### Infrastructure Lead

The Infrastructure Lead is one of the Lead Sysadmins who is responsible for broadly supervising all facets of the Lab's infrastructure.

The Infrastructure Lead is also responsible for:

* prioritizing work among the Sysadmins
* allocating work among the teams
* ensuring work is done in a timely manner
* setting abuse guidelines
* spearheading automation efforts
* maintaining the GitLab issue tracker&#x20;

The Infrastructure Lead provides recommendations and feedback on changes to the Lab's architecture or to substantial technical changes.

The Infrastructure Lead is **NOT** a person who takes on all responsibility. Instead, the Lead delegates work. authority, and responsibility to other teams and people.

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

### Networking

The Networking team is responsible for managing the [CSL's network infrastructure](../technologies/networking/), including switches, networking connections, [OpenVPN](../obsolete/openvpn.md), [NTP](../technologies/networking/ntp.md), [DNS](../technologies/networking/dns.md), and [DHCP](../technologies/networking/dhcp.md). They are responsible for the smooth flow of network traffic. They are also the people to ask when diagnosing networking connections on CSL systems.

### Cybersecurity & Monitoring

The Cybersecurity team is responsible for ensuring that proper cybersecurity procedures are being followed throughout the Lab, and quickly responding to any vulnerabilities detected. They are also responsible for communication with FCPS's Office of Cybersecurity (OCS). They are also responsible for [observability in the CSL](../technologies/monitoring/), including logging, alerts, and metrics. They are responsible for maintaining systems that provide monitoring capability such as [Grafana](../technologies/monitoring/grafana.md) and Prometheus.

### Documentation

The Documentation team is responsible for accurate, comprehensive, and well-written documentation for the Sysadmins. They assist and strongly encourage other teams in documenting everything in both our [Runbooks](documentation/#runbooks) and this [Docsite](documentation/).

## Teams

### Intranet

The Intranet team is responsible for the administration, maintenance, and development of [Ion](../services/ion/). They are also responsible for [printing operations in the Lab](../services/printing/), including the CUPS server and the printers.

### Director

The Director team is responsible for the administration, maintenance, and development of [Director](../services/director/). They also are responsible for ensuring the high availability of websites hosted on Director.

### Web Services

The Web Services (or WWW) team is responsible for maintaining the web presence of the Lab not supervised by other teams. This includes \*.tjhsst.edu domains not supervised by other teams or by the Infrastructure Lead, and software used by TJHSST classes like [Othello](../services/academic-services/othello/), the TJHSST AI Grader, and Tin. They are also responsible for managing TJ's proxy configuration file.

### Mail

The Mail team is responsible for maintaining TJ's mail servers, list servers, and webmail clients.

### Storage

The Storage team is responsible for the storage of data in the Lab including [Ceph](../technologies/storage/ceph/) and [OpenAFS](../obsolete/afs/openafs.md). They are also responsible for the CSL's data backups.

### Cluster

The Cluster team is responsible for maintaing [TJ's clusters](../services/cluster/) (Borg and HPC) and hardware used for advanced computing, including GPUs.

### Workstations

The Workstations team is responsible for TJ's workstations in rooms 200 and 202.

### Signage

The Signage team is responsible for maintaining TJ's [Signage displays](../services/signage/). They work closely with the Ion team in this regard.

### Printing

The Printing team is responsible for maintaining TJ's [Printers](../services/printing/). They use CUPS and maintain the availability of printers. They work closely with the Ion team in this regard.
