# Hadoop 102 : HDFS
*An introduction to HDFS*

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/headers/header_hadoop_102.png" />
</p>

`NOTE: This article is an excerpt from Mark Grower, "Hadoop: The Definitive Guide"`

## Table of contents

- [1. Introduction](#introduction)
- [2. HDFS concepts](#hdfs-concepts)
  - [2.1 HDFS Blocks](#hdfs-blocks)
  - [2.2 Namenodes and Datanodes](#namenodes-and-datanodes)
  - [2.3 Block caching](#block-caching)
  - [2.4 HDFS federation](#hdfs-federation)
  - [2.5 HDFS high availability](#hdfs-high-availability)
- [X. Useful readings](#useful-readings)

## Introduction

When a dataset outgrows the storage capacity of a single physical machine, it becomes necessary to **partition** it across a number of separate machines. Filesystems that manage the storage across a network of machines are called **distributed filesystems**. Since they are network based, all the complications of network programming kick in, thus making distributed filesystems more complex than regular disk filesystems. For example, one of the biggest challenges is making the filesystem tolerate node failure without suffering data loss.

Hadoop comes with a distributed filesystem called **HDFS**, which stands for **Hadoop Distributed Filesystem**.

HDFS is designed for storing very large files with streaming data access patterns, running on clusters of commodity hardware. So HDFS is built around the idea that the most efficient data processing pattern is a **write-once**,  **read-many-times** one.

## HDFS concepts

#### HDFS Blocks

A disk has a block size, which is the minimum amount of data that it can read or write. Filesystems for a single disk build on this by dealing with data in blocks, which are an integral multiple of the disk block size (*filesystem blocks are typically a few kb in size, whereas disk blocks are normally 512 bytes*).

HDFS too, has the concept of a block, but it is a much larger unit: **128MB** by default. Files in HDFS are broken into block-sized chunks, which are stored as independent units. Having a **block abstaction** for a distributed filesystem brings several benefits:

- a file can be larger than any single disk in the network
- simplify the storage subsystem (because of the blocks fixed size, it is easy to calculate how many can be stored on a single disk)
- blocks fit well with replication for providing fault tolerance and availability

To insure against corrupted blocks and disk and machine failure, each block is replicated to a small number of physically separate machines (typically three). If a block becomes unavailable, a copy can be read from another location in a way that is transparent to the client. A block that is no longer available due to corruption or system failure, can be replicated from its alternative locations to other live machines to bring the **replication factor** back to normal level.

The `fsck` command undestrand blocks, the following example will list the blocks that make up each file in the filesystem.

```hdfs
% hdfs fsck / -files -blocks
```

#### Namenodes and Datanodes

An HDFS cluster has two types of nodes operating in a **master-worker** pattern: namenode (*master*) and a number datanodes (*workers*).

- **Namenode**

The **Namenode** manages the filesystem namespace, it mantains the filesystem tree and the metadata for all the files and directories in the tree. This information is stored persistently on the local disk in the form of two files: the **namespace image** and the **edit log**. The namenode also knows the datanodes on which all the blocks for a given file are located.

- **Datanode**

The **Datanodes** are the workhorses of the filesystem. They store and retrieve blocks when they are told to, and they report back to the namenode periodically with lists of blocks that they are storing.

Without the namenode, the filesystem cannot be used: in fact, if the machine running the namenode were obliterated, all the files on the filesystem would be lost since there would be no way of knowing how to reconstruct the files from the blocks on the datanodes. So it is important to make the namenode **resilient to failure**, and Hadoop provides two mechanism for this:

- backup the files that make up the persistent state of the filesystem metadata, Hadoop writes its persistent state to multiple filesystems
- it is also possible to run a **secondary namenode**, which despite its name does not act as a namenode. Its main role is to periodically merge the namespace image with the edit log to prevent the event log from becoming too large. It keeps a copy of the merged namespace image, which can be used in the event of the namenode failing.

#### Block caching

Normally a datanode reads blocks from disk, but for frequently accessed files the blocks may be explicitly cached in the datanode's memory, in an off-heap **block cache**. By default, a block is cached in only one datanode's memory, although the number is configurable on a per-file basis.

Job schedulers (for [**MapReduce**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/hadoop_103.md), [**Spark**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_spark.md) and other frameworks) can take advantage of cached blocks by running tasks on the datanode where a block is cached, for increased read performance.

#### HDFS federation

The namenode keeps a reference to every file and block in the filesystem in memory, which means that on very large clusters with many files, memory becomes the limiting factor for scaling.

HDFS federation, introduced with Hadoop 2.x release series, allows a cluster to scale by adding namenodes, each of which manages a portion of the filesystem namespace.

#### HDFS high availability



## Useful readings

- [__Hadoop: The Definitive Guide__](https://www.safaribooksonline.com/library/view/hadoop-the-definitive/9781491901687/) - This book is ideal for programmers looking to analyze datasets of any size, and for administrators who want to set up and run Hadoop clusters
