# Xnor

*xnor* is the Computer Systems Lab's core fiber optic router and switch. Although for all intents and purposes xnor behaves as a single switch, it is actually comprised of two [Cisco Catalyst 4500-X WS-C4500X-F-16SFP+](http://www.cisco.com/c/en/us/products/collateral/switches/catalyst-4500-x-series-switches/data_sheet_c78-696791.html) switches, linked together using Cisco Virtual Switching System for redundancy. Each switch has 16 SFP+ ports, expandable up to 24.

##Uplinks
xnor has one 10Gbps fiber optic uplink to TJ's core switch for IPv4 traffic, and one 10Gbps fiber optic uplink directly to TJ's core router for IPv6 traffic. These are unfortunately not redundant for now, as there are limited ports available on the TJ core side. Thus, one of xnor's physical switches has the IPv4 uplink, and the other one has the IPv6 physical uplink.

## General Setup
xnor serves as the CSL's aggregation switch and router. All top-of-rack switches are connected to xnor using two redundant 10Gbps fiber connections (one to each physical switch). Any traffic which needs to be routed across VLANs or needs to be routed to TJ's border network is routed by xnor.
