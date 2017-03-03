## Templated Grafana Dashboard for Ubiquiti Networks EdgeRouters

Detailed dashboard for monitoring of Ubiquiti EdgeRouters via SNMP.


The Telegraf collector configuration is MIB-based so all of the required MIBs will need to be available for the collector to perform the needed translations.  Most SNMP distributions available on most platforms already provide these.  This configuration uses the "new" SNMP plugin for Telegraf, so SNMP monitoring must be enabled in the EdgeOS configurations.  The 'agents' list in the configuration is of the actual EdgeRouters themselves.


### Important Dependency Note
The included Telegraf:SNMP plugin configuration depends upon a feature that is not yet part of the released Telegraf 1.2.
Several collected object tables leverage a non-accessible index, such as to identify the IP Version for the statistics in each row.  The actual index is the sub-id for each table element, of which the Telegraf:SNMP plugin does not currently make available as a field or tag.  However there is a pull-request to add this capability using a new flag 'index_as_tag'
-  'index_as_tag' on tables requires PR \#2366 for support
  -  See https://github.com/influxdata/telegraf/issues/1948

There are two options:
 1.  Wait for a formal release of Telegraf with the needed support.  The noted PR is tagged for the Telegraf v1.3 milestone.  
 2.  Build custom version of Telegraf with the required PR applied.  The changes needed are entirely contained within the single `plugins/inputs/snmp/snmp.go` source file so this is a relatively simple change.  


### Dependencies
1. [InfluxDB](https://docs.influxdata.com/influxdb/) as the time-series database
2. [Telegraf](https://docs.influxdata.com/telegraf/) as the collector


### Quick Start
1. Enable SNMP monitoring on your EdgeRouters via EdgeOS BUI or CLI.
2. Merge the included `telegraf-inputs.conf` with your local Telegraf instance configuration (or create a new instance)
  1. Edit the SNMP 'community' string as appropriate
  2. Edit the 'agents' list to include all of your monitored EdgeRouters
  3. If you do _not_ already have an InfluxDB output configured in your Telegraf instance:
    1. Merge the included `telegraf-outputs.conf` with your local Telegraf instance configuration:
    2. Edit the URL to your new InfluxDB instance
    3. Edit the username and password for this InfluxDB instance as appropriate
3.  Restart Telegraf
4.  Import the included `ubnt-edgerouter-dashboard.json` as a new dashboard into your local Grafana instance


### Notes
As with any SNMP-based monitoring solution with EdgeRouters, IP forwarding statistics will not reflect packets that have been hardware-offloaded.


#### Files
- `ubnt-edgerouter-dashboard.json`:
 - Actual Grafana dashboard (no edits required)
- `telegraf-inputs.conf`:
 - Telegraf input configuration for use with this dashboard (edits required)
- `telegraf-outputs.conf`:
 - Optional Telegraf output configuration for use with this dashboard (edits required)


#### References
This dashboard is not yet available at https://grafana.net/dashboards/, pending a formal release version of Telegraf
with the noted 'index_as_tag' support (or similar)
