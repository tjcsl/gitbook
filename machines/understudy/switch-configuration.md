# Switch Configuration

## Fido

Fido is the Gigabit Ethernet switch connected to [Fiordland](server-configuration/#fiordland).

### Technical Specifications

| Field | Value |
| :--- | :--- |
| Switch Model | Cisco Catalyst 4948-10GE |
| FastEthernet Ports | 48 |
| Gigabit Ethernet Ports | 2 |

## Access the Switch

Plug an Ethernet cable into the port on the front of the switch labeled "SERIAL". Connect the other end of the cable into a USB-to-Ethernet adapter and plug that into Fiordland. Then, from the server, type `sudo screen /dev/ttyUSB0` to access Fido over serial. If the screen goes fully black, you might need to hit ENTER a couple times so that the prompt shows up.

If the prompt says `fido#`, you're good to go. If it says `fido>`, you'll need to type `enable`. The password for the switch is on passcard.

##  Set Up the VLAN

First, let's set up the Understudy VLAN.

1. Type in `config t` to enter config mode.
2. Enter `vlan 2100` to initialize a VLAN with the ID 2100.
3. Next, `name vlan2100` gives it the name vlan2100.
4. End the setup by typing `end`.

## Assign a Port to VLAN

Next, we need to attach the Ethernet port \(port 4, in this case\) to the VLAN we just created.

1. Type in `config t` to enter config mode.
2. Enter `int fa0/4` to configure port 4.
3. Running `switchport mode access` designates the port as an access port, making it link to only one VLAN.
4. Now, type in `switchport access vlan 2100` to link it to VLAN 2100.
5. End the setup by typing `end`.

## Set VLAN IP Address

Now, we should give VLAN 2100 an IP. This will be the same as the IP address for the server.

1. Type in `config t` to enter config mode.
2. Enter `int vlan 2100` to configure VLAN 2100.
3. The command to set the IP address is `ip address 198.38.21.113`.
4. Now let's the set the gateway with `ip default-gateway 198.38.21.126`.
5. End the setup by typing `end`.

## Set Up the Internet Port

Finally, we need to configure the port connected to core0.

1. Type in `config t` to enter config mode.
2. Enter `int gigabitethernet0/1` to configure Gigabit Ethernet port 1.
3. Running `switchport mode access` designates the port as an access port, making it link to only one VLAN.
4. Now, type in `switchport access vlan 2100` to link it to VLAN 2100.
5. End the setup by typing `end`.

## If You're Done...

Assuming nothing is wrong with [Core0](../switches/core0.md) or [Xnor](../switches/xnor.md), the Internet should work now. If so, type `copy running-config startup-config` to make the changes persistent. 

