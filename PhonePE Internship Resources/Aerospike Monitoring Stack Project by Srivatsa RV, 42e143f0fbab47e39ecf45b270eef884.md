# Aerospike Monitoring Stack 
Project by: Srivatsa RV, SRE Intern, T3

## Proposed Architecture

![Aerospike%20Monitoring%20Stack%20Project%20by%20Srivatsa%20RV,%2042e143f0fbab47e39ecf45b270eef884/AS-Prometheus-Grafana.png](Aerospike%20Monitoring%20Stack%20Project%20by%20Srivatsa%20RV,%2042e143f0fbab47e39ecf45b270eef884/AS-Prometheus-Grafana.png)

## Components Involved:

- Aerospike Server. (Documentation assumes AS Cluster is up and running)
- Prometheus Server (Standalone) (2.27.1)
- Aerospike-Prometheus-Exporter (1.3)
- Grafana Server 7.5.7 (Standalone)

# 1-Installing Prometheus

```
**INSTALLATION COMMANDS: Follow in Order

1- Update and Curl the Source Package**

sudo apt-get update && apt-get upgrade
export https_proxy="http://tinyproxy:8888"
export http_proxy="http://tinyproxy:8888"
wget https://github.com/prometheus/prometheus/releases/download/v2.27.1/prometheus-2.27.1.linux-amd64.tar.gz
tar -xvf prometheus-2.27.1.linux-amd64.tar.gz
mv prometheus-2.27.1.linux-amd64 prometheus-files

**2- Create a Prometheus user, required directories, 
   and make Prometheus the user as the owner of those directories.**

sudo useradd --no-create-home --shell /bin/false prometheus
**sudo mkdir /etc/prometheus 
sudo mkdir /var/lib/prometheus**
sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus

**3- Copy prometheus and promtool binary from prometheus-files folder
   to /usr/local/bin and change the ownership to prometheus user.**

sudo cp prometheus-files/prometheus /usr/local/bin/
sudo cp prometheus-files/promtool /usr/local/bin/
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool
 
**4- Move the consoles and console_libraries directories from prometheus-files to
   /etc/prometheus folder and change the ownership to prometheus user.**

sudo cp -r prometheus-files/consoles /etc/prometheus
sudo cp -r prometheus-files/console_libraries /etc/prometheus
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
sudo chown -R prometheus:prometheus /etc/prometheus/prometheus.yml (yet to make atm)
```

## 1.1-Setup Prometheus Service

```
**1- Create Prometheus Service File so that we can start using systemctl or service

#Paste this at /lib/systemd/system/prometheus.service path
-----------------------------------------------------------

[Unit]**
Description=Prometheus
Wants=network-online.target
After=network-online.target

**[Service]**
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

**[Install]**
WantedBy=multi-user.target

**2- Start Prometheus Serivce**

sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl status prometheus

**You should have Prometheus Service Running.......
  
3- Verify Service by any of the following methods,** 

- Check from Prometheus Web UI (Recommended) **http://localhost:9090**
- Curl endpoint > **$** curl stg-hostname.phonepe.nb6:9145/metrics
```

## 1.3- Configure prometheus.yml file and add targets

```
**CONFIGURE PROMETHEUS FILE 

PATH: cd /etc/prometheus/prometheus.yml 
NOTE: By default, Prometheus is configured to monitor itself, also exposes its own metr

Sample Configuration File
-------------------------**

global:
  scrape_interval: 10s

rule_files:
  - "/etc/prometheus/aerospike_rules.yml"

scrape_configs:
  - job_name: 'aerospike'
    scrape_interval: 60s
    static_configs:
            - targets: ['10.57.14.65:9145','10.57.14.66:9145','10.57.14.67:9145', 'stg-aerospike101.phonepe.nb6:9145', 'stg-aerospike102.phonepe.nb6:9145', 'stg-aerospike103.phonepe.nb6:9145', 'stg-aerospike111.phonepe.nb6:9145', 'stg-aerospike112.phonepe.nb6:9145', 'stg-aerospike113.phonepe.nb6:9145']

**-------------------------**

**Change the ownership of the file to prometheus user**

sudo chown prometheus:prometheus /etc/prometheus/prometheus.yml
```

## 1.4- Alternatively, Setup File Based Service Discovery

### 1.4.1 - Create "targets.json" file in /etc/prometheus

