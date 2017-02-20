## InfluxDB/Telegraf/Grafana Using Docker
-----------------------------------------
There are many options for setting up a new TSDB/Collector/Visualization stack,
one of which is using existing supported Docker images.

### Quick Start
1.  Select a desired host (_&lt;dockerhost&gt;_), such as any of the common Linux distributions.
2.  Ensure required docker components and dependencies exist
  -  (.deb) `sudo apt-get -y install docker-engine`
  -  (.rpm) `sudo yum -y install docker`
3.  Identify local persistent storage location(s)
  1.  InfluxDB Data (_&lt;influxdir&gt;_)
  2.  Grafana Data (_&lt;grafanadir&gt;_)
  3.  Configuration Dir (_&lt;configdir&gt;_)
  4.  (Optional) SNMP MIBs Dir (_&lt;mibsdir&gt;_)
4.  Ensure persistent storage locations exist
  -  `mkdir -p <influxdir> <grafanadir> <configdir> <mibsdir>`
5.  If using the Telegraf SNMP plugin, obtain dependent MIB files if needed.
  - Place these in _&lt;mibsdir&gt;_ identified above
6.  Copy the `telegraf.conf` from this directory to _&lt;configdir&gt;_
  1.  Edit `telegraf.conf` and either explicitly define `hostname` (do not leave empty) or change `omit_hostname` to `true`.  
  2.  Append to `telegraf.conf` any additional telegraf configurations identified with the desired dashboard
7. Start three Docker containers:
  1.  InfluxDB
    -  `docker run --name influxdb --rm -d -p 8086:8086 -v <influxdir>:/var/lib/influxdb influxdb`
  2.  Telegraf
    -  `docker run --name telegraf --rm -d -v <configdir>/telegraf.conf:/etc/telegraf/telegraf.conf:ro -v <mibsdir>:/root/.snmp/mibs:ro --net=container:influxdb nuntz/telegraf-snmp`
  3.  Grafana
    -  `docker run --name grafana --rm -d -p 3000:3000 -v <grafanadir>:/var/lib/grafana grafana/grafana`
8.  Point a browser to the new Grafana instance at `http://<dockerhost>:3000`
9.  Login with default user/pass of `admin/admin`
10. Click 'Add a Datasource'
  1.  Use 'Telegraf' for the name
  2.  Select 'InfluxDB' as the type
  3.  Use 'http://&lt;dockerhost&gt;:8086' as the URL
  4.  Use 'direct' for access
  5.  Use 'telegraf' for InfluxDB Database (must match the `database` defined in `telegraf.conf`)
  6.  Click 'Add'
11.  From the main menu, select 'Dashboards -> Import' to start importing and using dashboards.

### References
-  Official InfluxDB Docker Image:  [https://hub.docker.com/_/influxdb/]()
-  Official Telegraf Docker Image:  [https://hub.docker.com/_/telegraf/]()
-  Alternate Telegraf Docker Image:  [https://hub.docker.com/r/nuntz/telegraf-snmp/]()
-  Official Grafana Docker Image:  [https://hub.docker.com/r/grafana/grafana/]()

### Notes
-  The above assumes the use of the Telegraf SNMP input plugin.  The official Telegraf Docker image currently lacks the needed support for this plugin, so an alternate Docker image based on the official image is used above.
-  The OS default distributions may have older versions of Docker.  Often
the official docker package repositories are configured and used instead
of the OS-provided versions.  See [Install Docker Engine](https://docs.docker.com/engine/installation/)
for detail.
-  Once the stack is up and running, the database (InfluxDB) should be configured with authentication (users/passwords) and authorization (grants).  A read/write user for Telegraf and a read-only user for Grafana is recommended.
-  All three components support all of their standard configurations with their Docker implementations.  See the References above for specific documentation.
