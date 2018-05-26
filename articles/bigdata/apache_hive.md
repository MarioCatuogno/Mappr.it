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
| **Smallint** | 2 byte signed integer | 20 |
| **Int** | 4 byte signed integer | 20 |
| **Bigint** | 8 byte signed integer | 20 |
| **Boolean** | Boolean true or false | TRUE |
| **Float** | Single precision floating point | 3.14159 |
| **Double** | Double precision floating point | 3.14159 |
| **String** | Sequence of characters. The character set can be specified. Single or double quotes can be used | "Now it's the time" |
| **Timestamp** | Integer, float, or string | 1327882394 (Unix epoch seconds) |
| **Binary** | Array of bytes | |

Hive supports columns that are **structs**, **maps** and **arrays**:

| **Type** | **Descrpition** | **Example** |
| :---: | :---: | :---: |
| **Struct** |  Fields can be accessed using the “dot” notation. For example, if a column name is of type STRUCT {first STRING; last STRING}, then the first name field can be referenced using name.first | struct('John', 'Doe') |
| **Map** | A collection of key-value tuples, where the fields are accessed using array notation (e.g., ['key']). For example, if a column name is of type MAP with key→value pairs 'first'→'John' and 'last'→'Doe', then the last name can be referenced using name['last'] | map('first', 'John', 'last', 'Doe') |
| **Array** | Ordered sequences of the same type that are indexable using zero-based integers. For example, if a column name is of type ARRAY of strings with the value ['John', 'Doe'], then the second element can be referenced using name[1] | array('John', 'Doe') |

Here is a table declaration that demonstrates how to use these types:

```sql
CREATE TABLE movies (
name      STRING,
rating    FLOAT,
plot      ARRAY<STRING>,
location  STRUCT<nation:STRING, city:STRING, zip:INT>);
```

## Useful readings

- [**Link1**](https:link1.com) - Description
