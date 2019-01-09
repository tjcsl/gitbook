# Ceph

## Introduction

The Computer Systems Lab uses [Ceph](https://ceph.com) as its main storage infrastructure. We chose to use Ceph because of its scalability, redundancy, and reliability. 

Ceph is developed by Red Hat, Inc. and is used by organizations like CERN, Cisco, T-Mobile, and various educational institutions around the world.

## Servers

Currently, we have 3 OSD servers and 3 monitor/manager/metadata servers in the Ceph cluster. Each OSD server \([karel](../../../machines/ceph/karel.md), [wumpus](../../../machines/ceph/wumpus.md), and [stobar](../../../machines/ceph/stobar.md) \) has 12×4TB HDD and 2×400GB SSDs.

## Architecture

Ceph is composed of four different daemons: monitors, managers, object storage daemons, and metadata server daemons.  The backend storage layer is called the Reliable Autonomous Distributed Object Store \(RADOS\).

 Ceph was developed from the ground up to protect against hardware failures and corruption by distributing data across multiple hard drives hosted on multiple servers. The data written to each hard drive is managed by an object storage daemon \(OSD\) and the state of the cluster is managed by a set of monitors \(mon\). Using a state-of-the art algorithm, data is stored in various placement groups \(PGs\) which are replicated across multiple OSDs and served out to appropriate clients by the monitors.

The data itself is distributed according to the Controlled, Replicated Under Scalable Hashing \(CRUSH\) algorithm.  At least a majority of the monitors must agree to the state of the cluster and resolves differences in the main cluster map.  The monitors also store the CRUSH map.

Manager daemons provide an interface for outside systems to access data about the cluster.

Metadata server daemons are responsible for managing metadata for the CephFS file system.

