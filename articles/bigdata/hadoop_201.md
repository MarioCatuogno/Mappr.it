# Hadoop 201 : Data Warehouse
*An introduction to Data Warehouse with Hadoop*

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/headers/header_hadoop_201.png" />
</p>

## Table of contents

- [1. Introduction](#big-data-and-data-science)
- [2. Using Hadoop for Data Warehousing](#using-hadoop-for-data-warehousing)
- [3. OLTP schema with Hadoop](#oltp-schema-with-hadoop)
  - [3.1 Denormalization in Hadoop](#denormalization-in-hadoop)
  - [3.2 Updates in Hadoop](#updates-in-hadoop)
  - [3.3 Storage format](#storage-format)
  - [3.4 Partitioning](#partitioning)
- [X. Useful readings](#useful-readings)

## Introduction

One of the most common applications of Hadoop is as a complement to a DWH architecture, often referred as *DWH offload*.

This DWH offload with Hadoop encompases several areas, including:

- **ETL/ELT**: In the ELT model, data is loaded into a target system, and then transformed
- **Data archiving**: since traditional DBs are often difficult and expensive to scale as data volumes grow, it's common to move historical data to Hadoop, which provides an effective and inexpensive option to external archival systems such as tape
- **Exploratory analysis**: Hadoop provides an ideal sandbox to perform analysis on data that's not available in the DWH

With recent updates, Hadoop also has functionality required of a DWH, including reliable data storage and the **low-latency query capabilities** required to run user reports and queries.

The following figure shows a high-level view of the main components in a typical DWH architecture:

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_hadoop201_model1.png" />
</p>

- **Operational Source Systems**: are the OLTP that captures the transactions of a business
- **Data Staging Area**: acts as a temporary storage area and as a platform for ETL/ELT processing
- **DWH**: is where data is made available to users for analysis, reporting and so on

## Using Hadoop for Data Warehousing

Hadoop provides benefits as a complement to a DWH by providing a cost-effective and scalable platform for data processing. Making the data available in Hadoop can provide an exploratory sandbox for analysts to discover potentially valuable data that might otherwise never be available in the DWH.

Flexibility of Hadoop allows for evolving schemas and handling semistructured and unstructured data, which enables fast turnaround time when changes to downstram reports or schemas happen.

The following figure shows a high-level view of an architecture where Hadoop complements the DWH:

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_hadoop201_model2.png" />
</p>

1. On the left-hand side there's still an OLTP system but it's common to see additional multistructured sources of data introduced with Hadoop, such as web logs or social media feeds. For traditional data sources the **data ingestion** mechanism is often [**Sqoop**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_sqoop.md), while for event-based data it's commonly used a tool like [**Flume**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_flume.md) or [**Kafka**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_kafka.md).
2. Hadoop has replaced the staging layer in the previous architecture. The source data is ingested into HDFS before being transformed and loaded into target systems. Transformation occur through one of the **processing frameworks** supported, such as [**MapReduce**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/hadoop_103.md), [**Spark**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_spark.md), [**Hive**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_hive.md) or [**Pig**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_pig.md).
3. Once transformed, data is moved into target systems. In the example, this includes hosting the data in Hadoop via a SQL-like interface such as Hive or [**Impala**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_impala.md), or exporting into the DWH for further analysis. Additionally the figure also shows that we often move data from the DWH back into Hadoop.
4. One contrast with the traditional DWH architecture is that with the previous architecture, analysis tools never access the data in the staging area. In the Hadoop architecture, data analysis is commonly run against both the DWH and the data in Hadoop.
5. The processing is likely be orchestrated by a **workflow management tool** such as [**Oozie**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_oozie.md).

## OLTP schema with Hadoop

As seen [**here**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/database/dwh_architectures.md#oltp-schema), the goal of an OLTP schema is to normalize the data so as to reduce redundancy and provide data integrity.

The **dimensional model** tends to be a better fit for analysis in Hadoop.

Hadoop stores the same data that is stored in the OLTP database, however, this data is stored differently.

Some access pattern are particularly suited for storing data on HDFS because of the need for higher scan performance instead of random read performance.

#### Denormalization in Hadoop

Data is often denormalized when being brought into Hadoop. As a side effect, we pay the price of some **redundancy**, but it comes at the benefit of faster analysis. An example is the following:

> Consider a MOVIE table. Associated with the MOVIE table, we have MOVIE_GENRE and GENRE tables as well. The GENRE table is a small dimensional table and it will mostly be queried in conjunction with the MOVIE and MOVIE_GENRE tables.

In this case we have two options:

1. We can have three data sets in Hadoop: MOVIE, MOVIE_GENRE and GENRE. The benefit of this option is that it keeps the ingest pipeline very simple since it is copying over the tables directly from OLTP database. However the donwside is that during querying we will almost always have to have a join with all three tables.
2. We can denormalize the MOVIE, MOVIE_GENRE and GENRE tables into one data set on Hadoop. We will have to have an additional step in our ingest pipeline. However it will make querying faster since all the data is stored in the same data set in Hadoop. Since a movie can have multiple genres associated with it, we will likely leverage a derived data type like an array (*or comma-separated genre list*) to store all genres for a movie.

**Joins** are very costly compared to having data in the same table, so if and where it makes sense, denormalizing (*pre-aggregation*) should be given consideration. The exact choice, however, would depend on the schema distribution and size of the data sets.

Part of the reason joins are expensive is that when you have two data sets, there are no guarantees that corresponding records (*records with the same join key*) across the two data sets will be present on the same node of your cluster and streaming these data sets across the network takes time and resources. Having the data in the same data set implies that it's guaranteed that the various fields of a record would be present on the same node.

#### Updates in Hadoop

With Hadoop we cannot simply overwrite the updated records in place because we are storing our data in HDFS, which doesn't allow in-place updates.

One approach is that in Hadoop, instead of overwriting the entries in place, we create tables where we only **append**, not overwrite. With this approach, on every run of the data ingest job, we retrieve a list of new records that either neet to be inserted or updated to the Hadoop data set. These new records get appended to the **bottom** of the Hadoop data set.

For example:

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_hadoop201_table1.png"/>
</p>

This represent an USER table before the update. Now let's say the user with ID 1 updated his zipcode and a new user with ID 3 got added the same day. The new table in Hadoop would look like the following:

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_hadoop201_table2.png"/>
</p>

Notice that the zipcode column for the user with ID 1 has been updated in place from 80131 to 80100 and also note that a new user has been added with ID 3.

While this approach preserves history, it makes it harder to query this data set in Hadoop. For example if we want to execute a query to find the zipcode for the user with ID 1, we would have to potentially go through the entire data set and then pick the latest entry based on the last_update column. This makes query **slow to run and cumbersome to write**.

The solution for this is to have two copies of specific data sets in Hadoop. For the USER data set, the first copy called USER_HISTORY, would store user data in Hadoop, the way is described so far. It will be an **append-only** data set that will keep track of every single row that gets updated or added to the user table. The second copy called USER, would store only one record per user, which would correspond to its most recent state.

The USER_HISTORY data set would be used for queries that require knowledge of the lineage of changes. The USER would be used for queries that don't require **granularity** of when the user data was modified. Both copies would have the same schema, they will just have different semantics of what's stored in those tables.

The workflow of having two data sets each is represented in the following figure:

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_hadoop201_model3.png"/>
</p>

The data ingest pipeline will add new inserts/updates to USER_HISTORY on every run. After that a separate process will be triggered to run a **merge** that takes the new inserts/updates and merges them into the existing USER.

#### Storage format

Since relational data is typically structured in tables, the obvious candidates are **CSV**, [**Avro**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_avro.md) and [**Parquet**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/apache_parquet.md). CSV is a suboptimal choice in terms of performance, compression and schema management, so the choice will be between the latter two. Both are binary formats that contains schemas describing the data.

- **Avro**: has the advantage of supporting schema evolution. This means that the ETL process will not break when the OLTP schema is modified. Failed ETL processes due to schema maintenance that was not coordinated properly are a major concern for the ETL developers and administrators, so this advantage can be quite important.
- **Parquet**: has significant performance advantages. Fact tables in DWH are typically very wide, since the data relates to many dimensions. But each analytical query or aggregation usually requires only a small subset of columns. As a columnar format, Parquet will allow queries to skip blocks containing data from unused columns and, in addition, Parquet tables compress very well, which leads to significant space savings.

The rule of thumb is to use Avro for everything that makes sense to be stored in a **row-major manner** and Parquet for everything that needs to be stored in a **column-major manner**.

For data sets that get appended (*USER_HISTORY*) usually Avro is a better choice, while for data sets on which we often want to do queries that involve aggregations over a subset of columns (*USER*), Parquet is better.

For compression [**Snappy**](https://github.com/MarioCatuogno/Mappr.it/blob/master/articles/bigdata/hadoop_101.md#snappy) is one of the most used compression codecs for all data sets stored in Avro or Parquet.

#### Partitioning

## Useful readings

- [**Hadoop Application Architectures**](https://www.safaribooksonline.com/library/view/hadoop-application-architectures/9781491910313/) -  A book on architecting end-to-end data management solutions with Apache Hadoop
