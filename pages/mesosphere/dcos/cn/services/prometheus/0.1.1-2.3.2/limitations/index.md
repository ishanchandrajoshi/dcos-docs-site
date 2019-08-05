---
layout: layout.pug
navigationTitle: 限制
title: 限制
menuWeight: 110
excerpt: 了解配置限制
featureMaturity:
enterprise: false
---

## Out-of-band 配置

不支持 Out-of-band 配置修改。服务的核心责任是使用指定配置部署和维护服务。为此，服务假定其拥有任务配置所有权。如果最终用户通过 out-of-band 配置操作对个别任务进行修改，该服务将在以后覆盖这些修改。例如：

- 如果任务崩溃，系统将使用调度程序已知的配置重新启动任务，而非使用修改后的 out-of-band 配置重新启动。
- 如果初始化了配置更新，所有 out-of-band 修改将在滚动更新期间被覆盖。

Prometheus 还不提供持久的长期存储、异常检测、自动水平扩展和用户管理。
Prometheus 不是仪表板解决方案；它具有一个简单的 UI，用于实验 PromQL 查询，但依靠 Grafana 完成仪表板功能。

## 缩减

为防止意外丢失数据，该服务不支持减少 Pod 数量。

## 磁盘更改

为防止重新分配时意外丢失数据，服务不支持在初次部署后更改卷要求。

## 最大努力安装

如果您的集群没有足够的资源按要求部署服务，初始部署将不会完成，直到那些资源可用或按照修正的资源要求重新安装服务为止。同样，如果集群没有完成扩展所需的可用资源，则初始部署后的扩展将不会完成。

## 虚拟网络

在虚拟网络上部署服务时，如果没有完全地重新安装，服务可能无法切换到主机网络。同样，尝试从主机切换到虚拟网络也是如此。

## 任务环境变量

每个服务任务都有一些用于配置任务的环境变量。这些环境变量由服务调度程序设置。尽管可能在即席脚本中使用这些环境变量（例如，通过 `dcos task exec`)，给定环境变量的名称可能会随服务的不同版本而变化，且不应被视为服务的公用 API。