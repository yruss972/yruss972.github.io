---
id: 337
title: Couchbase is Simply Awesome
date: 2015-01-16T09:19:52-07:00
author: Yonah Russ
layout: post
guid: http://www.yonahruss.com/?p=337
permalink: /architecture/couchbase-is-simply-awesome.html
categories:
  - Architecture
  - Availability
  - Performance
tags:
  - Couchbase
  - Couchbase Server
  - Cross-platform software
  - Data management
  - Distributed computing architecture
  - JSON
  - Memcached
  - N1QL
  - NoSQL
  - Query language
  - Shard
  - Structured storage
  - Technology_Internet
---
<img class="aligncenter size-full wp-image-338" src="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/couchbase.jpg" alt="couchbase" width="698" height="400" srcset="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/couchbase.jpg 698w, http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/couchbase-300x172.jpg 300w" sizes="(max-width: 698px) 100vw, 698px" />

Here are five things that make Couchbase a go-to service in any architecture.

## Couchbase is simple to setup.

Keep It Simple. It&#8217;s one of the axioms of system administration. Couchbase, though complicated under the hood, makes it very simple to setup even complicated clusters spanning multiple data centers.

Every node comes with a very user friendly web interface including the ability to monitor performance across all the nodes in the same machine&#8217;s cluster.

Adding nodes to a cluster is as simple as plugging in the address of the new node after which, all the data in the cluster is automatically rebalanced between the nodes. The same is true when removing nodes.

Couchbase is built to never require downtime which makes it a pleasure to work with.

If you are into automation a la chef, etc., Couchbase supports configuration via REST api. There are cookbooks available. I&#8217;m not sure about other configuration management tools but they probably have the relevant code bits as well.

## Couchbase replaces Memcached

Even if you have no need for a more advanced NoSQL solution, there is a good chance you are using Memcached, Couchbase is the original Memcached on steroids.

Unlike traditional Memcached, Couchbase supports clustering, replication, and persistence of data. Using the Moxi Memcached proxy that comes with Couchbase, your apps can talk Memcached protocol to a cluster of Couchbase servers and get the benefits of automatic sharding and failover. If you want, Couchbase can also persist the Memcached data to disk turning your Memcached into a persistent, highly available key value store.

## Couchbase is also a schema-less NoSQL DB

Aside from support for simple Memcached key/value storage, Couchbase is a highly available, easy to scale, JSON based DB with auto-sharding and built in map reduce.

Traditionally, Couchbase uses a system called views to perform complicated queries on the JSON data but they are also working on a new query language called N1QL which brings tremendous additional ad hoc query capabilities.

Couchbase also supports connectivity to Elastic Search, Hadoop, and Talend.

## Couchbase is all about global scale out

Adding and removing nodes is simple and every node in a Couchbase cluster is read and write capable all the time. If you need more performance, you just add more nodes.

When one data center isn&#8217;t enough, Couchbase has a feature called cross data center replication (XDCR), letting you easily setup unidirectional or bidirectional replication between multiple Couchbase clusters over WAN. You can even setup full mesh replication though it isn&#8217;t clearly described in their documentation.

Unlike MongoDB, which can only have one master, Couchbase using XDCR allows apps in any data center to write to their local Couchbase cluster and that data will be replicated to all the other data centers.

I recently setup a system using five Couchbase clusters across the US and Europe, all connected in a full mesh with each other. In my experience, data written in any of the data centers updated across the globe in 1-2 seconds max.

## Couchbase is only getting better

Having used Couchbase built from source (read community support only) since version 2.1 (Couchbase is now at 3.0.2), I can say that it is only getting better. They have made amazing progress with XDCR, added security functionality, and the N1QL language.

The Couchbase community is great. Checkout the IRC channel if you need help.