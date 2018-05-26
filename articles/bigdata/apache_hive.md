# Apache Hive
*A beginner's guide to Apache Hive*

<p align="middle">
<img src="http://link.png" />
</p>

## Table of contents

- [1. Introduction](#introduction)
- [2. Data types and file format](#data-types-and-file-format)
- [X. Useful readings](#useful-readings)

## Introduction

**Apache Hive** is a SQL-like language for large data sets. Hive was initially developed by Facebook in 2007 to help the company handle massive amounts of new data. 

It is essentially a data warehouse system built on top of Hadoop which facilitates easy data summarization, ad-hoc queries, and the analysis of very large datasets that are stored in Hadoop. It provides a SQL interface, better known as **HiveQL** (**HQL**), which allows for easy querying of data, with its own *Data Definition* and *Data Manipulation* languages.

Hive is most suited for data warehouse applications, where relatively static data is analyzed, fast response times are not required, and when the data is not changing rapidly. Hive is not a full database. The design constraints and limitations of Hadoop and HDFS impose limits on what Hive can do. The biggest limitation is that Hive does not provide record-level **update**, **insert** nor **delete**.

So, Hive is best suited for data warehouse applications, where a large data set is maintained and mined for insights, reports, etc.

## Data types and file format

Hive supports several sizes of integer and floating-point types, a Boolean type, and character strings of arbitrary length. Hive v0.8.0 added types for timestamps and binary fields.
The following table lists the primitive types supported by Hive:

| **Type** | **Size** | **Example** |
| :---: | :---: | :---: |
| **Tinyint** | 1 byte signed integer | 20 |
## Useful readings

- [**Link1**](https:link1.com) - Description
