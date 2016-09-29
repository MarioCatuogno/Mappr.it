# Hadoop ecosystem
*A beginners guide to the Hadoop ecosystem*

IMG=HADOOP (Mappr.it style)
## Table of contents

- [The creation of Hadoop](#the-creation-of-hadoop)
- [What is Hadoop?](#what-is-hadoop?)
- [The Hadoop ecosystem](#the-hadoop-ecosystem)

## The creation of Hadoop

Traditionally, computation has been processor-bound with relatively small amounts of data with lots of complex processing. The early solution  focused on big computers with faster processor with a lot of memory but even this kind of approach couldn't keep up. This approach is called **traditional large-scale computation**; an alternative that was born in the last decades is the **distributed systems**. It consists in multiple computers used in parallel for a single job.

There were some challenges with distributed systems, especially related to programming complexity (*keeping data and processes in sync*), finite bandwidth and partial failures. Another problem with distributed systems is the **data bottleneck**: traditionally data is stored in a central location, with data copied to processors at runtime, which is fine for limited amounts of data but it's not good for modern systems with terabytes or petabytes of data.

This **data bottleneck** problem was faced by **Google** in the late 1990s. The goal was to indexing the entire web but this required massive amounts of storage, so a new approach was necessary to process such large amounts of data. The solution found produced a paper called [**GFS**](https://en.wikipedia.org/wiki/Google_File_System) (*Google File System*), released in 2003, followed by another paper called [**MapReduce**](https://en.wikipedia.org/wiki/MapReduce), released in 2004.

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

IMG=HADOOP ECOSYSTEM

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

## 2. Cloudera

**Cloudera** is the leader in the Apache Hadoop-based software and services. It was founded in 2008 by leading experts on Hadoop and nowadays has global operations spanning over 20 countries. Cloudera provides support, consulting, training and certification for Hadoop users.

One of the main products of Cloudera is **CDH** (Cloudera's Distribution, including Apache Hadoop). It is "*a 100% open source and free to download, enterprise-ready distribution of Hadoop and related projects*".

IMG=CDH

The key projects integrated in this distribution are:

Component | Description
--- | ---
[__Apache Hadoop (Core)__](http://www.cloudera.com/products/apache-hadoop/hdfs-mapreduce-yarn.html) | reliable, scalable, distributed storage and computing
[__Apache Accumulo__](http://www.cloudera.com/products/apache-hadoop/apache-accumulo.html) | a secure, distributed data store to serve performance-intensive Big Data applications
[__Apache Flume__](http://www.cloudera.com/products/apache-hadoop/apache-flume.html) | for collecting and aggregating log and event data and real-time streaming into Hadoop
[__Apache HBase__](http://www.cloudera.com/products/apache-hadoop/apache-hbase.html) | scalable record and table storage, with real-time read/write access
[__Apache Hive__](http://www.cloudera.com/products/apache-hadoop/apache-hive.html) | familiar SQL framework with metadata repository for batch processing of Hadoop data
[__HUE__](http://www.cloudera.com/products/apache-hadoop/hue.html) | the extensible web GUI that makes Hadoop users more productive
[__Apache Impala__](http://www.cloudera.com/products/apache-hadoop/impala.html) | the analytic database native to Hadoop for low-latency queries under multi-user workloads
[__Apache Kafka__](http://www.cloudera.com/products/apache-hadoop/apache-kafka.html) | the backbone for distributed real-time processing of Hadoop data
[__Apache Pig__](http://www.cloudera.com/products/apache-hadoop/apache-pig.html) | high-level data flow language for processing data stored in Hadoop
[__Apache Sentry__](http://www.cloudera.com/products/apache-hadoop/apache-sentry.html) | fine-grained, role-based authorization for Impala and Hive
[__Cloudera Search__](http://www.cloudera.com/products/apache-hadoop/apache-solr.html) | powered by Solr to make Hadoop accessible to everyone via integrated full-text search
[__Apache Spark__](http://www.cloudera.com/products/apache-hadoop/apache-spark.html) | the open standard for in-memory batch and real-time processing for advanced analytics
[__Apache Sqoop__](http://www.cloudera.com/products/apache-hadoop/apache-sqoop.html) | data transport engine for integrating Hadoop with relational databases

To manage the Hadoop cluster there's the software [__Cloudera Manager__](http://www.cloudera.com/products/cloudera-manager.html). It has an automated wizard to quickly deploy cluster, no matter what the scale or the deployment environment; it ensure consistency as you move from testing to production, or across environments with portable cluster configuration templates. It is part of the **Cloudera Express** and **Cloudera Enterprise**. The differences between the two products are the following:

Feature | Cloudera Express | Cloudera Enterprise
--- | --- | ---
Number of hosts supported | Unlimited | Unlimited
High availability support | :heavy_check_mark: | :heavy_check_mark:
HBase co-processor support | :heavy_check_mark: | :heavy_check_mark:
Configuration history and rollbacks | :heavy_multiplication_x: | :heavy_check_mark:
Rolling updates | :heavy_multiplication_x: | :heavy_check_mark:
Automated backup and disaster recovery | :heavy_multiplication_x: | :heavy_check_mark:
Event auditing | :heavy_multiplication_x: | :heavy_check_mark:
Metadata tagging capabilities | :heavy_multiplication_x: | :heavy_check_mark:
Data lineage exploration | :heavy_multiplication_x: | :heavy_check_mark:
Data encryption with KMS | :heavy_multiplication_x: | :heavy_check_mark:
File browsing, searching and disk quota management | :heavy_multiplication_x: | :heavy_check_mark:
HBase, MapReduce, Impala and YARN usage reports | :heavy_multiplication_x: | :heavy_check_mark:




  ``` r
  install.packages("dplyr")
  install.packages("ggplot2")
  ```
