---
description: Setup a Linux device on the FCPS network using Cloudpath
---

# FCPS Linux Wi-Fi Setup

{% hint style="warning" %}
All content in this article is shared for informational and educational purposes only. Use of any code or procedures described is done at your own risk. TJCSL assumes no liability for any errors or adverse effects that may result from the use of the information provided.
{% endhint %}

Firstly, you'll want to connect to the `FCPSonboarding` network using your network manager of choice. Then, depending on your specific setup, you may need to go to `http://www.neverssl.com` or [this link](https://wifienrollment.fcps.edu/enroll/FairfaxCountyPublicSchools/Production/process) to see the captive portal below. If FCPSonboarding isn't working for you, click the second link above on a known-working network (possibly your hotspot, during lunch).

<figure><img src="../.gitbook/assets/Screenshot 2026-01-05 at 2.57.39 PM.png" alt=""><figcaption></figcaption></figure>

Scroll down, accept the terms, and click "Start."

Next, click "Staff and Students," and enter your FCPS Student ID password (i.e. your Schoology credentials).

![Select "Staff and Students"](<../.gitbook/assets/Screenshot 2026-01-05 at 3.00.46 PM.png>)

![Enter your FCPS Student ID and password and then login. DO NOT ENTER THE CREDENTIALS ABOVE!](<../.gitbook/assets/Screenshot 2026-01-05 at 2.58.30 PM.png>)

You should see something like this. Select "Show all operating systems" at the bottom of the page (you might have to scroll down!).

![If you're using Linux, DON'T DOWNLOAD Cloudpath! Select "Show all operating systems."](<../.gitbook/assets/Screenshot 2026-01-05 at 3.01.15 PM.png>)

Scroll down (again) and select "Other Operating Systems."

![](<../.gitbook/assets/Screenshot 2026-01-05 at 3.01.23 PM.png>)

Download both the CA certificate and the user certificate to your machine (step 1 and 2, respectively). From here, the steps will depend on your specific Linux setup (mainly your network manager of choice), but there are instructions for the most common setups below:

#### wpa\_supplicant

Assuming you've downloaded the proper certificate files from both steps, run the following commands from the same folder that your certificates were downloaded in. You might be prompted to enter your FCPS password during the process. **YOU MUST REPLACE THE TEMPLATE VARIABLES IN <> WITH THE CORRECT VALUES.**

```
# disable bash history temporarily because of sensitive info
set +o history

# ensure initial wpa_supplicant directory
sudo mkdir -p /etc/wpa_supplicant/wifi_certs

# openssl must be installed beforehand! it's probably already installed.
# converts the p12 to a key and crt for wpa_supplicant compatibility
openssl pkcs12 -in <FCPS_ID>@student_byodfcpsedu.p12 -nocerts -out user.key
openssl pkcs12 -in <FCPS_ID>@student_byodfcpsedu.p12 -clcerts -nokeys -out user.crt

# move the certificates and set proper permissions (important!)
# change the wildcard if there are multiple
mv CA-*.cer FCPS.cer # for readability
sudo mv user.key user.crt FCPS.cer /etc/wpa_supplicant/wifi_certs/
sudo chmod 600 /etc/wpa_supplicant/wifi_certs/user.key
sudo chmod 644 /etc/wpa_supplicant/wifi_certs/user.crt /etc/wpa_supplicant/wifi_certs/FCPS.cer

# the following solution is semi-automated, but feel free to replace this with wpa_cli
# basically it just adds the FCPSbyod config to wpa_supplicant
sudo cp /etc/wpa_supplicant/wpa_supplicant.conf /etc/wpa_supplicant/wpa_sup.bak

sudo tee -a /etc/wpa_supplicant/wpa_supplicant.conf > /dev/null <<EOF

network={
    ssid="FCPSbyod"
    scan_ssid=1
    key_mgmt=WPA-EAP
    eap=TLS
    identity="<FCPS_ID>@student_byod.fcps.edu" # this differs for guest connections
    ca_cert="/etc/wpa_supplicant/wifi_certs/FCPS.cer"
    client_cert="/etc/wpa_supplicant/wifi_certs/user.crt"
    private_key="/etc/wpa_supplicant/wifi_certs/user.key"
    private_key_passwd="<FCPS_PASSWORD>"
}
EOF

sudo chmod 600 /etc/wpa_supplicant/wpa_supplicant.conf # sensitive password!

# finish up by restarting wpa_supplicant
sudo systemctl restart wpa_supplicant

# renew dhcp ONLY if using dhclient. differs for dhcpd and other dhcp managers.
sudo dhclient -r
sudo dhclient

# reenable history
set -o history
```

