# Apache HBase
*A beginner's guide to Apache HBase*

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/headers/header_apache_hbase.png" />
</p>

`NOTE: This article is an excerpt from Mark Grower, "Hadoop: The Definitive Guide"`

## Table of contents

- [1. Introduction](#introduction)
- [2. HBase data model](#hbase-data-model)
  - [2.1 HBase regions](#hbase-regions)
  - [2.2 HBase implementation](#hbase-implementation)
  - [2.3 HBase scanner](#hbase-scanner)
- [3. HBase vs RBDMS](#hbase-vs-rdbms)
- [X. Useful readings](#useful-readings)

## Introduction

**Apache HBase** is a column-oriented real-time database for random read/write access.

Altough there are contless strategies and implementations for DB storage and retrieval, most solutions are not built with very large scale and distribution in mind. HBase approaches the scaling problem from the opposite direction, itt is built from the ground up to **scale linearly**, just by adding nodes. HBase is not relational and does not support SQL, but it can host very large, sparsely populated tables on clusters made from commodity hardware.

## HBase data model

Applications store data in **labeled tables**. Tables are made of rows and columns, table cells are versioned. By default their version is a **timestamp** auto-assigned by HBase ath the time of cell insertion. An example of the HBase data model is the following one, storing photos:

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_hbase_table1.png"/>
</p>

**TABLE ROW KEYS** are also byte arrays, so theoretically anything can serve as a row key, from string to binary representation of long or even serialized data structures. Table rows are sorted by row key, the sort is byte-ordered.

**ROW COLUMNS** are grouped into **COLUMN FAMILIES**. All column families members have a common prefix (*e.g. info:*). A table's column families must be specified up front as part of the table schema definition, but new column family members can be added on demand.

Physically, all column family members are stored together on the filesystem. So although earlier we described HBase as column-oriented store, it would be more accurate if it were described as a **column-family-oriented** store.

HBase tables are like those in an RDBMS, only cells are versioned, rows are sorted, and columns can be added on the fly by the client as long as the column family they belong to preexist.

#### HBase regions

Tables are automatically partitioned horizontally by HBase into **regions**. Each region comprises a subset of a table's rows and is denoted by the table it belongs to, its first row (*inclusive*) and its last row (*exclusive*). Initially, a table comprises a single region, but as the region grows, it splits at a row boundary into two new regions of approximately equal size.

Regions are the units that get distributed over an HBase cluster, in this way, a table that is too big for any one server, can be carried by a cluster of servrs, with each node subset of the table's total regions.

Row updates are **atomic**, no matter how many row columns constitute the row-level transaction. This keeps the locking model simple.

#### HBase implementation

Just as [**HDFS**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/hadoop_102.md) and [**YARN**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/hadoop_104.md) are built of clients, workers and coordinating master (*the namenode and datanode in HDFS and resource manager and node managers in YARN*), so is HBase made up of an **HBase master node** orchestrating a cluster of one or more **regionserver workers**.

The master is responsible for assigning regions to registered regionservers and for recovering regionservers failures. The regionservers manage region splits, informing the master about new daughter regions.

HBase depends on [**ZooKeeper**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_zookeeper.md) and the assignement of regions is mediated via in case participating servers crash mid-assignment. HBase persists data via the Hadoop filesystem API. Most people using HBase run it on HDFS for storage, though by default HBase writes to the local filesystem, pointing at the HDFS cluster it should use. The following diagram shows HBase cluster members:

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_hbase_model1.png"/>
</p>

Internally, HBase keeps a special catalog table named `hbase:meta`, within which it maintains the current list, state and locations of all user-space regions afloat on the cluster. Entires in `hbase:meta` are keyed by:

- **region name**, where a region name is made up of the name of the table the region belongs to
- **the region's start row**
- **its time of creation**
- **MD5 hash** of all of the previous

As noted previously, row keys are sorted, so finding the region that hosts a particular row is a matter of a lookup to find the largest entry whose key is less than or equal to that of the requested row key. As regions transition (*split, disabled, enabled, deleted or redeployed for loading balance*) the catalog table is updated so the state of all regions on the clusters is kept current.

Fresh clients connect to the ZooKeeper cluster first to learn the location of `hbase:meta`, the client then does a lookup against the appropriate `hbase:meta` region to figure out the hosting user-space region and its location. Thereafter, the client interacts directly with the hosting regionserver.

#### HBase scanner

**HBase scanners** are like cursors in a traditional DB or Java iterators, except they have to be closed after use. Scanners return rows in order. Users obtain a scanner or a TABLE OBJECT by calling `getScanner()`, passing a configured instance of a SCAN OBJECT as parameter. In the scan instance you can pass the row at which to start and stop the scan, which columns in a row to return in the row result, and a filter to run on the server side.

Scanners will, under the covers, fetch batches of 100 rows at a time, bringing them client-side and returning to the server to fetch the next batch only after the current batch has been exhausted. Higher caching values will enable faster scanning but will eat up more memory in the client.

## HBase vs RDBMS

HBase is often compared to more traditional and popular relational DBs. Although they differ drammatically in thei implementations and in what they set out to accomplish, the fact that they are potential solutions to the same problems means that despite their enormous differences, the comparison is a fair one to make.

HBase is a distributed, column-oriented data storage system. It picks up where Hadoop left off by providing random reads and writes on top of HDFS. It has been designed from the ground up with a focus on scale in every direction: tall in numbers of rows (*billions*), wide in number of columns (*millions*) and able to be horizontally partitioned and replicated across thousands of commodity nodes automatically. The **table schema** mirror the physical storage, creating a system for efficient data structure serialization, storage and retrieval. The burden is on the application developer to make use of this storage and retrieval in the right way.

Typical RDBMS are fixed-schema, row-oriented DBs with [**ACID**](https://en.wikipedia.org/wiki/ACID) properties and sophisticated SQL query engine. The emphasis is on strong consistency, referential integrity, abstraction from the physical layer and complex queries through the SQL language.

The scaling of RDBMS usually involves loosening ACID restrictions and loosing most of the desirable properties that made relational DBs so convenient in the first place.

## Useful readings

- [__Hadoop: The Definitive Guide__](https://www.safaribooksonline.com/library/view/hadoop-the-definitive/9781491901687/) - This book is ideal for programmers looking to analyze datasets of any size, and for administrators who want to set up and run Hadoop clusters
