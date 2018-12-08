# iLO

HP iLO, or integrated Lights Out, is a feature on HP servers that allows users to access the machine over the network even if the OS on the server cannot be reached.

## Accessing

A server's iLO can be reached in two ways:

* Via `ssh`, through `ssh server-ilo-hostname`
* Via a web interface by visiting `https:\\server-ilo-hostname.csl.tjhsst.edu` while using a VPN

## Updating Firmware

The version of iLO firmware that the servers ship with does not support access to the OS. The firmware can be updated with through the following steps:

* [Retreive a copy of the most up-to-date-firmware](https://google.com/search?q=hp+ilo+5+firmware+update), download a `.exe` version for Windows
* Unzip the executable with `unzip` or a similar command
* Retrieve the `.bin` file from the unzipped executable
* Access the iLO server you wish to update via the web interface
* Go to `Firmware and OS Software` &gt; `Update Firmware`
* Select `Local file` and the `.bin` file you extracted earlier
* Select `Flash`, this will update the firmware

Flashing the iLO firmware _will_ reboot the iLO, but will **NOT** reboot the server.

## Password Limitations

HP has these really weird password limitations on iLOs.  For HP iLO 5 (as of firmware version 1.35), it accepts any length password for a reset but only stores the first 39 characters (so you have to use the first 39 to login0.  (*Really annoying*). For HP ilO 2, they just have a 20 character limitation.
