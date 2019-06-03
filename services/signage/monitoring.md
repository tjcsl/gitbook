# Monitoring

## Grafana

A [Grafana dashboard](https://grafana.tjhsst.edu/d/9P-CqBmZz/signage?orgId=1) (only accessible via a CSL VPN) offers a comprehensive overview of all of the signages in the school.  The dashboard posts alerts to `#signage` when displays malfunction.

## signage-exporter

The Grafana dashboard is supported by a [prometheus backend](../../technologies/monitoring/grafana), which scrapes data from each individual display's `signage-exporter`.  `signage-exporter` is stored in the [ansible](../../technologies/tools/ansible) repository under `roles/signage/files/signage-exporter.py`.  It is run from i3config and detects the connected display and touch inputs.
