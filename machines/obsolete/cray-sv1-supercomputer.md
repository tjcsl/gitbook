# Cray SV1 Supercomputer

{% hint style="warning" %}
This page contains information on systems that are in happy retirement from the Computer Systems Lab.
{% endhint %}

The **Cray SV1 Supercomputer** \(**Cray** or **Seymour** \([https://en.wikipedia.org/wiki/Seymour\_Cray](https://en.wikipedia.org/wiki/Seymour_Cray)\)\) used to be our supercomputer. Now, its shell can be found near the AudLob by the bus signage.

## History

The CSL used to have an ETA-10P supercomputer that was won through the SuperQuest competition. However, the supercomputer was damaged due to a roof leak. The only remains of the original supercomputer are the original floorplans, which are taped inside of the machine room electrical panel, and the honeywell rack, which is believed to have held hard drives. The trophy from the SuperQuest competition can still be seen in the windows of the machine room from the hallway in front of the systems lab.

In 2002, Cray donated the current SV1 supercomputer, seymour \(or simply "the cray"\). Although largely outdated by modern computers, the Cray is still an integral part of the lab and the machine room.

## Overview

### Mainframe Cabinet

This is the main cabinet of the Cray. The front faces the hallway windows, while the back faces the lab. The I/O and CPU reset switches are located at the top of the rear of the cabinet \(see boot-up\). The front has one of the main power switches.

### I/O Cabinet

This is the smaller cabinet off to the side of the mainframe cabinet. At the rear is the small text display and running lights. The front houses all of the I/O connections for the various modules, as well as network and serial connections. There is actually a small network hub at the base of the front of the cabinet. In addition, the power supplies for the case lights sit in an empty part of this cabinet.

### System Workstations \(SWS\)

The system workstation, or SWS, is the Sun SPARCstation 5 known as scadmin that sits adjacent to the main cabinets of the cray. In addition to providing direct serial access to the cray, the workstation has a direct ethernet connection that must be functioning in order for the cray to boot. The ethernet connection to the Cray should be connected to network port 0 on the back of the SWS. The main network connection to CSL network should be connected to network port 1. The two serial lines, A and B, should be connected to their respective ports, Serial A and Serial B.

### sn3313-mpn0

This is the white box at the top of the I/O cabinet. It runs VxWorks and is integral to booting the Cray.

### sn3313-fnc{0,1}

These are two blades in the back of the I/O cabinet. During bootup or reboot, these are booted along with sn3313-mpn0.

## Operation

During normal operation, the lights in the I/O cabinet should be running back and forth, the display on the white box should say "Running" and the cabinet should be making a whoosh sound.

### Boot-up Sequence

* Switch on Mains Disconnect at the bottom of the back of the I/O cabinet
* Flip breaker \(to ON\) at the bottom of the front of the mainframe cabinet
* Press the blue "Reset I/O" button at the top of the back of the mainframe cabinet.
* Press "Reset CPU" button, also blue, next to the "Reset I/O" button.
* On SWS, run

  ```text
  bootsys -c -H
  ```

* If running commands remotely, omit the -c option. It opens a console in an xterm. Instead, after finishing the next instruction, run

  ```text
  mfcon
  ```

  to start the console for the Cray.

* Still on the SWS, run

  ```text
  dring -initnode
  ```

* On the Cray, run

  ```text
  /etc/init 2
  ```

  to start multiuser mode.

  * The Cray will ask if the system date is correct. It usually is not, so enter a date in the MMDDhhmm\[\[CC\]YY\] format.
  * If the Cray complains about not mounting a filesystem because of filesystem errors, go back to runlevel 1 and fsck the filesystems in question.

#### SWS PATH

The Cray executables are not in the path by default. Add "/opt/CYRIops/bin" to the PATH. If using csh, execute

```text
setterm PATH "/opt/CYRIops/bin:$PATH"
```

### Shut-down Sequence

Don't shut it down. The only exception being that whenever a power outage happens, it helps to make sure the Mains Disconnect and the mainframe breaker are on the off position, until the Cray can be safely turned back on after power is restored.

To drop to single user mode from the SWS, run

```text
levelsys -r shutdown
```

on the SWS. UNICOS will still be running, but without networking and the other goodies of multi-user mode. Single-user mode can also be entered by running init on the Cray itself.

If the system really needs to be shutdown, the instructions are printed inside the mainframe cabinet around the top frame. The power cannot be turned off remotely.

### Setting the System Date and Time

When booting, the system will ask "is the date dow MM dd hh:mm:ss tz yyy correct? \(y or n\)." If you wish to change the date, enter the correct date in the "MMddhhmm\[yy\[ss\]\]" format. MM is the month, dd is the day of the month, hh is the hour in 24-hour time, mm is the minute, yy is the last 2 digits of the year, and ss is the second. The date can also be changed while the system is running with the date command.

### Setting the Time Zone

To change the time zone, edit the tz line in /etc/inittab. For Eastern time, the string "tz::timezone:TZ=EST5EDT" should be used. /usr/src/lib/libc/gen/ctime.c determines daylight savings time rules. This file might need to be changed with the \(relatively\) recent daylight savings time changes. Alternatively, the daylight savings timeperiods can be specified in the tz variable. Append the string ",M3.5.0,M9.5.0" to the tz string, where the first number is the month, the second number is the week, and the last number is the day \(0 = Sunday\) of the week that daylight savings time is active. The above example is from the last Sunday in March through the last Sunday in September.

### Adding/Removing User Accounts

The Cray uses a system called the UDB, for user database, which replaces the standard /etc/passwd and /etc/group files. Note, however, that the /etc/passwd and /etc/group files are automatically updated by the UDB. Note that the UDB database, /etc/passwd, and /etc/group files are not immediately updated in all situations. To see the information in the kernel tables, use the udbpl command.

For easy user management use the "nu" command. It makes setting all of the variables for the UDB easy. The X utility xadmin is also available.

Alternatively, use the UDB commands directly. A new user requires an entry in the UDB and a home directory. The udbgen command creates and maintains the UDB. The udbsee command converts the database to an ASCII file that is used as input for udbgen. To remove a user, use the udbgen delete command to remove the specified entry from the UDB. To simply disable the account, run "udbgen -c 'update:john:permbits+:system-restricted:'" as root, where john is the username.

### Networking

Look in /etc/config to configure networking. The ifconfig command can also be used for non-permanent networking settings. The name of the main network interface is gether0. The loopback interface is lo0.

## Local Cray Network

sn3313-mpn0, sn3313-fcn{0,1}, and the SWS are all connected via a local network through the hub in the I/O cabinet. When the power is first turned on, sn3313-mpn0 will bring up its network interface. sn3313-fcn{0,1} will also start, bringing up their network interfaces. When the bootsys command is run, the SWS connects over the network to these three devices, reboots them, and initializes the system. If the network is not connected properly, or one of the devices fails to start, the Cray will fail to boot.

### Network addresses

* 10.1.124.200 sws3313-private \(qe0 interface on the SWS\)
* 10.1.124.201 sn3313-mpn0
* 10.1.124.203 sn3313-fcn1
* 10.1.124.202 sn3313-fcn0

## Heat

During the summer months, the air conditioning cannot cool the Cray, sun machines, cluster, and other servers at the same time. The average temperature will rise to around 85 degrees fahrenheit. The Cray refuses to perform labor in such conditions and will quickly crash. If it is rebooted, it will fail to boot. The Cray is much happier in the cooler temperatures of winter.

## External Links

[Cray, Inc.\'s official website](http://www.cray.com/)

