# Setting Up the Operating System

If something goes wrong with the server, an operating system reinstall may be required. There are several recommendations regarding how the operating system \(Ubuntu, in this case\) is installed.

## Why Ubuntu?

Ubuntu may not be the flashiest distro out there \(indeed, many Linux elitists look down on it as a distro for new users\), but it's stable, reliable, and has great support, as it's both immensely popular and backed by a major corporation \(Canonical\). We'll be using the Long Term Support \(LTS\) branch of Ubuntu, as it gets extra support from Canonical - the latest version, 18.04 \(because it was released in April 2018\) is supported until April 2028. All CSL Linux servers have been running Ubuntu since a reinstall in 2017.

## Installing Ubuntu

Go to Ubuntu's website \(www.ubuntu.com\). Select "Download" from the top menu. Choose "Use the traditional installer" under Ubuntu Server.

{% hint style="info" %}
Do **NOT**:

* Select Ubuntu Desktop downloads. Those are for personal laptops and desktops, not servers.
* Select the regular Ubuntu Server installer if the server doesn't have a working Internet connection. The installer will not work without Internet. The traditional or alternative installer will work regardless of network connection, which is why it is recommended.
{% endhint %}

Select the link for the LTS installer, and choose the AMD64 option from the next screen. Once the ISO file has downloaded, use an tool like ImageUSB, Unetbootin, Rufus, or Etcher to write the installer to a flash drive. \(If you're a cool kid, you can use `dd` for this, but be careful what disks you name in the command.\)

After the installer has been written to the flash drive, plug it into one of Fiordland's USB ports and restart the server. It will take a long time to start up, but it should eventually boot into the Ubuntu installer.

Follow the directions in the installer. It'll try to configure the network, but if the network doesn't work, it'll give you the option to proceed without setting up the network. When you get to partitioning, select the disk drives \(which should be `/dev/sda1,` but it might not\) and mount it as `/`. If you decide to format it, make sure you're formatting it as the ext4 journaling file system.

After the installer has completed, press `Ctrl+Alt+F3` and type in `sudo mount /dev/sda1 /mnt` to mount the installed system \(if you didn't install Ubuntu to `dev/sda1`, then replace it with the partition that you did install it to\). Type in `chroot /mnt` to access the server, followed by `nano /etc/default/grub` to edit the Grand Unified Bootloader \(GRUB\) configuration file. There should be a line that says `#GRUB_TERMINAL=console` in the file. Remove the `#` from the beginning, press `Ctrl+X` and `Y` to save the file, and run `grub-update`. \(If that doesn't work, then `grub-mkconfig -o /boot/grub/grub.cfg` will do the same thing.\) This disables GRUB's graphical start, which doesn't work on Fiordland. Then you can unplug the USB drive from the server and reboot.

