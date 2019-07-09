# VM Servers

**VM Servers** \(also known as VM hosts\) host virtual machines for the various CSL services we maintain.  The VM Servers themselves run off a local hard disk \(often in a RAID configuration?\).

## Reasoning

Although VM servers have existed for multiple years in the Lab, complete virtualization of critical services occurred in the summer of 2017.  The reason for this was to allow services to be brought up quickly.  In addition, our current VM setup permits us to bring up new machines more quickly and fully utilize [RBD block storage as our storage backend](../ceph/).

## Exceptions

Various services are not virtualized at this time and probably should never be.

* [NTP](../../technologies/networking/ntp.md) is an extremely critical machine since it provides time for the whole Lab.
* [ns1](../../technologies/networking/dns/) is not virtualized because it provides DNS to not only VMS but also to the main Ceph backend
* [centauri ](../sun-servers/centauri.md)is not virtualized because it provides critical authentication services to the rest of the Lab \(and also the VM hosts themselves\)



