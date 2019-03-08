# Setting up RAID

This is a "tutorial" for setting up RAID arrays on the sun servers.

Oracle *has* [documentation](https://docs.oracle.com/cd/E19902-01/html/835-0796/wm2wosig.gjxrc.html) on this process, but apparently not for the version of the BIOS that our machines run.

## Before you begin

Put the drives you want to create the array out of in the server, then reboot to access the BIOS RAID menu.

## Accessing the RAID menu

While the machine is booting, spamming some combination of `control + i`, `control + c` and `control + s` might open the RAID menu.  Or it might not.  Keep trying different things until itworks.

{% hint style="info" %}
RAID must be enabled in the BIOS menu before it can be configured in the RAID menu.  If you cannot access the RAID menu, check the BIOS.  Or press different keys.  That could also be the problem.
{% endhint %}

{% hint style="info" %}
At this point, it should be noted that the BIOS is from 2007 or 2008 and has not been updated since.  It runs at about the speed you would expect from a motherboard this old.
{% endhint %}

## Configuring RAID

Each machine has three possible RAID arrays; disks compatible with each other seem to be placed into an array automatically.  Inactive arrays can normally be activated via the menu.

Each array appears to be RAID 1, as the total storage is only as large as a single disk, but the menu does not specify.


