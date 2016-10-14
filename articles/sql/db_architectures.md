# DB architectures
*A beginners guide to Databases*

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/headers/header_db_architectures.png" />
</p>

## Table of contents

- [1. DBMS](#dbms)
    - [1.1 What is a database](#what-is-a-database)
    - [1.2 RDBMS](#rdbms)
- [2. Conceptual data model](#conceptual-data-model)
    - [2.1 Relationships among entities](#relationships-among-entities)
    - [2.2 Issues of conceptual design](#issues-of-conceptual-design)
- [3. Logical Data Model](#logical-data-model)

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

## Conceptual data model

The purpose is to clearly describe the concepts relevant to a domain, the relationship between concepts and information associated with them. The **Entity Relationship** (**ER**) model describe a problem as:

* a collection of __entities__ described by __attributes__
* __relationship__ among entities

An **ENTITY** is an object that exists and is distinguishable from other objects. An **ENTITY SET** is a set of entities of the same type that share the same properties. In ER notation, entity sets are represented by *RECTANGLES*.

A **RELATIONSHIP** is a meaningful association among entities. A **RELATIONSHIP SET** is a set of relationships among entities of the same entity set. There can be more than one relationship between two entity types. In ER notation, relationship set between two entities is represented by *DIAMOND*.

We usually distinguish between the definition of an object and the object itself:

* __schema of entity__: the definition of the entity
* __instance of entity__: each entity
* __schema of relationship__: the definition of the relationship
* __instance of relationship__: each relationship

An **ATTRIBUTE** is a property of an entity. By convention attribute names begin with lower case letter and each attribute has a **DOMAIN**, which are set of allowable values for that attribute. An attribute can be:

* __single__: if it contains a single component with an independent existence (*e.g. salary*)
* __composite__: if it consists of multiple components each with an independent existence (*e.g. address*)
* __multi-valued__: if it may have multiple values for a single entity instance (*e.g. telephone number*)
* __derived__: if its value is calculated from other attributes but it is not physically stored in the db (*e.g. age*)

There are also **KEY ATTRIBUTES**:

* __candidate key__: is a set of attributes that uniquely identifies each instance of the entity (*e.g. an ID which identifies an employee*)
* __composite key__: is a key that consists of two or more attributes (*e.g. an ID + name of the employee*)
* __primary key__: is a minimal candidate key that is selected to identify each instance of the entity (*e.g. an ID which identifies an employee*)

There are some rules to simplify the model:

* each **entity** should represent a single concept
* each **attribute** should describe the subject that the entity represents
* an entity has always **primary key**

#### Relationships among entities

A **relationship** can be described by attributes (so called **DYNAMIC ATTRIBUTES**).

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_er_model1.png" />
</p>

The **DEGREE OF RELATIONSHIP** is the number of entities participating in the relationship (*e.g. in the previous example, WorksOn is a relationship of degree=2 as the two participating entities are Employee and Project*). Relationships of arbitrary degree N are called **n-ary** (*e.g. binary, tertiary, quaternary, etc.*).

**RELATIONSHIP CARDINALITIES** is the number of possible occurrences of an entity that may relate to a single occurrence of an associated entity through a specific relationship. For binary relationships there are 3 common types:

* __One-to-Many__ (1:N)

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_er_model2.png" />
</p>

In a **1:N** relationship, each instance of the entity E1 can be associated with more than one instance of entity E2; however E2 can only be associated with at most one instance of E1. (*e.g. E1=Project and E2=Department, each project has one department, each department has 0 or more project*)

* __Many-to-Many__ (N:M)

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_er_model3.png" />
</p>

In a **N:M** relationship, each instance of the entity E1 can be associated with more than one instance of entity E2 and vice versa. (*e.g. E1=Project and E2=Employee, each project has 1 or more employee, each employee works on 0 or more projects*)

* __One-to-One__ (1:1)

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_er_model4.png" />
</p>

In a **1:1** relationship, each instance of the entity E1 can be associated with at most one instance of E2 and vice versa. (*e.g. E1=Employee and E2=Department, each employee may manage 0 or 1 department, each department may have at most 1 employee*)

A **RECURSIVE RELATIONSHIP** is a relationship where the same entity participates more than once in different instances (*e.g. an employee has a supervisor who is an employee himself*).

#### Issues of conceptual design

There are some issues concerning conceptual design:

* __use of entities vs attributes__: How much important is to store data as a separate entity? The right POV depends on the requirements, for example:

IMG:ENTITIES_VS_ATTRIBUTES

* __binary vs n-ary relationship__: It is better to decompose n-ary relationships into binary one, because they are easier to understand and design, for example:

IMG:BINARY_VS_NARY

## Logical Data Model
