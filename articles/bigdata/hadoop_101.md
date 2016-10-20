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
- [3. Standard file format](#standard-file-format)
    - [3.1 Text data](#text-data)
    - [3.2 Structured text data](#structured-text-data)

## Introduction

At its core, Hadoop is a **distributed data store** that provides a platform for implementing powerful parallel processing frameworks. It is possible to store any type of data as is, without placing any constraints on how that data is processed.

A common term one hears in the context of Hadoop is **Schema-on-Read**. This simply refers to the fact that raw, unprocessed data can be loaded into Hadoop, with the structure imposed at processing time based on the requirements of the processing application.
This is different from **Schema-on-Write**, which is generally used with traditional data management systems. Such systems require the schema of the data store to be defined before the data can be loaded.

Relational databases and data warehouses are often a good fit for well-understood and frequently accessed queries and reports on high-value data. Increasingly, though, Hadoop is taking on many of these workloads, particularly for queries that need to operate on volumes of data that are not economically or technically practical to process with traditional systems.

#### Data storage format

There are a number of file formats and compression formats supported on Hadoop. Each has particular strengths that make it better suited to specific applications. Additionally, although Hadoop provides the **Hadoop Distributed File System** ([**HDFS**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/hadoop_102.md)) for storing data, there are several commonly used systems implemented on top of HDFS, such as [**HBase**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_hbase.md) for additional data access functionality and [**Hive**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_hive.md) for additional data management functionality.

#### Multitenancy

It’s common for clusters to host multiple users, groups, and application types.

#### Schema design

Despite the schema-less nature of Hadoop, there are still important considerations to take into account around the structure of data stored in Hadoop. This includes **directory structures** for data loaded into HDFS as well as the **output** of data processing and analysis. This also includes the schemas of objects stored in systems such as HBase and Hive.

#### Metadata management

As with any data management system, metadata related to the stored data is often as important as the data itself. Understanding and making decisions related to metadata management are critical.

## Data storage options

There is no such thing as a standard data storage format in Hadoop. Just as with a standard filesystem, Hadoop allows for storage of data in any format, whether it’s *text*, *binary*, *images*, or something else. Hadoop also provides built-in support for a number of *formats optimized for Hadoop* storage and processing.

#### File format

There are multiple formats that are suitable for data stored in Hadoop. These include plain text or Hadoop-specific formats such as **SequenceFile**. There are also more complex but more functionally rich options, such as [**Avro**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_avro.md) and [**Parquet**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_parquet.md). It’s possible to create your own custom file format in Hadoop, as well.

#### Compression

Compression codecs commonly used with Hadoop have different characteristics; for example, some codecs compress and uncompress faster but don’t compress as aggressively, while other codecs create smaller files but take longer to compress and uncompress, and not surprisingly require more CPU.

#### Data storage system

While all data in Hadoop rests in HDFS, there are decisions around what the underlying storage manager should be, for example, whether you should use HBase or HDFS directly to store the data. Additionally, tools such as Hive and [**Impala**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_impala.md) allow you to define additional structure around your data in Hadoop.

## Standard file format

In general, it’s preferable to use one of the Hadoop-specific container formats, but in many cases you’ll want to store source data in its raw form. As noted before, one of the most powerful features of Hadoop is the ability to store all of your data regardless of format.

#### Text data

A very common use of Hadoop is the storage and analysis of logs such as web logs and server logs. Such text data, of course, also comes in many other forms: **CSV** files, or unstructured data such as emails. Additionally, you’ll want to select a compression format for the files, since text files can very quickly consume considerable space on your Hadoop cluster.

There is an overhead of type conversion associated with storing data in text format. For example, storing 1234 in a text file and using it as an integer requires a **string-to-integer** conversion during reading, and vice versa during writing. It also takes up more space to store 1234 as text than as an integer.

Selection of compression format will be influenced by how the data will be used. For archival purposes you may choose the most compact compression available, but if the data will be used in processing jobs such as [**MapReduce**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/hadoop_103.md), you’ll likely want to select a **splittable format**. Splittable formats enable Hadoop to split files into chunks for processing, which is critical to efficient parallel processing.

Note also that in many, if not most cases, the use of a **container format** such as SequenceFiles or Avro will provide advantages that make it a preferred format for most file types, including text; among other things, these container formats provide functionality to support splittable compression.

#### Structured text data

A more specialized form of text files is structured formats such as **XML** and **JSON**. These types of formats can present special challenges with Hadoop since splitting XML and JSON files for processing is tricky, and Hadoop does not provide a built-in **InputFormat** for either. JSON presents even greater challenges than XML, since there are no tokens to mark the beginning or end of a record. In the case of these formats, you have a couple of options:

- **Use a container format such as Avro**: transforming the data into Avro can provide a compact and efficient way to store and process the data
- **Use a library designed for processing XML or JSON files**: Examples of this for XML include **XMLLoader** in the [**PiggyBank**](https://cwiki.apache.org/confluence/display/PIG/PiggyBank) library for Pig. For JSON, the [**Elephant Bird project**](https://github.com/twitter/elephant-bird) provides the **LzoJsonInputFormat**.

Excerpt From: Mark Grower. “Hadoop Application Architectures.” iBooks.
