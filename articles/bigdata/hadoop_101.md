# Hadoop 101 : Data Modeling
*An introduction to Hadoop Data Modeling*

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/headers/header_hadoop_101.png" />
</p>

`NOTE: This article is an excerpt from Mark Grower, "Hadoop Application Architectures"`

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
    - [3.3 Binary data](#binary-data)
- [4. Hadoop file formats](#hadoop-file-formats)
    - [4.1 SequenceFile](#sequencefile)
- [5. Serialization formats](#serialization-formats)
    - [5.1 Thrift](#thrift)
    - [5.2 Protocol buffers](#protocol-buffers)
    - [5.3 Avro](#avro)
- [6. Columnar formats](#columnar formats)
    - [6.1 RCFile](#rcfile)
    - [6.2 ORC](#orc)
    - [6.3 Parquet](#parquet)
- [7. Failure behavior](#failure-behavior)
- [8. Types of compression](#types-of-compression)
    - [8.1 Snappy](#snappy)
    - [8.2 LZO](#lzo)
    - [8.3 Gzip](#gzip)
    - [8.4 bzip2](#bzip2)
    - [8.5 Compression recommendations](#compression-recommendations)
- [9. Useful readings](#useful-readings)



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
- **Use a library designed for processing XML or JSON files**: examples of this for XML include **XMLLoader** in the [**PiggyBank**](https://cwiki.apache.org/confluence/display/PIG/PiggyBank) library for [**Pig**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_pig.md). For JSON, the [**Elephant Bird project**](https://github.com/twitter/elephant-bird) provides the **LzoJsonInputFormat**

#### Binary data

Although text is typically the most common source data format stored in Hadoop, you can also use Hadoop to process binary files such as images. For most cases of storing and processing binary files in Hadoop, using a container format such as **SequenceFile** is preferred. If the splittable unit of binary data is larger than **64MB**, you may consider putting the data in its own file, without using a container format.

## Hadoop file formats

There are several Hadoop-specific file formats that were specifically created to work well with MapReduce. These Hadoop-specific file formats include file-based data structures such as sequence files, serialization formats like Avro, and columnar formats such as RCFile and Parquet. These file formats have differing strengths and weaknesses, but all share the following characteristics:

- **splittable compression**: these formats support common compression formats and are also splittable; note that the ability to split files can be a key consideration for storing data in Hadoop because it allows large files to be split for input to MapReduce and other types of jobs. The ability to split a file for processing by multiple tasks is of course a fundamental part of parallel processing, and is also key to leveraging Hadoop’s data locality feature
- **agnostic compression**: the file can be compressed with any compression codec, without readers having to know the codec. This is possible because the codec is stored in the header metadata of the file format

#### SequenceFile

The **SequenceFile** format is one of the most commonly used file-based formats in Hadoop, but other file-based formats are available, such as MapFiles, SetFiles, ArrayFiles, and BloomMapFiles. Because these formats were specifically designed to work with MapReduce, they offer a high level of integration for all forms of MapReduce jobs, including those run via Pig and Hive.

SequenceFiles store data as binary **key-value** pairs. There are three formats available for records stored within SequenceFiles:

- **uncompressed**: for the most part, uncompressed SequenceFiles don’t provide any advantages over their compressed alternatives, since they’re less efficient for input/output (I/O) and take up more space on disk than the same data in compressed form
- **record-compressed**: this format compresses each record as it’s added to the file
- **block-compressed**: this format waits until data reaches block size to compress, rather than as each record is added. Block compression provides better compression ratios compared to record-compressed SequenceFiles, and is generally the preferred compression option for SequenceFiles. A **block** in block compression refers to a group of records that are compressed together within a single **HDFS block**

Regardless of format, every SequenceFile uses a common **header format** containing basic **metadata** about the file, such as the *compression codec* used, *key and value class names*, *user-defined metadata*, and a randomly generated *sync marker*.

SequenceFiles are well supported within the Hadoop ecosystem, however their support outside of the ecosystem is limited. They are also only supported in **Java**. A common use case for SequenceFiles is as a **container** for smaller files. Storing a large number of small files in Hadoop can cause a couple of issues. One is excessive memory use for the **NameNode**, because metadata for each file stored in HDFS is held in memory. Another potential issue is in processing data in these files — many small files can lead to many processing tasks, causing excessive overhead in processing. Because Hadoop is optimized for large files, packing smaller files into a SequenceFile makes the storage and processing of these files much more efficient.

The following diagram shows an example of the file layout for a SequenceFile using block compression. An important thing to note s the inclusion of the sync marker before each block of data, which allows readers of the file to seek to block boundaries:

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/images/hadoop_sequencefile.png" />
</p>

## Serialization formats

**Serialization** refers to the process of turning data structures into byte streams either for storage or transmission over a network. Conversely, **deserialization** is the process of converting a byte stream back into data structures. Serialization is core to a distributed processing system such as Hadoop, since it allows data to be converted into a format that can be efficiently stored as well as transferred across a network connection.

The main serialization format utilized by Hadoop is **Writables**, which are compact and fast, but not easy to extend or use from languages other than Java.

There are, however, other serialization frameworks seeing increased use within the Hadoop ecosystem, including Thrift, Protocol Buffers, and Avro. Of these, [**Avro**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_avro.md) is the best suited, because it was specifically created to address limitations of Hadoop Writables.

#### Thrift

**Thrift** was developed at Facebook as a framework for implementing cross-language interfaces to services. Using Thrift allows us to implement a single interface that can be used with different languages to access different underlying systems. Although sometimes used for data serialization with Hadoop, Thrift has several drawbacks: it does not support internal compression of records, it’s not splittable, and it lacks native MapReduce support. Note that there are externally available libraries such as the Elephant Bird project to address these drawbacks, but Hadoop does not provide native support for Thrift as a data storage format.

#### Protocol buffers

The **Protocol Buffer** (*protobuf*) format was developed at Google to facilitate data exchange between services written in different languages. Like Thrift, Protocol Buffers do not support internal compression of records, are not splittable, and have no native MapReduce support; but the Elephant Bird project can be used to encode protobuf records, providing support for MapReduce, compression, and splittability.

#### Avro

**Avro** is a language-neutral data serialization system designed to address the major downside of Hadoop Writables: lack of language portability. Like Thrift and Protocol Buffers, Avro data is described through a **language-independent schema**.

Since Avro stores the schema in the **header** of each file, it’s self-describing and Avro files can easily be read later, even from a different language than the one used to write the file. Avro also provides better native support for MapReduce since Avro data files are compressible and splittable. Another important feature of Avro that makes it superior to SequenceFiles for Hadoop applications is support for **schema evolution**: the schema used to read a file does not need to match the schema used to write the file. This makes it possible to add new fields to a schema as requirements change.

Avro schemas are usually written in **JSON**. In addition to metadata, the file header contains a unique **sync marker**. Just as with SequenceFiles, this sync marker is used to separate blocks in the file, allowing Avro files to be splittable. Following the header, an Avro file contains a series of blocks containing serialized Avro objects. These blocks can optionally be compressed, and within those blocks, types are stored in their native format, providing an additional boost to compression.

## Columnar formats

Until relatively recently, most database systems stored records in a row-oriented fashion. This is efficient for cases where many columns of the record need to be fetched. For example, if your analysis heavily relied on fetching all fields for records that belonged to a particular time range, row-oriented storage would make sense. This option can also be more efficient when you’re writing data, particularly if all columns of the record are available at write time because the record can be written with a single disk seek. More recently, a number of databases have introduced columnar storage, which provides several benefits over earlier row-oriented systems:

- skips I/O and decompression (*if applicable*) on columns that are not a part of the query
- works well for queries that only access a small subset of columns
- is generally very efficient in terms of compression on columns because entropy within a column is lower than entropy within a block of rows. In other words, data is more similar within the same column, than it is in a block of rows
- is often well suited for data-warehousing-type applications where users want to aggregate certain columns over a large collection of records.

Columnar file formats supported on Hadoop include the RCFile format, which has been popular for some time as a Hive format, as well as newer formats such as the Optimized Row Columnar (ORC) and Parquet.

#### RCFile

The **RCFile** format was developed specifically to provide efficient processing for MapReduce applications, although in practice it’s only seen use as a **Hive storage format**. The RCFile format was developed to provide fast data loading, fast query processing, and highly efficient storage space utilization. The RCFile format breaks files into row splits, then within each split uses **column-oriented storage**.

Although the RCFile format provides advantages in terms of query and compression performance compared to SequenceFiles, it also has some deficiencies that prevent optimal performance for query times and compression. Newer columnar formats such as ORC and Parquet address many of these deficiencies.

#### ORC

The **ORC** format was created to address some of the shortcomings with the RCFile format, specifically around query performance and storage efficiency. The ORC format provides the following features and benefits, many of which are distinct improvements over RCFile:

- provides lightweight, always-on **compression** provided by type-specific readers and writers
- allows predicates to be pushed down to the **storage layer** so that only required data is brought back in queries
- supports the **Hive type model**, including new primitives such as decimal and complex types
- is a **splittable** storage format

A drawback of ORC as of this writing is that it was designed specifically for **Hive**, and so is not a general-purpose storage format that can be used with non-Hive MapReduce interfaces such as Pig.

#### Parquet

**Parquet** shares many of the same design goals as ORC, but is intended to be a **general-purpose storage format** for Hadoop.

The goal is to create a format that’s suitable for different MapReduce interfaces such as **Java**, [**Hive**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_hive.md), and [**Pig**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_pig.md), and also suitable for other processing engines such as [**Impala**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_impala.md) and [**Spark**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_spark.md). Parquet provides the following benefits, many of which it shares with ORC:

- similar to ORC files, Parquet allows for **returning only required data fields**, thereby reducing I/O and increasing performance
- provides **efficient compression**; compression can be specified on a per-column level
- is designed to support **complex nested data structures**
- stores **full metadata** at the end of files, so Parquet files are self-documenting
- fully supports being able to **read and write** to with Avro and Thrift APIs

## Failure behavior

Over time, we have learned that there is great value in having a single interface to all the files in your Hadoop cluster. And if you are going to pick one file format, you will want to pick one with a schema because, in the end, most data in Hadoop will be structured or semistructured data.
So if you need a schema, Avro and Parquet are great options. However, we don’t want to have to worry about making an Avro version of the schema and a Parquet version. Thankfully, this isn’t an issue because Parquet can be read and written to with Avro APIs and Avro schemas.

An important aspect of the various file formats is **failure handling**; some formats handle **corruption** better than others:

- **columnar formats**: while often efficient, do not work well in the event of failure, since this can lead to incomplete rows
- **Sequence files**: will be readable to the first failed row, but will not be recoverable after that row
- **Avro**: provides the best failure handling; in the event of a bad record, the read will continue at the next sync point, so failures only affect a portion of a file

## Types of compression

Compression is another important consideration for storing data in Hadoop, not just in terms of reducing storage requirements, but also to improve data processing performance. Because a major overhead in processing large amounts of data is disk and network I/O, reducing the **amount of data** that needs to be read and written to disk can significantly **decrease overall processing time**.

Although compression can greatly optimize processing performance, not all compression formats supported on Hadoop are splittable. Because the **MapReduce framework** splits data for input to multiple tasks, having a non-splittable compression format is an impediment to efficient processing.

#### Snappy

**Snappy** is a **compression codec** developed at Google for high compression speeds with reasonable compression. Although Snappy doesn’t offer the best compression sizes, it does provide a good trade-off between speed and size. Processing performance with Snappy can be significantly better than other compression formats. It’s important to note that Snappy is intended to be used with a container format like **SequenceFiles** or **Avro**, since it’s not inherently splittable.

#### LZO

**LZO** is similar to Snappy in that it’s optimized for speed as opposed to size. Unlike Snappy, LZO compressed files are **splittable**, but this requires an additional indexing step. This makes LZO a good choice for things like plain-text files that are not being stored as part of a container format. It should also be noted that LZO’s **license** prevents it from being distributed with Hadoop and requires a separate install, unlike Snappy, which can be distributed with Hadoop.

#### Gzip

**Gzip** provides very good compression performance (*on average, about 2.5 times the compression that’d be offered by Snappy*), but its write speed performance is not as good as Snappy’s. Gzip is also not splittable, so it should be used with a container format. Note that one reason Gzip is sometimes slower than Snappy for processing is that Gzip compressed files take up fewer blocks, so fewer tasks are required for processing the same data. For this reason, using smaller blocks with Gzip can lead to better performance.

#### bzip2

**bzip2** provides excellent compression performance, but can be significantly slower than other compression codecs such as Snappy in terms of processing performance. Unlike Snappy and Gzip, bzip2 is inherently splittable. In general bzip2 is about 10 times slower than GZip. For this reason, it’s not an ideal codec for Hadoop storage, unless your primary need is reducing the storage footprint. One example of such a use case would be using Hadoop mainly for active **archival purposes**.

#### Compression recommendations

In general, any compression format can be made splittable when used with container file formats (*Avro, SequenceFiles, etc.*) that compress blocks of records or each record individually. If you are doing compression on the entire file without using a container file format, then you have to use a compression format that inherently supports splitting.

Here are some recommendations on compression in Hadoop:

- **enable compression of MapReduce intermediate output**: this will improve performance by decreasing the amount of intermediate data that needs to be read and written to and from disk
- **pay attention to how data is ordered**: often, ordering data so that like data is close together will provide better compression levels; data in Hadoop file formats is compressed in chunks, and it is the entropy of those chunks that will determine the final compression (*e.g. if you have stock ticks with the columns timestamp, stock ticker, and stock price, then ordering the data by a repeated field, such as stock ticker, will provide better compression than ordering by a unique field, such as time or stock price*)
- **consider using a compact file format with support for splittable compression**: such as Avro.

A single HDFS block can contain multiple Avro or SequenceFile blocks. Each of the Avro or SequenceFile blocks can be compressed and decompressed individually and independently of any other Avro/SequenceFile blocks. This, in turn, means that each of the HDFS blocks can be compressed and decompressed **individually** thereby making the data splittable, as shown in the following figure:

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/images/hadoop_avro_compression.png" />
</p>

## Useful readings

- [**Hadoop Application Architectures**](https://www.safaribooksonline.com/library/view/hadoop-application-architectures/9781491910313/) -  A book on architecting end-to-end data management solutions with Apache Hadoop