Reference [this](https://wiki.archlinux.org/index.php/WPA_supplicant) for further information.

#### NetworkManager (preferred)

Assuming you've downloaded the proper certificate files from both steps, run the following commands from the same folder that your certificates were downloaded in. You might be prompted to enter your FCPS password during the process. **YOU MUST REPLACE THE TEMPLATE VARIABLES IN <> WITH THE CORRECT VALUES.**

```
# disable bash history temporarily because of sensitive info
set +o history

# ensure initial ssl cert directories
sudo mkdir -p /etc/ssl/certs/
sudo mkdir -p /etc/ssl/private/

# openssl must be installed beforehand! it's probably already installed.
# converts the p12 to a key and crt for networkmanager compatibility
# enter your fcps password if asked
openssl pkcs12 -in <FCPS_ID>@student_byodfcpsedu.p12 -nocerts -out user.key
openssl pkcs12 -in <FCPS_ID>@student_byodfcpsedu.p12 -clcerts -nokeys -out user.crt

# move the certificates and set proper permissions (important!)
# change the wildcard if there are multiple
mv CA-*.cer FCPS.cer # for file identification and readability

sudo mv user.key user.crt /etc/ssl/private/
sudo mv FCPS.cer /etc/ssl/certs/
sudo chmod 600 /etc/ssl/private/user.key
sudo chmod 644 /etc/ssl/private/user.crt /etc/ssl/certs/FCPS.cer

# the following solution is semi-automated, but feel free to replace this with a
# different networkmanager solution (possibly a tui)

# add our FCPSbyod interface to nmcli (NetworkManager)
# note: replace wlan0 with your actual wireless interface name (check with 'ip link')
sudo nmcli connection add \
  type wifi \
  con-name "FCPSbyod" \
  ifname wlan0 \
  ssid "FCPSbyod" \
  wifi-sec.key-mgmt wpa-eap \
  802-1x.eap tls \
  802-1x.identity "<FCPS_ID>@student_byod.fcps.edu" \
  802-1x.ca-cert "/etc/ssl/certs/FCPS.cer" \
  802-1x.client-cert "/etc/ssl/private/user.crt" \
  802-1x.private-key "/etc/ssl/private/user.key" \
  802-1x.private-key-password "<FCPS_PASSWORD>" \
  802-1x.domain-suffix-match "nescloudpath-pv01.fcps.edu"

# connect to the new interface, which should handle dhcp
nmcli connection up FCPSbyod

# reenable history
set -o history
```

Note: As of somewhere around `NetworkManager` version 1.36.x (which ships by default in Ubuntu 22.04 LTS, Fedora 36, and probably some other major Linux distributions), `NetworkManager` will fail to connect if `Domain` is not properly set to `nescloudpath-pv01.fcps.edu` and will give **NO** helpful information about why.

#### GNOME Desktop Environment

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption><p>This image is just for UI reference. DO NOT ENTER VALUES FROM THIS IMAGE!</p></figcaption></figure>

Some desktop environments, GNOME below, allow you to configure this through built-in utilities. You can configure GNOME like this, with Identity being `<FCPS ID>@student_byod.fcps.edu`, the User Certificate and Private Key being the User CA you converted (see commands above), the CA certificate being the CA certificate you downloaded, and the Private key password your FCPS password.
