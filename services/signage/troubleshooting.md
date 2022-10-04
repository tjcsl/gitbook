# Troubleshooting

**There is no internet on** `cs-library`**. Help!**

`cs-library` has always had trouble with the Wi-fi in that area.  Hence, Ansible is not run on it and`unattended-upgrades` do not run on it.  It would be nice to have a network drop for a wired connection.

**Why can I not access Ion or it says "Access Restricted"?**

First, you should check that you have a network connection.  Second, you should make sure that the IPs of the Signage displays are within the `INTERNAL_IP` range defined in Ion's  production's `secret.py`.

#### A signage is not on, but its software is functional. What do I do??

First off, check the hardware of the signage. Some connections might be broken, or something might be unplugged (for some reason). If everything seems clear, it might be that the input of the monitor is wrong, and it needs to be changed. Just find the buttons on the monitor and change the input to HDMI.
