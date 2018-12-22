# NTP

**NTP \(Network Time Protocol\)** is a protocol to synchronize the clocks between various computers \(read more at its [WIkipedia page](https://en.wikipedia.org/wiki/Network_Time_Protocol)\). In the CSL we currently run one NTP server: agni.

## Configuration

To sync to the CSL's NTP server, use the following configuration file in `/etc/systemd/timesyncd.conf` \(for Ubuntu\)

```text
[Time]
NTP=agni.csl.tjhsst.edu
```

To apply the changes, run `systemctl restart systemd-timesyncd`.  You  can see the current time by running `timedatectl status`.  All machines in the CSL should be synchronized to the NTP server.

## Troubleshooting

**`systemctl status sytemd-timesyncd` is complaining about being unable to reach the NTP server.  What should I do?**

Make sure that you can reach the server's NTP ports.  Running `nc agni.csl.tjhsst.edu 13` should return the current time.  If it does not connect, something is wrong with the machine's connection to agni.

