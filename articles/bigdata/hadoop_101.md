# Hadoop 101 : Data Modeling
*An introduction to Hadoop Data Modeling*

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/headers/header_hadoop_101.png" />
</p>

## Table of contents

- [1. Introduction](#introduction)
    - [1.1 Data storage format](#data-storage-format)
    - [1.2 Multitenancy](#multitenancy)
    - [1.3 Schema design](#schema-design)
    - [1.4 Metadata management](#metadata-management)
- [2. Data storage options](#data-storage-options)
    - [2.1 File format](#file-format)
    - [2.2 Compression](#compression)
    - [2.3 Data storage system](#data-storage-system)

## Introduction

At its core, Hadoop is a **distributed data store** that provides a platform for implementing powerful parallel processing frameworks. It is possible to store any type of data as is, without placing any constraints on how that data is processed.

A common term one hears in the context of Hadoop is **Schema-on-Read**. This simply refers to the fact that raw, unprocessed data can be loaded into Hadoop, with the structure imposed at processing time based on the requirements of the processing application.
This is different from **Schema-on-Write**, which is generally used with traditional data management systems. Such systems require the schema of the data store to be defined before the data can be loaded.

Relational databases and data warehouses are often a good fit for well-understood and frequently accessed queries and reports on high-value data. Increasingly, though, Hadoop is taking on many of these workloads, particularly for queries that need to operate on volumes of data that are not economically or technically practical to process with traditional systems.

#### Data storage format

There are a number of file formats and compression formats supported on Hadoop. Each has particular strengths that make it better suited to specific applications. Additionally, although Hadoop provides the **Hadoop Distributed File System** (*HDFS*) for storing data, there are several commonly used systems implemented on top of HDFS, such as **HBase** for additional data access functionality and **Hive** for additional data management functionality.

#### Multitenancy

It’s common for clusters to host multiple users, groups, and application types.

#### Schema design

Despite the schema-less nature of Hadoop, there are still important considerations to take into account around the structure of data stored in Hadoop. This includes **directory structures** for data loaded into HDFS as well as the **output** of data processing and analysis. This also includes the schemas of objects stored in systems such as HBase and Hive.

#### Metadata management

As with any data management system, metadata related to the stored data is often as important as the data itself. Understanding and making decisions related to metadata management are critical.

## Data storage options

There is no such thing as a standard data storage format in Hadoop. Just as with a standard filesystem, Hadoop allows for storage of data in any format, whether it’s *text*, *binary*, *images*, or something else. Hadoop also provides built-in support for a number of *formats optimized for Hadoop* storage and processing.

#### File format

There are multiple formats that are suitable for data stored in Hadoop. These include plain text or Hadoop-specific formats such as **SequenceFile**. There are also more complex but more functionally rich options, such as **Avro** and **Parquet**. It’s possible to create your own custom file format in Hadoop, as well.

#### Compression

#### Data storage system
