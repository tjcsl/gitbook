# Networking

## Networking

## The Layer System

There is lots and lots of stuff here, but it's covered more succinctly and completely on [Wikipedia](http://en.wikipedia.org/wiki/OSI_model). However this will be left here since it is generally more applicable to the Systems Lab.

It is easiest to look at the network as a layer system, where each layer sits on top of the previous layer and uses its functions. This can also be compared to the level system presented in the computer architecture course.

### Layer 1

The first network layer is the "hardware," which in this case is the cable. There are a few different types of cable that can handle network connections, but below are the more commmon ones. \(BTW, you can get a lot of this information from Phrack.\)

#### Category 3 UTP

This is the old standard for cable. In actuality, there is nearly no such cabling in the school. Cat 3 can handle up to a 10Mbps link, which is the reason it is so unattractive.

#### Category 5 UTP

Currently this is the worldwide standard for LAN wiring. Cat 5 cable can handle up to 100Mbps if all eight wires are connected correctly. There is also a Cat 5e standard that exists, which supposedly handles up to 1Gbps \(350MHz\). When making such cables, one has to be careful for the cables to meet the Cat 5 restrictions. They have to be between 3 feet and 100 meters. Also, the specification only allows for the untwisting of 1/2 inch of cable on each end.

#### Fiber

Fiber optic cable can carry data for longer distances than Cat 5. The standard fiber cable can be as long as 2 kilometers. Fiber can also handle Gigabyte connections quite well. Unfortunately fiber optic cable is very expensive, and rarely used in LAN settings.

### Layer 2

Layer 2 is the transport layer. Basically, it defines the way the data moves through the Layer 1 network \(i.e. the cable\). There are a number of protocols to do that.

#### Ethernet

Ethernet is one of the most commonly used layers. According to the Ethernet standard, RFC826, each NIC \(Network Interface Card\) must have a unique MAC address. A MAC address is a unique 48-bit number given to every piece of Ethernet networking equipment. The first 24 bits of it were given out to manufacturers. The last 24 bits are up to each manufacturer's discretion. Although it is possible to change the MAC address on a board, I wouldn't recommend it, as there is no need for it, and it can cause major havoc. Each Ethernet "packet" is called a frame. Each Ethernet frame contains the source MAC address, the destination MAC address, and the payload \(also called MTU\), which can be up to 1500 bytes. \(If you are using copper gigabit Ethernet, some switches may let you to set the MTU to 9000, but that is outside the standard.\) The higher layers are carried in these 1500 bytes.

#### ATM

ATM allows for up to 155Mbps transfer speeds, but has a smaller MTU than Ethernet. On the other hand, ATM provides some of the features that higher levels provide, such as guaranteed data delivery, and thus is an overall gain for applications such as video streaming. It is, however, little used.

### Layer 3

Layer 3 is generally referred to as the IP layer. This is the layer that allows computers to communicate with one another using a certain address scheme that is not interface dependent \(i.e. MAC address\).

#### IPv4

IPv4 is the current overall standard used on the Internet. Addresses are usually shown with dotted quads \(like 198.38.16.9\). This addressing scheme thus allows 255^4 different addresses. This IP space is split into three different classes of IP blocks, which are designated A, B, and C. Class A is the 255.0.0.0 netmask, meaning that only the first of the quads is specified. For example, MIT has the 18 class A. Class B has a 255.255.0.0 netmask, meaning that the first two quads are specified. Fairfax County has the 151.188 class B. Class C has a 255.255.255.0 netmask, and thus includes only 256 IPs. For example, the Computer Systems Lab owns 16 Class C's: 198.38.16 - 198.38.31. The body that gives these IP blocks out is arin.net. In addition to all that, there are a few IP's that are reserved for special purposes. The 10 Class A is reserved for local networks, as well as the 192.168 Class B. The 127 Class A is reserved for loopback networks. All Class A's 224 and above are reserved for multicast and other similar applications.

