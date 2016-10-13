# DB architectures
*A beginners guide to Databases*

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/headers/header_db_architectures.png" />
</p>

## Table of contents

- [1. DBMS](#dbms)
    - [1.1 What is a database](#what-is-a-database)
    - [1.2 RDBMS](#rdbms)
    - [1.3 Conceptual data model](#conceptual-data-model)

## DBMS

#### What is a database?

A **database** is a collection of logically related data, used to represent and model a specific problem. **DBMS** (*Database Management System*) was invented to solve some of the traditional problem of **data archives**, some of them were:

Problem | Description
--- | ---
__Atomicity of updates__ | failures may leave archives in an inconsistent state with partial updates carried out
__Concurrent access__ | by multiple users, can lead to inconsistency
__Inconsistency__ | multiple file formats
__Integrity__ | integrity constraints are buried into the code, rather than being stated explicitly
__Redundancy__ | duplication of information in different files
__Security__ | hard to provide user access to parts of data

The benefits of a DBMS are:

Benefit | Description
--- | ---
__Data independence__ | application worry about what data they want, not how they are stored
__Efficient data access__ | DBMS is smart about how to retrieve data
__Data integrity__ | DBMS won't let corrupt your data
__Concurrent access__ | handles multiple users efficiently

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_dbms1.png" />
</p>

DBMS is a system software for creating and managing databases. It provides users and programmers a way to create, retrieve, update and manage data.

#### RDBMS

A specific kind of DBMS are the **Relational Database Management System**, because they have:

* __Built-in multilevel integrity__:
    * at __field level__ to ensure data accuracy,
    * at __table level__ to ensure records are not duplicated,
    * at __relationship level__ to ensure the consistency of data,
    * at __business level__ to ensure data is accurate in terms of business rules
* __Guaranteed data consistency and accuracy__
* __Logical and physical data independence__
* __Easy data management and retrieval__

A relational database, is crucial to **consistency**, **integrity** and **accuracy** of data.

A **data model** describes how data is represented and accessed. It also determines an application's data quality, for each level there is a specific role:

* __Conceptual level__ (*business analyst*): describes WHAT the system contains
* __Logical level__ (*data architect*): describes HOW the concepts will be represented and linked, regardless of the DBMS
* __Physical level__ (*db admin*): describes HOW and WHERE the system will be implemented using a specific DBMS

#### Conceptual data model

The purpose is to clearly describe the concepts relevant to a domain, the relationship between concepts and information associated with them. The **Entity Relationship** (**ER**) model describe a problem as:

* a collection of __entities__ described by __attributes__
* __relationship__ among entities