```yaml
[
  {
    "labels": {
      "job": "<job-name>",
      "env": "<env-name>"
    },
    "targets": [
      "<target-1>",
      "<target-2>",
      "<target-3>"
   ]
  }
]
```

### 1.4.1 - Create "targets.json" file in /etc/prometheus

```yaml
global:
  scrape_interval: 1m
  scrape_timeout: 10s

- job_name: 'aerospike-tls'
    static_configs:
      - targets:
        - '10.57.14.57:9145'

- job_name: 'aerospike-auto-discovered-targets'
  file_sd_configs:
  - files:
    - '/etc/prometheus/targets.json' #This path is relative
```

### Final Result - Prometheus

![Aerospike%20Monitoring%20Stack%20Project%20by%20Srivatsa%20RV,%2042e143f0fbab47e39ecf45b270eef884/Untitled.png](Aerospike%20Monitoring%20Stack%20Project%20by%20Srivatsa%20RV,%2042e143f0fbab47e39ecf45b270eef884/Untitled.png)

# 2- Installing Aerospike Prometheus Exporter

```
**INSTALLATION GUIDE

>** Paste the commands in order (AS-IS fashion).   **OR
>** You can chose to run this by copying into a shell script, in that case,

**Script to install prometheus exporter**
> **nano aspromexporter_installer.sh
> chmod 755 aspromexporter_installer.sh
> ./aspromexporter_installer.sh**

export https_proxy="http://tinyproxy:8888"
export http_proxy="http://tinyproxy:8888"
wget https://www.aerospike.com/download/monitoring/aerospike-prometheus-exporter/latest/artifact/deb -O aerospike-prometheus-exporter.tgz
tar -xvzf aerospike-prometheus-exporter.tgz
dpkg -i aerospike-prometheus-exporter-1.3.0-amd64.deb
systemctl start aerospike-prometheus-exporter
systemctl enable aerospike-prometheus-exporter
```

## 2.1 Configure Exporter by Adding Respective Aerospike Node ID

```yaml
...........
....
..

[Aerospike]
db_host="10.57.12.185"
db_port=4333

# TLS certificates.
# Supports below formats,
# 1. Certificate file path                                      - "file:<file-path>"
# 2. Environment variable containing base64 encoded certificate - "env-b64:<environment-variable-that-contains-base64-encoded-certificate>"
# 3. Base64 encoded certificate                                 - "b64:<base64-encoded-certificate>"
# Applicable to 'root_ca', 'cert_file' and 'key_file' configurations.

# root certificate file
root_ca="/etc/aerospike/ssl/phonepeaerospike/cacert.pem"

# certificate file
cert_file="/etc/aerospike/ssl/phonepeaerospike/cert.pem"

# key file
key_file="/etc/aerospike/ssl/phonepeaerospike/key.pem"

# Passphrase for encrypted key_file. Supports below formats,
# 1. Passphrase directly                                                      - "<passphrase>"
# 2. Passphrase via file                                                      - "file:<file-that-contains-passphrase>"
# 3. Passphrase via environment variable                                      - "env:<environment-variable-that-holds-passphrase>"
# 4. Passphrase via environment variable containing base64 encoded passphrase - "env-b64:<environment-variable-that-contains-base64-encoded-passphrase>"
# 5. Passphrase in base64 encoded form                                        - "b64:<base64-encoded-passphrase>"
key_file_passphrase=""

# node TLS name for authentication
node_tls_name="phonepeaerospike"

# Aerospike cluster security credentials.
# Supports below formats,
# 1. Credential directly                                                      - "<credential>"
# 2. Credential via file                                                      - "file:<file-that-contains-credential>"
# 3. Credential via environment variable                                      - "env:<environment-variable-that-contains-credential>"
# 4. Credential via environment variable containing base64 encoded credential - "env-b64:<environment-variable-that-contains-base64-encoded-credential>"
# 5. Credential in base64 encoded form                                        - "b64:<base64-encoded-credential>"
# Applicable to 'user' and 'password' configurations.

# database user
user="admin"

# database password
password="admin"

# authentication mode: internal (for server), external (LDAP, etc.)
auth_mode="internal"

...
........
...........

```

## [**full config file here**](https://github.com/aerospike/aerospike-prometheus-exporter)

