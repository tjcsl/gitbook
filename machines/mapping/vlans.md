# VLANs

The following is a list of VLANs and the corresponding address spaces for reference. This includes IPv4 and IPv6. Most traffic in the CSL is tagged with one of these VLANs.

## 16

* SAN VLAN
* IPv4: 172.16.0.0/16 \(172.16.0.0-172.16.255.255\)
* IPv4 Gateway: Unrouted
* IPv6: None

## 1600

* CSL Servers/Services
* IPv4: 198.38.16.0/23 \(198.38.16.0-198.38.17.255\)
* IPv4 Gateway: 198.38.17.254
* IPv6: 2001:468:cc0:1600::/64
* IPv6 Gateway: 2001:468:cc0:1600::
* Note: In general, virtual services are assigned 198.38.17.0/24 addresses, and physical servers \(such as VM Servers and Storage endpoints\) are assigned 198.38.17.0/24 addresses. 
* Please contact the lead Sysadmins before assigning addresses in the range 198.38.17.128/27

## 1800

* Workstations
* IPv4: 198.38.18.0/25 \(198.38.18.0-198.38.18.127\)
* IPv4 Gateway: 198.38.18.126
* IPv6: 2001:468:cc0:1800::/64
* IPv6 Gateway: 2001:468:cc0:1800::2

## 1801

* Laptops \(Sysadmin\)
* IPv4: 198.38.18.128/26 \(198.38.18.128-198.38.18.191\)
* IPv4 Gateway: 198.38.18.190
* IPv6: 2001:468:cc0:1801::/64
* IPv6 Gateway: 2001:468:cc0:1801::

## 1802

* DMZ \(RAS, etc\) \(publicly-facing services\)
* IPv4: 198.38.18.192/26 \(198.38.18.192-198.38.18.255\)
* IPv4 Gateway: 198.38.18.254
* IPv6: 2001:468:cc0:1802::/64
* IPv6 Gateway: 2001:468:cc0:1802::

## 1900

* Experimental
* IPv4: 198.38.19.0/24 \(198.38.19.0-198.38.19.255\)
* IPv4 Gateway: 198.38.19.254
* IPv6: 2001:468:cc0:1900::/64
* IPv6 Gateway: 2001:468:cc0:1900::

## 2000

* Cluster
* IPv4: 198.38.20.0/24 \(198.38.20.0-198.38.20.255\)
* IPv4 Gateway: 198.38.20.254
* IPv6: 2001:468:cc0:2000::/64
* IPv6 Gateway: 2001:468:cc0:2000::/64

## 2100

* Understudy
* IPv4: 198.38.21.0/25 \(198.38.21.0-198.38.21.127\)
* IPv4 Gateway: 198.38.21.126
* IPv6: 2001:468:cc0:2100::/64
* IPv6 Gateway: 2001:468:cc0:2100::

## 2101

* Sunrays
* IPv4: 198.38.21.128/25 \(198.38.21.128-198.38.21.255\)
* IPv4 Gateway: 198.38.21.254
* IPv6: 2001:468:cc0:2101::/64
* IPv6 Gateway: 2001:468:cc0:2101::

## 2200

* OpenVPN
* IPv4: 198.38.22.0/25 \(198.38.22.0-198.38.22.127\)
* IPv4 Gateway: 198.38.22.126
* IPv6: 2001:468:cc0:2200::/64
* IPv6 Gateway: 2001:468:cc0:2200::
* Note: This should be terminated at the VPN VM; this VLAN shouldn't reach the core switch.

## 2201

* IPSecVPN
* IPv4: 198.38.22.128/25 \(198.38.22.128-198.38.22.255\)
* IPv4 Gateway: 198.38.22.254
* IPv6: 2001:468:cc0:2201::/64
* IPv6 Gateway: 2001:468:cc0:2201::
* Note: This should be terminated at the VPN VM; this VLAN shouldn't reach the core switch.

## 2300

* Management \(iLO, LOM\)
* IPv4: 198.38.23.0/25 \(198.38.23.0-198.38.23.127\)
* IPv4 Gateway: 198.38.23.126
* IPv6: 2001:468:cc0:2300::/64
* IPv6 Gateway: 2001:468:cc0:2300::

