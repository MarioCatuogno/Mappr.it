# Hadoop 201 : Data Warehouse
*An introduction to Data Warehouse with Hadoop*

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/headers/header_hadoop_201.png" />
</p>

## Table of contents

- [1. Introduction](#big-data-and-data-science)
- [2. Using Hadoop for Data Warehousing](#using-hadoop-for-data-warehousing)
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

## Useful readings

- [**Link1**](https:link1.com) - Description
