# Week - 8

**This week you will be monitoring the setup for the datastore you worked on last week.**

1. Setup **collectd** to monitor various metrics for operating system and one of the following datastores ES/RMQ/AS

2. Setup influxdb on a VM

3. Send **collectd** metrics to InfluxDB

4. Plot these metrics on a Grafana dashboards

5. Setup Riemann on a VM

6. Write a script to collect the metrics previously collected by collectd and send it to riemann

7. The metrics should go to InfluxDB from Riemann

8. metrics such as disk usage/ram usage/HWM etc should have a threshold (80%) and should send a critical alert to Riemann once the threshold is breached.

You can divide these amongst yourselves and create a document on how to install/configure them, what added advantages/disadvantages do these tools provide over the other monitoring tools you've already explored:

1. Prometheus

2. Nagios

3. Metricbeat

4. Sysdig

5. Netdata

6. Diamond collector and graphite

7. Icinga2
