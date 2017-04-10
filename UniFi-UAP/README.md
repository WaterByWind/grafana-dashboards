## Templated Grafana Dashboard for Ubiquiti UniFi Access Points

Detailed dashboard for monitoring of Ubiquiti UniFi Access Points via SNMP.

___Please Note___ that the collector configuration has been updated to support the current dashboard. If you are updating a previous revision of the dashboard please check the collector configuration as well to ensure it is current.

This dashboard works with Gen2 and later APs.  Gen1 APs *do* support monitoring via SNMP, however they provide very little instrumentation due to the lack of support for the IF-MIB and UBNT-UniFi-MIB mibs.

The Telegraf collector configuration is MIB-based so all of the required MIBs will need to be available for the collector to perform the needed translations.  Most SNMP distributions available on most platforms already provide these with one exception (see below).  This configuration uses the "new" SNMP plugin for Telegraf, so SNMP monitoring must be enabled in the UniFi controller.  The 'agents' list in the configuration is of the actual APs themselves, however, and not the controller.

This dashboard does display the SNMP contact and location detail, but there is currently no method to configure these via the UniFi controller.  Instead, these may be set (optionally, as desired) using the method described in [UniFi - How to make persistent changes to UAP(s) system.cfg](https://help.ubnt.com/hc/en-us/articles/205223330-UniFi-How-to-make-persistent-changes-to-UAP-s-system-cfg).  Two simple additions to the `config.properties` file apply this to all APs in a given site (adjust the item number and values as appropriate):
```
config.system_cfg.1=snmp.contact=admin@mydomain.com
config.system_cfg.2=snmp.location=somewhere
```

### Dependencies
1. [InfluxDB](https://docs.influxdata.com/influxdb/) as the time-series database
2. [Telegraf](https://docs.influxdata.com/telegraf/) as the collector


### Quick Start
1. Enable SNMP monitoring on your UAPs via UniFi controller
2. Merge the included `telegraf-inputs.conf` with your local Telegraf instance configuration (or create a new instance)
  1. Edit the SNMP 'community' string as appropriate
  2. Edit the 'agents' list to include all of your monitored UAPs
  3. If you do _not_ already have an InfluxDB output configured in your Telegraf instance:
    1. Merge the included `telegraf-outputs.conf` with your local Telegraf instance configuration:
    2. Edit the URL to your new InfluxDB instance
    3. Edit the username and password for this InfluxDB instance as appropriate
3.  Restart Telegraf
4.  Import the included `unifi-ap-dashboard.json` as a new dashboard into your local Grafana instance


### Notes
The CCQ & Stations graphs may need to be adjusted if there are more than than two total vAPs (multiple SSIDs per radio) selected, as the legend may grow too large with the graphs configured as-is.  This would be a matter of preference.  The template does allow a selection of vAPs to display as an option.

The Unifi MIB locations may be found in the [UniFi Updates Blog](https://community.ubnt.com/t5/UniFi-Updates-Blog/bg-p/Blog_UniFi) announcements for UniFi releases. These should be downloaded and placed in the default SNMP MIBs location on the server where the Telegraf instance is running.  Example copies of these two files are also included in the `mibs` dir as well.

The FROGFOOT-RESOURCES-MIB will also be needed, but a simple web search should provide many references to sources if needed.  Some SNMP distributions already include this, but not all.  An example of this is included in the `mibs` dir as well.


#### Files
- `unifi-ap-dashboard.json`:
 - Actual Grafana dashboard (no edits required)
- `telegraf-inputs.conf`:
 - Telegraf input configuration for use with this dashboard (edits required)
- `telegraf-outputs.conf`:
 - Optional Telegraf output configuration for use with this dashboard (edits required)
- `mibs/`:  
 - Example MIB files for use with this dashboard (no edits required):
     - `FROGFOOT-RESOURCES-MIB`
     - `UBNT-MIB`
     - `UBNT-UniFi-MIB`


#### References
This dashboard is also available at https://grafana.net/dashboards/1486, including screenshots.
