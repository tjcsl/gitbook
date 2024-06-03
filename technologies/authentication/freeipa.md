---
description: The CSL's authentication system
---

# FreeIPA

**FreeIPA** ([https://www.freeipa.org](https://www.freeipa.org)), which stands for "Free Identity, Policy, and Authentication", is an open-source identity management system that provides the CSL with centralized account (Name Service Switch, or NSS) and host management services. FreeIPA was implemented lab-wide on June 16, 2023, replacing the CSL's old LDAP/Kerberos setup dating back to 2007.

## History

Previously, the CSL used NIS to store network user information. However, when the decision was made to integrate CSL accounts and authentication with Windows Active Directory (previously all CSL accounts were managed separately and required an application form to receive), LDAP was chosen to replace NIS as the backend for the NSS database.

Integrated authentication using LDAP and [Kerberos](../../obsolete/kerberos.md) was initially deployed in lab 231 during the spring of 2006. Sun Directory Server 5.2 was used at the time, replicated from sol across what are now known as chuku and ekhi. During the summer following, LDAP was moved into a VMWare virtual machine known as daystar in order to run LDAP on a faster system. However, for reasons not completely understood, the VM subsequently developed problems during the fall of 2006 and resulted in NSS becoming painfully slow on both rockhopper (at that time used for all of lab 231 and 16 LTSP nodes in the CSL) and the rest of the CSL workstations. In order to remedy the situation, `/etc/passwd` was rapidly deployed as a flatfile across all affected systems. Hesiod was subsequently set up as the NSS database for the remainder of the school year and the beginning of the next.

During the winter of 2007-08, NSS was switched back to LDAP following various discussions. LDAP was configured on chuku and mihr, running Sun Directory Server 6.

Upon reception of the [Sun Academic Excellence Grant](../../machines/history/2008-sun-aeg.md), LDAP was moved into the LDOMs (UltraSPARC-specific virtual machines) ldap1 and ldap2 running on [Ohare](../../machines/other/sun-servers/ohare.md) and Logan. The service was eventually migrated to KVM virtual machines openldap1 and openldap2.

By the early 2020s, this setup was starting to show its age; account creation was a cumbersome, several-stage process that was prone to failure, LDAP was hard to integrate with systems like Single Sign-On (SSO), and there was a lot of cruft left over from fifteen years of the old setup. The use of FreeIPA as a replacement for the legacy system had been considered since 2020, but it wasn't until 2023 that the decision was made to switch to FreeIPA, largely because the lab was being shut down anyway for Ceph upgrades. Migration work started on May 31, 2023 with the installation of a FreeIPA server on Utonium. The install was moved to Sauron on June 16, a couple hours after the school year ended. (It was going to be moved shortly after school started, but had to be paused last-minute after an Ion lead remembered that people needed to see their bus locations.)

## Systems

The CSL has three FreeIPA servers. Sauron serves as `ipa1`, the primary server. `ipa2` and `ipa3` are VMs. In addition to FreeIPA, these three machines serve as lab NTP servers, and `ipa1` is also the lab's DHCP server. However, these functions (FreeIPA, NTP, DHCP) function largely independently of one another; the colocation is mostly for historical reasons and to save on resources.

## Features

The nice thing about FreeIPA is that it takes care of a lot of stuff on its own. It comes with a web UI to make changes, as well as an API and some handy Python libraries. We have built-in support for host-based access control instead of relying on SSH configs or the old system that was implemented in OpenLDAP. It makes the account management process a lot easier.

