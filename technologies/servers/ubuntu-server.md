# Ubuntu Server

Ubuntu Server is a Linux distribution developed by Canonical Inc., noted for its stability and ease of use. You can find out more on [the Ubuntu Wiki](https://wiki.ubuntu.com/BionicBeaver/ReleaseNotes).

Almost all of the CSL runs on Ubuntu Server, except for [Workstations](../../services/workstations/) \(which run Ubuntu Desktop\) and [The Cluster](../../services/cluster/) \(which runs [CentOS](centos.md)\).

## Notable Features

* Easy-to-use installer ISO \(download link [here](https://www.ubuntu.com/download/server)
* Extremely large, if not _the_ largest, package ecosystem
* Has included some unpopular software, including \(but not limited to\):
  1. The Unity desktop environment
  2. Integrated advertising
  3. netplan

## Notable CSL modifications

* Instead of installing from a usb, we prefer to use [Netboot](../networking/netboot.md).

## Trivia

* Ubuntu gets a significant portion of its packages from Debian, and shares the same package manager \(`apt`\).
* One of Ubuntu's official help channels is actually found on [StackExchange](https://askubuntu.com/).
* Ubuntu major releases are referred to by alliterating `<adjective> <animal>` codenames. As of this writing \(Jan 2019\), the current stable release \(18.04.1\) is codenamed **Bionic Beaver**.
* The default version of Python is 2 on all Ubuntu releases.
  * Plus, the minor version of Python 3 available is always 1 behind the latest.
* `nano`, arguably the superior console text editor, is installed by default \(as it should be\).
* Netplan can act as a configuration proxy for NetworkManager or systemd-networkd, in case you ever wanted to double the amount of cancer you handle.
* 16.04.5 LTS \(**Xenial Xerus**\) will always be the best release in my heart &lt;3

