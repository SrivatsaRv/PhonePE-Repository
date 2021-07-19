# Aerospike Prometheus Exporter

### 1- Installation - From Script and Terminal Options

```

```

### 2- Configuration and Running the Aerospike Prometheus Exporter

```
**CONFIGURATION GUIDE**

**>** Head to **/etc/aerospike-prometheus-exporter,
>** Change this in **ape.toml.** (This should be one and only file in the 
  /etc/aerospike-prometheus-exporter directory))

Edit /etc/aerospike-prometheus-exporter/ape.toml to add db_host(default=localhost) 
and db_port (default 3000) to point to an Aerospike server IP and port.
**[Aerospike]
db_host="localhost"
db_port=3000

NOTE = This is explicit list to display selected metrics, leaving the config file 
       as it is means that Prometheus SCRAPES ALL THE METRICS AS IS.** 

# Namespace metrics allowlist
namespace_metrics_allowlist=[
"client_read_[a-z]*",
"stop_writes",
"storage-engine.file.defrag_q",
"client_write_success",
"memory_*_bytes",
"objects",
"*_available_pct"
]

# Set metrics allowlist
set_metrics_allowlist=[
"objects",
"tombstones"
]

# Node metrics allowlist
node_metrics_allowlist=[
"uptime",
"cluster_size",
"batch_index_*",
"xdr_ship_*"
]

# XDR metrics allowlist (only for Aerospike versions 5.0 and above)
xdr_metrics_allowlist=[
"success",
"latency_ms",
"throughput",
"lap_us"
]

# Metrics Blocklist - If specified, these metrics will be NOT be scraped.

# Namespace metrics blocklist
namespace_metrics_blocklist=[
"memory_used_sindex_bytes",
"client_read_success"
]

# Set metrics blocklist
# set_metrics_blocklist=[]

# Node metrics blocklist
node_metrics_blocklist=[
"batch_index_*_buffers"
]

# XDR metrics blocklist (only for Aerospike versions 5.0 and above)
# xdr_metrics_blocklist=[]
```

### 3 - Starting the Prometheus Exporter

```
**NOTE: This /metrics endpoint is by default exposed ONCE the Aerospike-Prometheus 
      Exporter is setup on the respective machines.** 
```