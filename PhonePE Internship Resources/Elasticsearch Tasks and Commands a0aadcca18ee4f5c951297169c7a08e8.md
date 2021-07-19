# Elasticsearch Tasks and Commands

- [x]  Install and configure 1 node elasticsearch cluster version 7.8.0
- [ ]  The ES cluster should be on TLS and have a username/password
- [ ]  Data should be persisted on disk
- [x]  Check the various jvm options and come up with the appropriate heap and GC settings
- [x]  Add 2 more nodes to the cluster without restarting elasticsearch service on first one
- [x]  Create 3 indices (books details, author details, publishing company details) on the cluster, set the number of shards to be 3 for each index
- [x]  Insert at least 10 documents per index.
- [ ]  The publishing company documents should be parent for the book details documents.
- [ ]  Take backup of all the indices
- [ ]  Delete all the indices
- [ ]  restore them

### 1- Cluster Level Monitoring Commands

```
1- Cluster Health
> **curl -XGET 'localhost:9200/_cluster/health?pretty'** 

2- Cluster State 
> **curl -XGET 'localhost:9200/_cluster/state?pretty'**

3- Full Cluster Monitoring + Stats
> **curl -XGET 'localhost:9200/_cluster/stats?human&pretty'**

4- Individual Node Details
> **curl -XGET 'http://localhost:9200'**

5- Master Node Information
> **curl -XGET 'http://localhost:9200/_cluster/state/master_node?pretty'**
```

### Data Handling - Index, Documents etc

```
1- List all the indices in the node 
> **curl --silent 'http://127.0.0.1:9200/_cat/indices' | cut -d\  -f3**

2- List the contents of a particular index
> 
```