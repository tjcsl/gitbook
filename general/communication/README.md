# Communication

## Overview

Communication is important to the effective functioning of the Lab. We use Slack as our primary form of communication within the Sysadmins. Communication through e-mail is used to communicate with various stakeholders in the TJ community. IRC is used as a fallback form of communication in the event Slack is down. Sending mass notifications to the whole school is done through Ion announcements.

## Slack

Slack \([https://slack.com](https://slack.com)\) is a proprietary set of tools for team collaboration. It offers various integration with different services. We have a workspace at [https://tjcsl.slack.com](https://tjcsl.slack.com). The CSL's first widespread use of the Slack workspace began in August of 2018. Slack became the official form of communication under the direction of Omkar Kulkarni.

### Channels

Conversations in Slack are organized into public and private channels. You can join a public channel by clicking on the`+` icon. Private \(invite-only\) channels require an invite from someone within the channel. Use invite-only channels for sensitive or non-public discussions.

Some channels include:

* \#general: For general discussions
* \#random: For random discussions
* \#ion: For discussion of Ion
* \#director: For discussion of Director
* \#sysadmins \(private\): For internal Sysadmin-only discussions
* \#notifications \(private\): For 
* and other channels

### Applications

Workspace members can add applications to the channels to improve the workflow of the Sysadmins. Some examples of integrated applications include:

* Sentry: Notifies Sysadmins of errors/events on web applications

{% page-ref page="../../technologies/monitoring/sentry.md" %}

* Uptime Robot: Notifies Sysadmins of downtime on critical public-facing CSL maintained webpages

{% page-ref page="../../technologies/monitoring/uptime-robot.md" %}

### Best Practices

* Keep conversations in relevant channels.
  * Don't talk about account recovery in the Signage channel, etc.
* Don't spam.
  * Don't make people annoyed with notifications from a channel to the point where they mute the channel.
* Use proper etiquette and be respectful.
  * There is no need to be disrespectful in the Slack channels.
* Keep private conversations in DM.
  * Don't pollute channels with unnecessary messages that could just be communicated directly.
* Use emotes to react to messages.
  * This reduces overall clutter and makes it easier to find stuff.

### Roles

Workspace owners have full administrative access to the workspace. The owners can remove members and change workspace settings. The lead Sysadmin\(s\) and Faculty Sponsor should be owners and should designate other owners they find necessary. The Faculty Sponsor should be "Primary Owner".

Workspace members can join, write, and read in public channels.. They can also join, write, and read in invite-only channels. They can also create new public or invite-only channels.

The workspace membership should be restricted to people with an `@tjhsst.edu` e-mail.

## E-mail

Communicating with TJ community members is primarily done through email. The Eighth Period Office, administration, TJ Tech Team, teachers, and students rely on these channels for communicating with the Sysadmin team. Hence, e-mail services in the CSL \(mail.tjhsst.edu and lists.tjhsst.edu\) are **mission-critical services**.

### E-mail Responses

To keep everyone happy, responding to e-mails quickly is preferred.

The person who is responsible for responding to an e-mail usually varies on a case-by-case basis and based on various factors. Some of these factors include:

* Is it a support inquiry, complaint, question, or request?
* Who sent it and what position to they have?
* How important is it?
* Which Sysadmin\(s\) has/have access to the systems necessary to complete the request?
* How much knowledge does the Sysadmin have of the functional area?

In general, the lead or contact person for a specific functional area should respond to e-mails.

### Lists

#### Sysadmin Mailing List

Most communication directed to the Sysadmins gets sent to [sysadmins@tjhsst.edu](mailto:sysadmins@tjhsst.edu) . The official list address is `sysadmins@lists.tjhsst.edu`. This mailing list is restricted to Sysadmins and the Faculty Sponsor. Ask a mail admin for access if you did not get added to the list.

{% hint style="info" %}
If you did not get added to the mailing list, refer the mail admins to the new sysadmins onboarding issue on `sysadmins/infrastructure/access-requests`.
{% endhint %}

#### Intranet Mailing List

Most communication related to Ion gets sent to [intranet@tjhsst.edu](mailto:intranet@tjhsst.edu). The official list address is `intranet@lists.tjhsst.edu`. This mailing list is restricted to Intranet administrators.

#### Director Mailing List

Most communication related to Director gets sent to [director@lists.tjhsst.edu](mailto:director@lists.tjhsst.edu). This mailing list is restricted to Director administrators..

## IRC

Internet Relay Chat \(IRC\) is an application layer protocol that facilitates communication in the form of text. You can read more about the protocol [on Wikipedia](https://en.wikipedia.org/wiki/Irc). We use Freenode to facilitate communications. IRC had been the primary tool for communication among Sysadmins until the switch over to Slack.

### Clients

Multiple clients support IRC. We recommend one of the clients listed below:

* TheLounge: A modern self-hosted IRC web client.   We host an instance at [lounge.tjhsst.edu](https://lounge.tjhsst.edu) on Director.  Their repository can be found at [https://github.com/thelounge/thelounge](https://github.com/thelounge/thelounge).  Login is done through LDAP.
* Matrix through matrix-appservice-irc: Matrix is an open network for secure, decentralized communication.  [https://matrix.org](https://matrix.org), it's reference homeserver, hosts a Freenode IRC bridge.  One way to connect to Matrix is through their reference client, Riot \([https://about.riot.im/](https://about.riot.im/)\).  Instructions on how to connect can be found at [https://github.com/matrix-org/matrix-appservice-irc/wiki/End-user-FAQ](https://github.com/matrix-org/matrix-appservice-irc/wiki/End-user-FAQ).  The bridge's repository is located at [https://github.com/matrix-org/matrix-appservice-irc/](https://github.com/matrix-org/matrix-appservice-irc/).
* Hexchat: A local IRC client.  It is installed on most Linux distros.  Their website is [http://xchat.org/](http://xchat.org/).

### Channels

* \#tjcsl
  * Channel for Sysadmins
* \#tjhsst
  * Channel for TJHSST students, past and present. and  it is composed of alums and current students.

### Etiquette

Using common sense, following server rules, and being respectful to others is good practice.

### Useful Commands

Most of these are well documented by IRC.

| Command | Description |
| :--- | :--- |
| `/msg NickServ identify <username> <password>` | Authenticate to NickServ |
| `/msg NickServ register <password> <email>` | Begin registration of current nick |
| `/join <channel>` | Join a channel |

Sending messages is as easy as writing the message in the appropriate channel. You can reference others in the channel by mentioning their nick.

## Ion Announcements

Posting general announcements to the TJ community \(such as extended downtime or school-wide announcements\) should generally be done through an Ion announcement. To approve an announcement, you must be an Ion admin. You can add an announcement by clicking the big `Add` button at the top of the dashboard.

The announcement should be simple, concise, informative, and attributed to the right person or team.

