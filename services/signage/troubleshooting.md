# Troubleshooting

**There is no internet on** `cs-library`**. Help!**

`cs-library` has always had trouble with the Wi-fi in that area.  Hence, Ansible is not run on it and`unattended-upgrades` do not run on it.  It would be nice to have a network drop for a wired connection.

**Why can I not access Ion or it says "Access Restricted"?**

First, you should check that you have a network connection.  Second, you should make sure that the IPs of the Signage displays are within the `INTERNAL_IP` range defined in Ion's  production's `secret.py`.





