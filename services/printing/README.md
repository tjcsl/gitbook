---
description: The CSL's printing infrastructure
---

# Printing

## Printing

Printing is a production service offered by the Computer Systems Lab. We provide the means for students to print from Ion and the CSL workstations.

The contact person for the printing service and associated infrastructure is [the Printing Lead](../../general/sysadmins-list.md#current-leads).

### Printers

We currently have three functioning printers:

* printer202 \(in Room 202\)
  * This is a HP LaserJet P3015
* 198.38.18.3 \(in Room 200C\)
  * This is a HP LaserJet P3015
* 198.38.18.4 \(in Room 200\)
  * This is a HP LaserJet P3015

### Ion Printing

There is a web interface to provide printing services on Ion at [https://ion.tjhsst.edu/printing](https://ion.tjhsst.edu/printing). Ion uses the CUPS server at `cups2.csl.tjhsst.edu` to manage jobs. Relevant code can be found [here](https://github.com/tjcsl/ion/tree/master/intranet/apps/printing).

## Workstation Printing

Each workstations uses the CUPS server at `cups2.csl.tjhsst.edu` to manage jobs and send them to the printers.

## Abuse Protection

Jobs are limited to 10 pages. Abuse of printing privileges may result in appropriate punishment as determined by the Faculty Sponsor and lead Sysadmins.

