---
layout: layout.pug
navigationTitle:
excerpt:
title: Operations
menuWeight: 30
model: /mesosphere/dcos/services/elastic/data.yml
render: mustache
---

#include /mesosphere/dcos/services/include/operations.tmpl

## Back up and Restore

You interact with the Elasticsearch cluster directly to create snapshots and restore operations. Elasticsearch's built-in [snapshot and restore module](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html) allows you to create snapshots of individual indices or an entire cluster into a remote repository like a shared file system, S3, or HDFS.