```yaml
NOTE: Exporter, if to be setup with TLS Enabled Aerospike Clusters, make sure you 
have certificates with **subjectAltName** in them, else the old and current style of 
certs in use will be useful. Setup your own CA , and generate new set of certs. 

2 Copies will have to be placed, one at aerospike itself during the time of 
configuration (SALT might do it) and the other copy (path of certs basically) will
sit in the exporter configuration file **ape.toml**
```

## 2.2 Starting Exporter Service

```
systemctl start aerospike-prometheus-exporter.service
systemctl enable aerospike-prometheus-exporter.service
systemctl status aerospike-prometheus-exporter.service

**Reference given for status of Exporter Service once it is started

aerospike-prometheus-exporter.service - Aerospike Prometheus Exporter Service
     Loaded: loaded (/lib/systemd/system/aerospike-prometheus-exporter.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2021-04-29 12:25:07 IST; 5 days ago
       Docs: https://github.com/aerospike/aerospike-prometheus-exporter
   Main PID: 169910 (aerospike-prome)
      Tasks: 4 (limit: 2344)
     Memory: 6.8M
     CGroup: /system.slice/aerospike-prometheus-exporter.service
             └─169910 /usr/bin/aerospike-prometheus-exporter --config /etc/aerospike-prometheus-exporter/ape.toml

Apr 29 12:25:07 stg-aerospikesrivatsa001 systemd[1]: Started Aerospike Prometheus Exporter Service.
Apr 29 12:25:07 stg-aerospikesrivatsa001 aerospike-prometheus-exporter[169910]: time="2021-04-29T12:25:07+05:30" level=info msg="Welcome to Aerospike Prometheus Exporter v1.2.0"
Apr 29 12:25:07 stg-aerospikesrivatsa001 aerospike-prometheus-exporter[169910]: time="2021-04-29T12:25:07+05:30" level=info msg="Loading configuration file /etc/aerospike-prometheus-exporter/ape.toml"
Apr 29 12:25:07 stg-aerospikesrivatsa001 aerospike-prometheus-exporter[169910]: time="2021-04-29T12:25:07+05:30" level=info msg="Listening for Prometheus on: :9145"
															---------- X ----------         

NOTE: Last line of status must be "Listening for Prometheus on: :9145"** 

```

### We are done.
Exporter is now live, and is exposing the metrics of the <IP> at <IP>:9145

![Aerospike%20Monitoring%20Stack%20Project%20by%20Srivatsa%20RV,%2042e143f0fbab47e39ecf45b270eef884/Untitled%201.png](Aerospike%20Monitoring%20Stack%20Project%20by%20Srivatsa%20RV,%2042e143f0fbab47e39ecf45b270eef884/Untitled%201.png)

# 3-Installing Grafana  and Setting Service

```
**CURRENT VERSION RUNNING - 7.5.7**

****export https_proxy="http://tinyproxy:8888"
export http_proxy="http://tinyproxy:8888"
****sudo apt-get install -y adduser libfontconfig1
wget https://dl.grafana.com/oss/release/grafana_7.5.7_amd64.deb
sudo dpkg -i grafana_7.5.7_amd64.deb
mkdir -p /var/lib/grafana/dashboards/
mkdir -p /etc/grafana/provisioning/dashboards/
mkdir -p /etc/grafana/provisioning/datasources/

[**refer more at: Grafana Installation Guide](https://grafana.com/grafana/download?platform=linux)**

**START SERVICE AND ENABLE PERSISTENT BOOT STARTUP**
sudo systemctl daemon-reload
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
sudo systemctl status grafana-server
```

```
**UNINSTALLING GRAFANA**

sudo apt-get remove --auto-remove grafana
sudo apt-get purge --auto-remove grafana
sudo apt-get autoclean
sudo apt-get autoremove

chmod -R a+w /var/lib/grafana
```

## 3.1 Provisioning Dashboards:

There are 2 ways to do it, 

1. Use the  /etc/grafana/provisioning/dashboards and /datasources path and configure it as code. 
2. Is just create the /dashboards and /datasources directory and then head over to Grafana Web UI and then drop your JSON files required. 

```yaml
**NOTE: The target names in Prometheus, eg "aerospike" has to be referenced correctly
in the JSON and grafana variables, ie keep the target names as it is, if you want
meta tags for tracking, use labels.** 
```