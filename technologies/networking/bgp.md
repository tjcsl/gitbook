# BGP

**Border Gateway Protocol (BGP)** is a protocol used by public-facing routers to build and maintain their routing tables. A router or network that runs BGP is known as an Autonomous System (AS) and is assigned an AS number by the Internet Assigned Numbers Authority (IANA). TJ's AS number is 3140.

In order to even remotely understand this, please read the [wikipedia article](https://en.wikipedia.org/wiki/Border_Gateway_Protocol).

## Peer routes

| Peer | AS number |
| ---- | --------: |
| Virginia Tech | 40220 |
| Verizon/UUNet | 701 |

| Advertised IP addresses |
| ----------------------- |
| 198.38.16.0/20          |
| 2001:468:cc0::/48 (AS 40220 only) |

 In order to build the Internet, Autonomous Systems set up peering agreements with each other to allow traffic to route between their networks. For example, TJ peers with Virginia Tech (AS 40220) and Verizon/UUNet (AS 701, this route is currently down due to line issues). Each AS then advertises prefixes to its peers indicating the networks that it has access to. In this case, TJ advertises the prefixes 198.38.16.0/20 and 2001:468:cc0::/48 (AS 40220 only). In return, we receive from Virginia Tech and Verizon a list of prefixes which they are able to route to (in our case, the rest of the internet).

 ## Building the Routing Table

| External Networks | Order of Preference | Speed |
| ----------------- | ------------------- | ----- |
| [National LambdaRail (NLR)](http://www.nlr.net) | 1 | 1Gb/s |
| [Internet 2](http://www.internet2.edu) | 2 | 1Gb/s (lower capacity?) |
| Verizon/UUNet Internet | 3 | 15Mb/s |

So now we have a bunch of AS numbers and the prefixes they can reach; now how do we get our cat pictures. Using our collection of AS numbers and prefixes, TJ's border router builds a routing table based on its internal policy that decides how it is going to send traffic to every advertised IP on the internet. The primary means of deciding which path to take is path length (this is not necessarily the same thing as shortest distance because path length is measured by how many different AS we jump through). However, this policy can be overridden to a certain extent by the network admins. For example, via Virginia Tech, we have access to two private networks, Internet2 (<http://www.internet2.edu/>) and National LambdaRail (<http://www.nlr.net/>), in addition to our connection to "The Internet". One of the policies we have in place is that, all other things being equal, we will prefer to send traffic over NLR because it is generally a higher capacity network. Failing that we prefer Internet2 and then finally fallback to the Internet.

A second very important policy that we have in place is to always prefer our 1Gb connection to Virginia Tech over our 1.5Mb T1 connection to Verizon/UUNet. This is important because otherwise all traffic to other Verizon/UUNet customers (like FCPS) would prefer the very slow T1 connection because it would be a "shorter" path. The way we accomplish this is by pre-pending our AS repeatedly to our prefix when we announce it to Verizon/UUnet. This way Verizon sees a very long path (701->3140->3140->3140->198.38.16.0/20) and will prefer the shorter path through Virginia Tech.

One important note about these routing policies is that they ONLY affect packets that leave TJ for other networks. Packets coming the other way are routed according to the policies in place on the Border Router of the appropriate organization. This means that it is quite possible for you to send a request for a website down one path and receive the reply on a totally different path. As an example, many Universities prefer their Internet2 connection because it is more reliable but they still have an NLR connection for high-speed communication with groups not on Internet2. Therefore, when talking to them, we will send our packets over NLR but receive their replies via Internet2.

## Security

The two main vulnerabilities in BGP are BGP hijacking and route leakage. In BGP hijacking, an AS advertises routes for prefixes that it does not actually control thus providing its peers (and other "nearby" AS) with an apparently shorter (and therefore likely preferred) path. This is what happened in April of 2010 when an ISP in China hijacked routes for a number of very popular sites (http://bgpmon.net/?p=282). Route leakage is generally the result of a misconfigured multi-homed router (a router that is connected to more than one AS, like TJ's). For example, if TJ were to re-advertise the routes we receive from Verizon to Virginia Tech and vice versa, these systems could potentially decide that the quickest path between AS 40220 and AS 701 is via AS 3140 (TJ) resulting in a DoS on TJ's router as well as traffic in between those systems. Route leakage between two routers can also cause the routing tables in those routers to grow exponentially in size until one of the routers runs out of Memory and crashes. This was the the root cause of TJ's internet outage a while back (a route leakage on Internet2 overloaded and crashed the Virginia Tech router that we connect to).

To prevent route leakage, most ISPs will have prefix filtering in place to prevent any of their clients from causing such a disruption. For example, neither Virginia Tech nor Verizon will accept any prefix advertisements from us except 198.38.16.0/20 and 2001:468:cc0::/48 (or subnets thereof). Of course, not all ISPs are this careful and diligent in configuring their peerings. In addition, in inter-ISP peerings where both ISPs have access to a substantial portion of the Internet via multiple peers; prefix filtering is not necessarily feasible as it would not be possible to build and maintain the necessarily access lists.

## Additional information

 * BGP IPv4 peering map for AS 3140 (TJHSST): <http://bgp.he.net/AS3140#_graph4>
 * BGP IPv4 peering map for AS 21984 (FCPS): <http://bgp.he.net/AS21984#_graph4>

 You can find the most up-to-date BGP information on our [Border Router](../../machines/switches/README.md)
