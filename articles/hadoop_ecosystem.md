# The Hadoop ecosystem
*A beginners guide to the Hadoop ecosystem*

<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/headers/header_hadoop_ecosystem.png" align="middle"/>

## Table of contents

- [The creation of Hadoop](#the-creation-of-hadoop)
- [What is Hadoop?](#what-is-hadoop)
- [The Hadoop ecosystem](#the-hadoop-ecosystem)
- [Hadoop use cases](#hadoop-use-cases)

## The creation of Hadoop

Traditionally, computation has been processor-bound with relatively small amounts of data with lots of complex processing. The early solution  focused on big computers with faster processor with a lot of memory but even this kind of approach couldn't keep up. This approach is called **traditional large-scale computation**; an alternative that was born in the last decades is the **distributed systems**. It consists in multiple computers used in parallel for a single job.

There were some challenges with distributed systems, especially related to programming complexity (*keeping data and processes in sync*), finite bandwidth and partial failures. Another problem with distributed systems is the **data bottleneck**: traditionally data is stored in a central location, with data copied to processors at runtime, which is fine for limited amounts of data but it's not good for modern systems with terabytes or petabytes of data.

This **data bottleneck** problem was faced by **Google** in the late 1990s. The goal was to indexing the entire web but this required massive amounts of storage, so a new approach was necessary to process such large amounts of data. The solution found produced a paper called [**GFS**](https://en.wikipedia.org/wiki/Google_File_System) (*Google File System*), released in 2003, followed by another paper called [**MapReduce**](https://en.wikipedia.org/wiki/MapReduce), released in 2004.

Nowadays more data is coming, through IoT, sensor data and streaming; more data means bigger question and better answers. Hadoop can easily scales to store and handle all of your data, especially because it is **cost-effective**: typically providing a significant cost-per-terabyte saving over traditional legacy system. And the most important thing: "*Hadoop integrates with the existing datacenter components*", letting you to exploit data you have been throwing away.

## What is Hadoop?

Talking at very high level about **Hadoop**, it can be considered as "*a software framework for storing, processing and analyzing big data*". It is:

* __distributed__: an Hadoop cluster is made up of several machines all working together
* __scalable__: if you need to store more data or if you need to process data faster, you can add more machines to the cluster; adding nodes increase capacity and computational power proportionally
* __fault-tolerant__: as you add more machines to the cluster, the probability that one of these machine is going to fail increases, but Hadoop is designed with this assumption; the system continues to function and the master re-assigns work to a different node. Data replication means there is no loss of data and nodes which recover rejoin the cluster automatically
* __open source__: it is overseen by the Apache Software Foundation with over 90 committers to core Hadoop (*coming from Cloudera, Facebook, Yhaoo!, Google, etc.*), hundreds of committers on other Hadoop-related projects and thousand of contributors writing features and fixing bugs

Hadoop stores massive amounts of data in a very **resilient way** and handles all the low-level distributed system details and enables developers to focus only on the business problems.

The **Core Hadoop** is composed by a **file system** and a **processing framework**:

* [__HDFS__](https://en.wikipedia.org/wiki/Apache_Hadoop#HDFS) (*Hadoop Distributed File System*): any type of file can be stored in HDFS, data is split into chunks and replicated as it is written; provides resiliency and high availability and is handled automatically by Hadoop
* __YARN__ (*Yet Another Resource Negotiator*): manages the processing resources of the Hadoop cluster, schedules jobs and runs processing frameworks (*like Spark*)
* [__MapReduce__](https://en.wikipedia.org/wiki/MapReduce): a distributed processing framework

## The Hadoop ecosystem

Hadoop has a large and growing ecosystem that extend basic capabilities. These are some of the most common projects of Hadoop found in a **Cloudera** distribution:

<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/images/hadoop_ecosystem.png" align="middle" />

Tools built around Hadoop can be configured/extended to handle many different tasks, such as the following ones.

Task | Tools
--- | ---
ETL | s
BI environment | s
General data storage | s
Predictive analysis | s
Statistical analysis | s
Machine learning | s

Hadoop works also with a lot of commercial products as well, for example in the BI/Business analytics sector, or the ETL part of data processing or databases. Some of them are:

IMG=HADOOP COMMERCIAL PRODUCTS

Hadoop is nowadays used in many industries (*organization in financial world, healthcare, manufacturing, non-profit, etc.*), some organization that use Hadoop are:

IMG=ORGANIZATION WHICH USES HADOOP

Some of the most important projects of the Hadoop ecosystem are:

Project | What does it do?
--- | ---
[__Spark__](http://spark.apache.org) | In-memory and streaming processing framework (*faster and more efficient than MapReduce*)
[__HBase__](http://hbase.apache.org) | NoSQL database built on HDFS
[__Hive__](https://hive.apache.org) | SQL processing engine designed for batch workloads
[__Impala__](http://impala.apache.org) | SQL query engine designed for BI workloads
[__Parquet__](https://parquet.apache.org) | Very efficient columnar data storage format
[__Sqoop__](http://sqoop.apache.org) | Data movement to/from RDBMS
[__Flume__](https://flume.apache.org) | Streaming data ingestion
[__Kafka__](http://kafka.apache.org) | Streaming data ingestion
[__Solr__](https://lucene.apache.org/solr/) | Powerful text search functionality
[__Hue__](http://gethue.com) | Web-based user interface for Hadoop
[__Sentry__](http://sentry.apache.org) | Authorization tool, providing security for Hadoop

The Hadoop ecosystem is growing every month and new features are frequently added such as processing frameworks, support for new file types and performance enhancements.

## Hadoop use cases

#### Financial Services
* __JP Morgan Chase__: fraud detection, anti-money laundering and self-service applications. Now stores and processes data that was previously discarded
* __MasterCard__: first Certified Payment Card Industry (*PCI*) data security standards Hadoop solution, in conjunction with Cloudera Manager and Cloudera Navigator
* __Western Union__: cross border, consumer-to-consumer money transfers and bill payments

#### Insurance
* __Allstate__: fraud detection, predictive analytics and reporting. Leveraging Hadoop to compliment and not replace the existing system
* __RelayHealth__: processes healthcare provider-to-payer interaction, creating analytics platforms on Hadoop
* __Markerstudy__: fraud detection and prevention at point-of-sale, uses Hadoop to provide the most appropriate price and product to every customer

#### Telecommunications
* __Telkomsel__: data discovery and analytics, ETL operations from their data
* __Nokia__: improve user experiences with phones and location services, model traffic using advanced analytics and 3D mapping
* __SFR__: 360-degree customer view spanning devices and data sources, offload data ingest, processing and exploration from the DWH

#### Healthcare
* __Children's Healthcare of Atlanta__: analysis of sensor data, started with a very small cluster (6 nodes, built from scavenged PCs)
