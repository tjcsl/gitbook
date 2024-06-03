# NTP

**NTP (Network Time Protocol)** is a protocol to synchronize the clocks between various computers (read more at its [WIkipedia page](https://en.wikipedia.org/wiki/Network\_Time\_Protocol)). In the CSL we currently run two NTP servers, which coincidentally are also the FreeIPA servers.

## Configuration

This should be automatically handled when you join a machine to FreeIPA. You can copy an NTP configuration file from any NTP client to `/etc/chrony/chrony.conf` or `/etc/chrony.conf` (depending on the OS) and run `systemctl restart chronyd`.

## Troubleshooting

**`systemctl status chronyd` is complaining about being unable to reach the NTP server.  What should I do?**

Make sure that you can reach the server's NTP ports.  Running `nc ntp1.tjhsst.edu 123` should return the current time.  If it does not connect, something is wrong with the machine's connection to the NTP servers.
