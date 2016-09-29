# Hadoop Cloudera
*A beginners guide to the Cloudera Hadoop distribution*

IMG=HADOOP (Mappr.it style)
## Table of contents

- [What is Cloudera?](#what-is-cloudera-?)
- [Cloudera Manager](#cloudera-manager)


## 2. What is Cloudera?

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

## Cloudera Manager

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
