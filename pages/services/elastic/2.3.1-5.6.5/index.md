---
layout: layout.pug
navigationTitle: Elastic 2.3.1-5.6.5
excerpt:
title: Elastic 2.3.1-5.6.5
menuWeight: -1
model: /services/elastic/data.yml
render: mustache
---

<!-- Imported from https://github.com/mesosphere/dcos-commons.git:sdk-0.40 -->

DC/OS Elastic Service is an automated service that makes it easy to deploy and manage Elastic on Mesosphere DC/OS, eliminating nearly all of the complexity traditionally associated with managing an Elasticsearch cluster. Elasticsearch is a distributed, multitenant-capable, full-text search engine with an HTTP web interface and schema-free JSON documents. Elasticsearch clusters are highly available, fault tolerant, and durable. For more information on Elasticsearch, visit the [Elastic](https://www.elastic.co/) site. Multiple Elasticsearch clusters can be installed on DC/OS and managed independently, so you can offer Elasticsearch as a managed service to your organization with or without the commercial X-Pack capabilities.

# Benefits

DC/OS Elastic offers the following benefits of a semi-managed service:

*   Easy installation
*   Elastic scaling of nodes
*   Replication for high availability
*   Elasticsearch cluster and node monitoring

# Features

DC/OS Elastic provides the following features:

*   Single-command installation for rapid provisioning
*   Multiple clusters for multiple tenancy with DC/OS
*   High availability runtime configuration and software updates
*   Storage volumes for enhanced data durability, known as Mesos Dynamic Reservations and Persistent Volumes
*   Automatic reporting of Elasticsearch metrics to DC/OS statsd collector