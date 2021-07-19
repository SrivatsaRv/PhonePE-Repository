# Salt Installation for Grafana

## Directory Structure in Gitlab

```python
The structure required is as follows, in Gitlab,

**salt-config/aerospike/**
- install_grafana.sh 
**salt-config/aerospike/grafana** 
  - install.sls 
  - grafana_datasource_all.yaml
  - grafana_dashboard_all.yaml
  - dashoards_json.zip

export https_proxy="http://tinyproxy:8888"
export http_proxy="http://tinyproxy:8888"
salt-call state.sls aerospike.grafana.install -l debug test=y
```

```
CLEANUP SCRIPT FOR GRAFANA 

rm -r /etc/grafana
rm -r /var/lib/grafana
rm -r /var/log/grafana
rm -r /usr/share/b
```

```
**1- /etc/grafana/provisoning/dashboards/all.yaml**

apiVersion: 1
providers:
  
  - name: 'default'
    # orgId: 1
    folder: 'Aerospike'
    folderUid: 'aerospike1'
    type: file
    disableDeletion: false
    editable: true
    
    updateIntervalSeconds: 10
    options:
    path: /var/lib/grafana/dashboards

  - name: 'Node Overview'
    # orgId: 1
    folder: 'Aerospike'
    folderUid: 'aerospike1'
    type: file
    disableDeletion: false
    editable: true
    allowUiUpdates: true
    updateIntervalSeconds: 10
    options:
    path: /var/lib/grafana/dashboards/node.json
    
  - name: 'Namespace Overview'
    # orgId: 1
    folder: 'Aerospike'
    folderUid: 'aerospike1'
    type: file
    disableDeletion: false
    editable: true
    allowUiUpdates: true
    updateIntervalSeconds: 10
    options:
    path: /var/lib/grafana/dashboards/namespace.json
    
  - name: 'Latency Overview'
    # orgId: 1
    folder: 'Aerospike'
    folderUid: 'aerospike1'
    type: file
    disableDeletion: false
    editable: true
    allowUiUpdates: true
    updateIntervalSeconds: 10
    options:
    path: /var/lib/grafana/dashboards/latency.json
    
  - name: 'Alerts Overview'
    # orgId: 1
    folder: 'Aerospike'
    folderUid: 'aerospike1'
    type: file
    disableDeletion: false
    editable: true
    allowUiUpdates: true
    updateIntervalSeconds: 10
    options:
    path: /var/lib/grafana/dashboards/alerts.json
    
    
  - name: 'XDR Overview'
    # orgId: 1
    folder: 'Aerospike'
    folderUid: 'aerospike1'
    type: file
    disableDeletion: false
    editable: true
    allowUiUpdates: true
    updateIntervalSeconds: 10
    options:
    path: /var/lib/grafana/dashboards/xdr.json
```