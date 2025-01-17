# Week - 7

This week you will be automating the setup for the datastore you worked on last week.

You need to automate the setup of the following datastore using ansible, there should be no manual process in the setup.

**Elasticsearch:**

- Install and configure 3 nodes elasticsearch cluster version 7.8.0
- The ES cluster should be on TLS and have a username/password
- Data should be persisted on disk
- JVM/GC settings should be configured

**RMQ:**

- Install and configure 3 node RMQ cluster version 3.7.9
- The RMQ cluster should be on TLS and have a username/password
- Data should be persisted on disk
- Create a vhost and a user with read-write permissions to the vhost
- Create a user with monitoring access
- Add rabbitmq_management plugin

**Aerospike:**

- Install and configure 3 node Aerospike cluster version 4.8.0.6
- Data should be persisted on disk
- Create a namespace Order
