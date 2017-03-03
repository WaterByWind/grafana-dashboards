## Grafana Dashboards
---------------------

A set of miscellaneous [Grafana](http://grafana.org) dashboards along with required collector configurations.


### Quick Start
To use any of these:  
1.  Edit and/or merge any required collector configurations.  
2.  Restart/refresh collector if necessary.  
3.  Import the .json as a new dashboard in your local Grafana instance.  


### Contents
Each dashboard is contained in its own directory, along with any required dependencies and configurations.
- `HP-LaserJet`
  - Detailed dashboard for monitoring of HP LaserJet printers via SNMP
- `UBNT-EdgeRouter`
  - Detailed dashboard for monitoring of Ubiquiti EdgeRouters via SNMP
- `UniFi-UAP`
  - Detailed dashboard for monitoring of Ubiquiti UniFi Access Points via SNMP
- `Extra`
  - Sample Quick Start for InfluxDB/Telegraf/Grafana stack using Docker

Potential additional dashboards _may_ include:  EdgeSwitch, BSD, Linux, Solaris, Raspbian (Raspberry Pi)


### References
These dashboards, along with screenshots, may also be found at https://grafana.net/waterbywind