#### Subnets

Since it is impractical to have each and every computer on the same physical network, subnets were created. Each subnet is defined by a netmask, which says how many computers there are on the subnet. For example, if a computer's IP is 198.38.17.1 and its netmask is 255.255.248.0, it can expect 198.38.16-23 to be in its local network. The way it would determine that is that if its IP and the destination IP when AND'ed with the netmask come out to the same thing, that would mean that they are on the same physical network.

#### ARP

ARP \(Address Request Protocol\) is the protocol used to figure out what IP maps to what MAC address. This protocol really does not belong in the layer system, and neither does ICMP. The way it works is that an ARP packet is sent out which says something to the effect of "I'm looking for the MAC address that holds the IP xxx.xxx.xxx.xxx" \(this packet is broadcast\). After this, when the requesting computer finds out the correct MAC address, it can start sending proper Ethernet frames to it.

#### IPX

IPX is an alternative to IP made by Novell. The way its addressing is done is that each node's IPX address is based on the MAC address of the NIC. In order for IPX to work, the router connecting two segments has to know about IPX and be able to forward it. Since IP became the mainstream standard, and most routers on the Internet do not forward IPX packets, it can only be used in a LAN setting.

### Layer 4

Layer 4 refers to the layer that is set on top of the IP/IPX layer.

#### ICMP

ICMP \(Internet Control Message Protocol\) is a port-less protocol which is most commonly used for two things: ICMP Echo \(aka ping\), and traceroute. Other than that, it's useless. For now, the workings of traceroute are black magic, unless you feel like figuring out its code \(the beauty of Open Source\).

#### TCP

There is a book about TCP, called \[TCP/IP Illustrated\], which is in two volumes, each of about 1000 pages. Thus, I will only try to make important points about TCP rather than describe it in the fullest possible way. TCP allows for direct connections between two IPs. Moreover, it guarantees that the data that leaves from one end gets to the other. It does this by using rather complex sequencing, and acknowledgment, which is beyond the scope of this document. With TCP you can have multiple ports per IP, and there are POSIX functions in Linux which will let you bind\(2\) to those ports and listen\(2\) on them in order to accept\(2\) incoming connections. This is all done through the socket\(2\) interface. To read more about these system calls, one can look up each of the previously referred to man pages.

#### UDP

UDP is the same thing as TCP, only it in no way guarantees delivery. This is usually not a problem on a local network, but when sending packets over the Internet, it is not uncommon to lose a few here and there. One advantage of UDP is that it can be used to multicast packets. This means that one and the same UDP packet will be broadcast to all nodes on a local network. This can facilitate large, repetitive data transfers. One obvious application of this is video broadcast, where one video server would multicast the video stream. Multicast uses a special set of IPs, 224. and above.

## Switching

The difference between a hub and a switch is that a hub just forwards all incoming traffic to all of its ports. As such, all computers that sit directly on a hub can listen to any traffic that goes through that hub. In addition to being inefficient, this allows for the possibility of a NIC to go into promiscuous mode and pass all of that traffic to the user. \(Under regular conditions, NICs throw out all traffic not destined for their MAC address. In promiscuous mode, they take in all traffic.\) A switch is smarter than that. It looks at the incoming traffic, and routes it directly to certain ports depending on the contents of that traffic. Switching can be done on multiple layers.

### Layer 2

Layer 2 switching is the most common type of switching that all switches can do. These switches figure out what MAC addresses sit on which ports, and as such, they provide direct port to port paths for communication. Their traffic will not be "overheard" by any other ports. \(Of course if you have a really nice switch, then you can set monitoring ports which will be able to hear all or some traffic.\) In addition to this layer of security, switches allow ports of different speeds to be connected. They achieve this by having a relatively large packet buffer sitting on each port which can store some data. Switches also allow for Full Duplex operation. This means that computers can send out packets all at the same time without them colliding. Collisions arise from having multiple interfaces send a packet at the same time. However this is avoided as the switch allows each port to see only the traffic which relates to it. Collisions can be a real problem with hubs especially, since it means that no computer sitting on a hub can send a packet while another is sending.

