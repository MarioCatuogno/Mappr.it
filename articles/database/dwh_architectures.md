# Data Warehouse Architectures
*A beginners guide to Data Warehouses*

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/headers/header_dwh_architectures.png" />
</p>

## Table of contents

- [1. DWH](#dwh)
    - [1.1 What is a data warehouse?](#what-is-a-data-warehouse)
    - [1.2 Properties of a DWH](#properties-of-a-dwh)
    - [1.3 DWH vs OLTP](#dwh-vs-oltp)
    - [1.4 Main goals for a DWH system](#main-goals-for-a-dwh-system)
- [2. DWH definitions](#dwh-definitions)
    - [2.1 MOLAP](#molap)
- [3. DWH architecture](#dwh-architecture)
  - [3.1 DWH vs Data Marts](#dwh-vs-data-marts)
  - [3.2 The flow of data within a DWH](#the-flow-of-data-within-a-dwh)
  - [3.3 Metadata and data granularity](#metadata-and-data-granularity)
- [4. DWH architecture models](#dwh-architecture-models)
  - [4.1 Independent Data Marts](#independent-data-marts)
  - [4.2 Data Marts Bus architecture with linked Dimensional Marts](#data-marts-bus-architecture-with-linked-dimensional-marts)
  - [4.3 Hub and Spoke](#hub-and-spoke)
  - [4.4 Centralized DWH](#centralized-dwh)
  - [4.5 Federated](#federated)
  - [4.6 Inmon DWH model](#inmon-dwh-model)
  - [4.7 Kimball DWH model](#kimball-dwh-model)
  - [4.8 The future of DWH](#the-future-of-dwh)
- [5. OLTP schema](#oltp-schema)
  - [5.1 Dimensional model](#dimensional-model)
  - [5.2 Normalized model](#normalized-model)
- [6. Useful readings](#useful-readings)

## DWH

In computing a **Data Warehouse** (DWH) is a system used for **reporting** and **data analysis**, and is considered as a **core component** of **business intelligence** (BI).

#### What is a data warehouse?

There are many definitions of DWH, some of them are:

- a database designed for query and analysis rather than for transaction processing
- a database built for the purpose of analysis
- a database designed to help users understanding and enhancing their organization's performances
- a database designed to enable BI activities

The DWH has now been more generally seen as a **strategy** to bring **heterogenous data** together under a common conceptual and technical umbrella, and to make data available for new operation or decision support applications.

The DWH acts as the underlying engine used by BI environments that serve reports, dashboards and other interfaces to end users. Typical activities are:

- **slice and dice**: break a body of information down into smaller parts or to examine it from different viewpoints so that you can understand it better
- **drill-down**: the process of dividing an information area up into finer and finer layers in a hierarchy, but with the purpose of narrowing in to one small area or item

A DWH is subject-oriented, integrated, non-volatile and time-variant.

#### Properties of a DWH

- **DWH is subject-oriented**

Is the ability do define a DWH by **subject matter**. It is designed for query and analysis rather than for transaction processing and can be used to analyze a particular subject area.

- **DWH is integrated**

A DWH contains historical data derived from transactional data, but usually integrates data from **multiple data sources**. Source data may come from internally developed systems, purchased applications, 3rd party data, etc.

It may involve transactions, production, marketing, HR and more. It must resolve problems such as **naming conflicts** and **inconsistencies** among units of measure.

- **DWH is non-volatile**

Once data is in the DWH, it will not change, so **historical data** should never been altered.

The DWH separate **analysis workload** from **transactional workload** and enable the organization to consolidate data from several sources. This helps maintaining historical records, analyzing data to gain better understand of the business.

- **DWH is time-variant**

A DWH usually stores many months or years of data to support **historical analysis**, in order to discover trends and identify hidden patterns and relationships in business.

Users of the DWH performs **time-related data analysis**.

#### DWH vs OLTP

They both have very different requirements:

- **Data Warehouse** (DWH): separate analysis workload from transactional workload. Are very much read-oriented systems with an high demand of data reading. This enables far better analytical performance and avoid impacting the transactional system.
- **Online Transaction Processing** (OLTP): supports only business operations. The application can be built only to support a specific set of operations.

Feature | DWH | OLTP
--- | --- | ---
**Purpose** | Planning, problem solving and decision making | Control and run fundamental business tasks
**Workload** | Support for ad-hoc queries | Only predefined operations
**Schema model** | Dimensional schema (*STAR*) to optimize query and performances | Highly normalized schema (*3NF*)
**Data modification** | Periodic long-running update by the ETL process (*run nightly or weekly*) | Short and fast data update issued by the end users
**Elaborate data** | Millions of rows | Few rows
**Typical operations** | Find the total sales for all customers in the last month | Retrieve a specific order for a customer
**What data reveals** | Multidimensional view of various kinds of business activities | A snapshot of ongoing business processes
**Historical data** | Many months or years | A few weeks or months

#### Main goals for a DWH system

The DWH system make informations **easily accessible** in the following ways:

- understandable contents
- intuitive and obvious data meaning to business users
- data structure must mimic the business user's thought processes and vocabulary
- tools to extract and analyze data must be simple and fast in accessing data

The DWH system must present information **consistently**; as data is aquired from a variety of sources, it must be carefully *cleansed*, *quality-assured* and *released* only when it fits user consumptions.

The DWH must have the **right data** to support decision making.

The DWH must **adapt to change**; user's needs, business conditions, data and technology are all subject to change but existing data application should not be changed when the business community asks new questions as new data is added to the DWH.

The DWH must present information in a **timely way**; data must be up to data as much as possible.

The DWH must **protect information**; by *access control*.

The **business community** must accept the DWH system to use it successfully. It must become the organization's "*single source of truth*". The business community must embrace the DWH and actively use it.

## DWH definitions

There are two main POV of DWH:

- **Structural POV** ([Bill Inmon](https://en.wikipedia.org/wiki/Bill_Inmon)): a DWH is a subject-oriented, integrated,non-volatile and time-varian collection of data, in support of management's decision making process
- **Functional POV** ([Ralph Kimball](https://en.wikipedia.org/wiki/Ralph_Kimball)): a DWH is a copy of the transaction data specifically structured for query and analysis

The applications of a DWH are many:

- **Reporting**: the process of organizing data into informational summaries, in order to monitor how different area of business are performing (*Raw data -> Informations*)
- **Analysis**: the process of exploring data and report them, in order to extract **meaningful** information and insights which can be used to better understand and improve business performances (*Informations -> Insights*)
- **Business Intelligence**: a set of techniques and tools for the acquisition and transformation of raw data, into meaningful and useful informations for **business analysis** purposes. Main goals of BI are:
  - get deeper insights in business
  - improve productivity and efficiency
  - inform decision making
  - increase revenues and profitability
  - improve customer's service and satisfaction
  - enforce regulatory compliance
  - increase flexibility and agility
  - streamline budgeting and planning

#### MOLAP

A **Multidimensional Online Analytical Processing** (*MOLAP*) cube, is a dimensional structure that gives multidimensional views of various kinds of business activities, helping with *planning*, *problem solving* and *decision suppport*.

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/images/dwh_molap_cube.png" />
</p>

**MOLAP** cube contains dimensional attributes, facts and features:

- aggregated historical data stored in multidimensional schema
- complex queries which involve aggregate data
- response time as effectiveness measure
- huge metadata needs and great complexity

## DWH architecture

The main components of a DWH are:

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_dwh_model1b.png" />
</p>

- **Operational system**: a system designed to support day-to-day *transaction processing*; the main priorities are **performance** and **availability**. This source maintains little historical data
- **Cloud**: input data from cloud systems or other kind of flat files
- **ETL System** (Extract Transform Load): consists of a **set of processes** with the purpose of:
    - **Extract**: reading and understanding the data source and copying data needed into the ETL system for further manipulation
    - **Transform**: applying calculations, cleansing, combining data from multiple sources and de-duplicating data, data integration with metadata, aggregation
    - **Load**: load the cleansed data into the DWH
  - **3 main components**: staging area, warehouse and data marts. These component perform many functions such as:
    - logical conversion of files
    - domain verification
    - conversion from one DBMS to another
    - creation of default values when needed
    - summarization of data
    - addition of time values to data key
    - merging of records
    - deleting redundant data
    - restructuring of data keys
- **Staging Area**: contains copy of the source system's extraction. The main goal is to bring the data as fast as possible from sources to ETL engine, in order to minimize the source system interaction. Data in staging area are **temporary** and can be deleted after all data is loaded into the DWH
- **Warehouse**: is the **core component** and contains:
  - **raw data**: atomic data required for ad-hoc user queries
  - **summary data**: used to pre-compute long operations in advance
  - **metadata**: can be used as documentation or for **audit** purposes
- **Data Mart**: has the same role of the DWH but is **limited in scope**. It may server one particular department or business and exists in two forms:
  - **independent**: those which are fed directly from source data, can have problem tuning into islands of inconsistent information
  - **dependent**: fed from an existing DWH, dependent data marts can avoid the problem of inconsistency, but they require an enterprise-level DWH
- **Presentation Area**: is where data are made available for **direct querying** by users, report writers and other analytical BI applications. Data are presented with a **dimensional model** (*widely accepted as the preferred CONCEPTUAL SCHEMA for presenting data, because it addresses 2 requirements simultaneously: deliver data that are understandable to the user and deliver fast query performance*)

#### DWH vs Data Marts

DWH | Data Marts
--- | ---
Holds **multiple** subject areas | Often holds a **single** subject area
Holds **detailed** information | Hold **summarized** information
Works to **integrate** all data sources | Concentrates information from a given subject area
Does not necessarily use a dimensional model, but **feeds** other dimensional model DBs | Is built focused on a dimensional model using a **star schema**

#### The flow of data within a DWH

Step | Description
--- | ---
**Data Collection** | Identify the *data sources*. The owner of the data source should be responsible for maintaining data quality.
**Transformation and Cleansing** | The most time-consuming phase. Data are usually re-structured to optimize subsequent use for querying, reporting and analysis. This is usually done in the *staging area*. Cleansing consists of sending source data through a series of processing steps to improve the quality of data, some tasks are: correction of mispellings, resolving of domain conflicts, dealing with NAs or parsing into standard formats. The *data feed* needs to be run on a regular basis to keep the DWH up-to-date. This often has to be completed within the *overnight time window*.
**Analysis** | Selected data are taken from the central warehouse and processed to produce useful results. Often the most frequently accessed data are first summarized and then stored into Data Marts.
**Presentation** | Is displaying results for the end user, usually in the form of reports. The results might appear as text or charts and could be viewed online, printed or published on a web server.

#### Metadata and data granularity

**Metadata** is data that provides information about other data. There are various versions of metadata:

- **Business metadata**: it has the data ownership information, business definition and policies
- **Technical metadata**: it includes database system names, table and columns names and sizes, data types and allowed values. Also includes structural information such as primary and foreign keys
- **Operational metadata**: it includes currency of data and data lineage. Currency of data means whether the data is active, achieved or purged. Lineage of data means the history of data migrated and then transformation applied on it
- **Diagnostic metadata**: cleaning activities can be architectured to create diagnostic metadata, eventually leading to business process re-engineering to improve data quality in the source system over time
- **Audit dimension**: when a fact table is created in the ELT system, it is helpful to create an audit dimension containing the ETL processing metadata known at the time. A simple audit dimension row could contain one or more basic indicators of data quality, usually derived from an *error event schema* that records data quality violations encountered while processing the data
- **Atomic data**: DWH should contain atomic data, which are required to withstand *assaults* from unpredictable ad-hoc user queries. The most finely-grained data must be available in the presentation area so that the user can ask the most precise question possible.

## DWH architecture models

DWH and their architecture vary depending upon the specifics of the organization. The most commons are:

#### Independent Data Marts

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_dwh_model2.png"/>
</p>

Is common for *organizational units* to develop their own data marts. These marts typically have **inconsistent data definitions** and use different dimensions and measures, so the architecture is difficult to analyze across all marts.

#### Data Marts Bus architecture with linked Dimensional Marts

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_dwh_model3.png"/>
</p>

The building starts with the analysis of a business process, using **dimensions** and **measures** that will be used with other data marts (*conformed dimensions*). Additional marts are built with these dimensions and measures, which results in logically integrated marts with an *enterprise view* of the data. **Atomic** and **summarized** data are kept in the marts and are organized in a **Star Schema** to provide a dimensional view of data.

#### Hub and Spoke

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_dwh_model4.png"/>
</p>

The **Normalized Relational DWH** is the center of this architecture, built upon an extensive enterprise-level analysis of *data requirements*. **Atomic data** is maintained in the DWH in **3NF**, while **summarized data** are kept in the data marts which use the DWH as source system.

#### Centralized DWH

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_dwh_model5.png"/>
</p>

There are no dependent data marts. The DWH contains **atomic data**, some **summarized data** and **logical dimensional view** of the data.

#### Federated

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_dwh_model6.png"/>
</p>

Practical solution for firms that have a pre-existing, complex decision-support environment and do not want to rebuilt it. The data is either logically or physically integrated using **shared keys**, **global metadata**, **distributed queries** or other methods.

#### Inmon DWH model

[William Inmon](https://en.wikipedia.org/wiki/Bill_Inmon) suggests a **top-down development approach** that adapts traditional RDBMS tools to the development needs of an enterprise-wide DWH.

He defines DWH as a "*centralized 3NF relational repository for the entire enterprise*", which provides a **logical framework** for delivering BI. A normalized data model is designed first to store atomic data at the lowest level of detail, then the **individual dimensional data marts**, which contain data required for specific business processes or departments, are created from the DWH for decision-support tasks.

#### Kimball DWH model

[Ralph Kimball](https://en.wikipedia.org/wiki/Ralph_Kimball) suggest a **bottom-up approach** that uses **dimensional modeling** as a unique technique to design a DWH. The data marts facilitate reports and analysis and are created first. These provide a thin view into the organization's data, and, when required, they can be combined into a larger DWH.

Enterprise cohesion is given by a **data mart bus standard**.

#### The future of DWH

 | Inmon | Kimball
 --- | --- | ---
 **Building DWH** | Time consuming | Takes less time
 **Maintainance** | Easy | Difficult, redundant and subject to revision
 **Cost** | High initial costs. Subsequent phases cost lower | Low initial cost. Subsequent phases costs the same
 **Time** | Longer start-up time | Shorter start-up time
 **Data Integration requirements** | Enterprise-wide | Individual business areas

 The harder you work on really conforming the dimensions, the more your data marts looks like the DWH that Inmon advocates.

 While the ETL (*Extract Transform Load*) data transformation is performed in staging area, in ELT (*Extract Load Transform*) raw data is loaded directly into the DWH and transformed there.

 **ELT** is more useful for processing the large data sets required for big data analytics applications. The two previous authors have a different vision:

 - **Inmon 2.0**: new paradigm of DWH. Focus on the basic types of data, their structure, and how they relate to form a powerful store of data that meets the organization's needs for information. It is based on **data life-cycle** and its **declining probability of access**
 - **Kimball 2.0**: data feeds from sources must support huge bandwidths, metadata must be **open**, **universal**, **extensible**, **customizable** and **powerful**

## OLTP schema

The goal of an **OLTP schema** usually is to normalize the data so as to reduce redundancy and provide data integrity. It allows the user to easily and atomically create, delete or update a given field in just one table and have it automatically propagated thereafter.

Traditonal DWH design usually involves a variant of one of two common approaches for data modeling: the **dimensional model** (*star schema*) or the **normalized model**.

In general, the dimensional approach is easier and faster when you're performing analytical queries, while the normalized approach makes updating information easier.

#### Dimensional model

A dimensional model starts with a **FACT TABLE**, immutable values or measurements about entities in the DB (*e.g. phone call in a telecom use case, sale in a retail use case, etc*). To this fact table, the dimensional approach adds **DIMENSIONS**, tables with details about the entities in the system that give context to the facts (*e.g. in a retail example, a fact can be the sale of an item, the date of purchase and the location of purchase*).

#### Normalized model

A normalized approach to DWH, involves a design very similar to an OLTP database. Tables are created by **ENTITY** and designed to preserve the **3NF** where attributes depend on the primary key in its entirety (*e.g. in a retail example, an items table will contain the item name, since it depends completely on the item ID, but not the manufacturer name, which does not*). As results, queries on a normalized DWH schema usually involve long chains of joins.

## Useful readings

- [**Fundamentals of DBMS**](https://www.amazon.com/Fundamentals-Database-Management-Systems-Gillenson/dp/0470624701) - A book that provides concise coverage of the fundamental topics necessary for a deep understanding of the basics of DBMS
- [**The DWH toolkit**](http://www.kimballgroup.com/data-warehouse-business-intelligence-resources/books/data-warehouse-dw-toolkit/) - A classic guide to dimensional modeling which provides a complete collection of modeling techniques.
