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

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_dwh_model1a.png" />
</p>