### Layer 3/4

Layer 3/4 switches take switching to the TCP/IP, UDP/IP, and IPX levels. These switches can take instructions such as "All IPs in this range go to this port" or "TCP port 80 of this IP goes to this port, while TCP port 21 on that IP should not go through at all." This is very similar to what routers do, and in fact is the same.

## Routing

Routers are basically more versatile layer 3/4 switches. Most routers can handle a very large variety of inputs. For example, most low-end routers will have a 10BT and FDDI ports. FDDI is used for T1-type connections and is never seen on a switch. Routers can also handle such inputs as OC-12 and above. Some switches can handle an OC-12 input \(Gigabit\), but there are no switches that can handle a dozen OC-148 connections \(10 Gigabit apiece\). In general, routers are used to "convert" between different types of mediums \(and route traffic through them\), while switches are used to connect many machines together.

### Purpose

The purpose of routing may not seem obvious. The question arises, "Why not put all computers onto the same physical layer 2 network?" The answer is, "Try it." Ignoring the different connection type issue, let's do the math for the Internet, which is nothing more than a big network. If there are a meager 6,000,000 computers on the Internet all connected to each other through 100Mbps connections \(and a whole bunch of switches\), then let us take a look at some of the procedures. For example, let us look at a simple broadcast packet that is used very often: ARP. When a computer sends out an ARP packet, then all other computers in the entire world will receive that ARP and one will respond. So at maybe an average of 1 ARP/minute/computer, each being 64 bytes, we get 6.1MB/s of sustained traffic over the whole network taken up by just ARP traffic. Another theoretical 6MB/s are left for all other communication. \(What if there are 60,000,000 computers?\) Or in a different situation, let's say there's one person who thinks, "Let's see what happens if I send out broadcast packets at the full 100Mbit/s." Now, these broadcast packets get sent to each and every computer. That would mean that just this one person is taking up all bandwidth available. Needless to say, there needs to be a better way of doing this.

### Overview

Most routers have a connection to the inside world and the outside world. With only two connections, its job is simple: relaying packets back and forth between interfaces, dropping broadcast and multicast packets. The routers also usually will take a look at the traffic that goes in and out, blocking some of the traffic and redirecting the other portion. For example, it is usually a good idea to block unnecessary ports to the outside world to prevent an attack. A router would take care of that.

### ARP Proxying

ARP Proxying is used when two separate network segments exist, but need to be united into one. Thus, the router in between them will proxy the ARP requests between both segments, effectually making the nodes appear to be on the same segment. We used to do this for the soundlab in order to avoid any complications. However this is a very hack-ish way of setting things up, and should very rarely be used. It is usually better to resolve conflicts in other ways.

### IPchains and IPtables

These are the tools that allow you to use filtering/forwarding features of the Linux kernel. ipchains is for 2.2 \(though 2.4 also has compatibility options for it\), while iptables is for 2.4 kernels. These are mainly used in two situations: for NAT \(aka IP Masquerading\), and routing. Take a look at the HOWTOs provided on [The Linux Documentation Project](http://www.tldp.org) with respect to both of those tools.

## VLANs

Some switches just have too many ports, and you would like to use one switch for multiple physical networks which you do not want to be connected together. VLANs allow you to split up a single physical connected network into logical segments, and disallow communications between the segments.

### Cluster

The new cluster is a good example of the use of a VLAN. The machines must talk amongst themselves to relay information, but outside machines should not be able to intercept them or be able to directly communicate with them. Traffic is only allowed through a few machines that are connected to both the cluster VLAN and to the main VLAN in the lab.

## Notes

Most of the text in this page was taken from [The Syslab Book](https://livedoc.tjhsst.edu/wiki/The_Syslab_Book), with slight alterations.

