# Setting Up a Signage Stick
## Guide Description
In the past, Signage was a separate web application from Ion, viewed on Raspberry Pis.

Now, Signage is run on Intel Compute Sticks running Ubuntu Server with a minimal GUI to display an Ion subpage.

This guide will show you how to (mostly) set up a new stick.

## Manual Stuff

### Installing the OS
When you first get one of the Compute Sticks, it should come pre-loaded with Windows 10 by default. If this is not the case, you probably got the wrong version of compute stick (the Ubuntu ones aren't powerful enough).

#### Prepare the Installation USBs

These steps only have to be done if there are no installation USBs around.

I'm assuming (hopefully correctly) that you know how to manage USBs on whatever computer you are using. If not, just [https://bing.com Google] it.

1. Download the OS
  * Head on over to [https://www.ubuntu.com/download/server] and download .iso for the the latest LTS version (currently 16.04 LTS).
2. Put the OS on a USB
  * On Linux or Mac, use the `dd` utility to flash a USB with the OS installer. On Windows, use [https://sourceforge.net/projects/win32diskimager/:: Win32DiskImager] or something to that nature.
  * Example `dd` command: `sudo dd if=ubuntu_server-16.04.iso of=/dev/sdx progress=status`, replacing `sdx` with the actual USB device identifier.
3. Download the wpa_supplicant packages and dependencies
  * On Ubuntu system with the same arch as the compute stick (preferably an existing compute stick), use this command to download `wpa_supplicant` and all of its dependencies: ```apt-get download wpa_supplicant && apt-cache depends -i wpa_supplicant | awk '/Depends:/ {print $2}' | xargs apt-get download```
  * On a Windows or Mac system, you will have to do this manually by looking at [https://packages.ubuntu.com/] and downloads and tracing the dependencies manually.

#### Boot from the OS Install Disk

1. Plug in a keyboard into the Compute Stick
2. Plug the Stick into an available monitor
3. Turn the Stick on
4. As the Stick is booting, repeatedly press the F10 key until you see a boot menu.
  * If this does not work, reboot and try again
5. At the boot screen, select the USB as the boot device
  * If you can't boot, [Google](https://bing.com) what went wrong.
6. Follow the on-screen prompts to install Ubuntu Server LTS
  * Try to install as little extra features as possible
  * Set the username/password as user/user for easy installing later on.
    * We will change the password later on, don't worry

### Set up Networking

~~TODO: put the config files on the USB as well for easy installation~~
This should now be automated as well, consult whomever owns the magical USB drive (or make a new one yourself!)

#### Mount the USB

Now, plug in the other installation USB (the one with the packages on it) into the Compute Stick. Mount it with
```
sudo mount /dev/sdxn /mnt
```
where `sdxn` is the USB partition you stored the files on. `x` will usually be `a`, and `n` will usually be `1`.

#### Install the Networking Packages
`cd /mnt` to get into the same directory as the downloaded networking packages, then run
```
sudo dpkg -i *.deb
```
to install them all. This shouldn't take too long.

#### Configure Networking
1. Put this in the file `/etc/wpa_supplicant/wpa_supplicant.conf` (you will need to use `sudo`):
    ```
    ctrl_interface=/var/run/wpa_supplicant
    network={
        ssid="tjhsst"
        key_mgmt=NONE
        scan_ssid=1
    }
    p2p_disabled=1
    ```
2. Put this in the file `/etc/systemd/network/wlp1s0.network` (you should change all occurrences of `wlp1s0` to whatever the actual network adapter is, that can be found out using `ip a`):
    ```
    [Match]
    Name=wlp1s0

    [Network]
    DNS=198.38.16.40 198.38.16.41

    [Route]
    Gateway=198.38.31.254

    [Address]
    Address=198.38.31.xx/24
    ```
    You should also replace the `xx` with the actual IP you have decided to assign the Compute Stick. Check with whoever is in charge about this (if it's yourself, do whatever)

3. Run
    ```
    sudo systemctl edit wpa_supplicant
    ```
    and type
    ```
    [Service]
    Type=
    Type=simple
    ExecStart=
    ExecStart=/sbin/wpa_supplicant -i wlp1s0 -c /etc/wpa_supplicant/wpa_supplicant.conf
    ```
    in order to make the wpa_supplicant actually be configured by the config file you made earlier.

4. Run ```sudo systemctl enable systemd-networkd && sudo systemctl enable wpa_supplicant && sudo reboot``` to finish the network configuration.

    Try to `ping fcps.edu` 30sec after booting to make sure networking sure. If this doesn't work, check with someone who knows more than you.

## Automatic Stuff

### Change the Mirror
Run
```
sudo vim /etc/apt/sources.list
```
and type
```
:%s/us.archive.ubuntu.com/mirror.rit.edu/g
```
then save the file and exit.

### Download Packages
Run
```
sudo apt update && sudo apt upgrade && sudo apt install chromium-browser xinit i3
```
to update the system and install all the required packages.

### Make sure the time is right

1. Edit `/etc/systemd/timesyncd.conf`, edit it so it looks like this:
    ```
    [Time]
    NTP=ntp1.tjhsst.edu
    FallbackNTP=ntp.ubuntu.com
    ```
2. Then, run
    ```
    sudo systemctl enable systemd-timesyncd && sudo systemctl start systemd-timesyncd
    ```
    to enable NTP syncronization

### Make it Auto-start
To the end of `~/.config/i3/config`, add the line
```
exec chromium-browser --kiosk https://ion.tjhsst.edu/signage/display/{display_name}
```
replacing `{display_name}` with the actual signage display name. Check with an [[Ion]] or [[Signage]] developer for specific names.

TODO:
+ Add a section on autologin and autostart of x to make this above this actually happen
+ Also add a section about removing the undesirable features of i3
+ Also add a section about removing more undesirable features of chromium for security
