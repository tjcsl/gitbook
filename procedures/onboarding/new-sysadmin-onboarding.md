# New Sysadmin Onboarding

## Notes

### Operating Systems

We do not support Microsoft Windows for Sysadmin work.  The Lab functions completely on \*nix-based operating systems \(with a bit of \*BSD\), and, hence, many assumptions are made about the systems from which Sysadmins administer the Lab, severely limiting the usefulness of Microsoft Windows.  For example, we write scripts to target \*nix-based operating systems with `bash` support which Windows is not.  Even though new versions of Windows supports [Windows Subsystem for Linux](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux),  this compatibility layer does not provide a full Linux system.  Hence, various programs developed for Linux such as `sshuttle`, `iptables`, and many others do not work on WSL or have serious problems. Although you could maybe get by using Windows for simple tasks, any serious administrative tasks require a Linux box.

The recommended operating systems for Sysadmin work are:

* Arch Linux \([https://www.archlinux.org/](https://www.archlinux.org/)\)
* Ubuntu \([https://www.ubuntu.com/](https://www.ubuntu.com/)\)

Other mainstream Linux distros are also acceptable.

Although it is possible to conduct work as a Sysadmin from macOS, we don't explicitly support it as the Lab runs on Linux.  Programs on macOS are often very old and/or ports of Linux programs, all of which may result in problems, incompatibilities, or inconsistencies for Sysadmins using macOS.

