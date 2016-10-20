# Apache Sqoop
*A beginner's guide to Apache Sqoop*

IMG:APACHE SQOOP

## Table of contents

- [1. What is Sqoop?](#what-is-sqoop)

## What is Sqoop?

**Apache Sqoop** is a tool that uses **MapReduce** to transfer data between Hadoop clusters and relational databases very efficiently. It works by spawning tasks on multiple data nodes to download various portions of the data in parallel. When it finishes, each piece of data is replicated to ensure **reliability**, and spread out across cluster to ensure you can process it in parallel on your cluster.

There are 2 versions of Sqoop:

* __Sqoop1__: is a *thick client* and the command you run will directly submit the MapReduce jobs to transfer the data.
* __Sqoop2__: consists of a central server that submits the MapReduce jobs on behalf of clients, and a much lighter weight client that you use to connect to the the server.

With Sqoop
