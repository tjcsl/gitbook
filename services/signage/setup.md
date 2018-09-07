# Setup

## Network Configuration Overview

Each Signage Intel Compute Stick connects to TJ's Windows network. Each stick's MAC address is whitelisted by TJ's Tech Team to access the `tjhsst` network. Using [wpa\_supplicant](https://wiki.archlinux.org/index.php/WPA_supplicant) and [systemd-networkd](https://wiki.archlinux.org/index.php/Systemd-networkd), each stick is assigned an IP on the Windows network.

## Installation

In the past, Signage was a separate web application from Ion, viewed on Raspberry Pis.

Now, Signage is run on Intel Compute Sticks running Ubuntu Server with a minimal GUI.

This guide will show you how to set up a new stick

### Manual Stuff

#### Installing the OS

When you first get one of the [Compute Sticks](https://livedoc.tjhsst.edu/wiki/Compute_Sticks), it should come pre-loaded with Windows 10 by default. If this is not the case, you probably got the wrong version of compute stick \(the Ubuntu ones aren't powerful enough\).

**Prepare the Installation USBs**

These steps only have to be done if there are no installation USBs around.

I'm assuming \(hopefully correctly\) that you know how to manage USBs on whatever computer you are using. If not, just [Google](https://bing.com) it.

1. Download the OS
   * Head on over to Ubuntu's [website](https://ubuntu.com/download/server) and download .iso for the the latest LTS version \(at the time of this writing 16.04 LTS\).
2. Put the OS on a USB
   * On Linux or Mac, use the `dd` utility to flash a USB with the OS installer. On Windows, use [Win32DiskImager](https://sourceforge.net/projects/win32diskimager/::) or something to that nature.
   * Example `dd` command: `sudo dd if=ubuntu_server-16.04.iso of=/dev/sdx progress=status`, replacing `sdx` with the actual USB device identifier.
3. Download the wpa\_supplicant packages and dependencies
   * On an Ubuntu system with the same architecture as the Compute Stick \(preferably an existing Compute Stick\), use this command to download `wpa_supplicant` and all of its dependencies:

     ```text
     apt-get download wpa_supplicant && apt-cache depends -i wpa_supplicant | awk '/Depends:/ {print $2}' | xargs apt-get download
     ```

   * On a Windows or Mac system, you will have to do this manually by tracing dependencies.

**Get Configuration Files**

There is a GitLab [repo](https://gitlab.tjhsst.edu/signage3/cs-config) which contains important scripts and config files to setup networking and an Ansible-ready system.

You should clone it by running within the mounted wpa\_supplicant drive

```text
git clone git@gitlab.tjhsst.edu:signage3/cs-config.git
```

**Boot from the OS Install Disk**

1. Plug in a keyboard into the Compute Stick
2. Plug the Stick into an available monitor
3. Turn the Stick on
4. As the Stick is booting, repeatedly press the F10 key until you see a boot menu.
   * If this does not work, reboot and try again
5. At the boot screen, select the USB as the boot device
   * If you can't boot, [Google](https://bing.com) what went wrong.
6. Follow the on-screen prompts to install Ubuntu Server LTS
   * Try to install as few extra features as possible
   * Set the username/password as user/user for easy installing later on.
     * We will change the password later on, don't worry

**Mount the USB**

Now, plug in the other installation USB \(the one with the packages on it\) into the Compute Stick. Mount it with

```text
sudo mount /dev/sdxn /mnt
```

where `sdxn` is the USB partition you stored the files on. `x` will usually be `a`, and `n` will usually be `1`.

### Automatic Configuration

Scripts to complete initial setup of the script can be found on [GitLab](https://gitlab.tjhsst.edu/signage3/cs-config). You will need to copy this repository to a flash drive.

* Cd into `/mnt/cs-config`
* Run `./setup_network.sh <LAST TWO DIGITS OF IP>` to configure  networking.
* Reboot.
* Check that you have internet \(`ping`\) and use `ip a` to check that you have been assigned an IP address.
* Run `./setup_finish.sh` to complete setup \(install python basic config\)
* From your device, run the CSL Ansible play for signage \(`signage.yml`\) to configure the base of the system.

