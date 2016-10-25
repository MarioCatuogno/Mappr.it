# Lambda Architecture
*An introduction to Lambda Architecture*

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/headers/header_lambda_architecture.png"/>
</p>

## Table of contents

- [1. Introduction](#introduction)
- [2. Useful readings](#useful-readings)

## Introduction

**Lambda architecture** is a data-processing architecture designed by [**Nathan Marz**](https://github.com/nathanmarz) to handle massive quantities of data by taking advantage of both batch and stream processing methods. **Batch processing** provides comprehensive and accurate views of batch data, while **stream processing** provide views of online data.

The rise of lambda architecture is correlated with the growth of big data, real-time analytics, and the drive to mitigate the latencies of MapReduce.

The following figure represent the Lambda architecture:

<p align="middle">
<img src="https://raw.githubusercontent.com/MarioCatuogno/Mappr.it/master/charts/diagram_lambda_model1.png"/>
</p>

1. All data entering the system is dispatched to both the **Batch Layer** and the **Speed Layer** for processing
2. The **Batch Layer** has two functions: managing the **master data set** (*an immutable append-only set of raw data*) and pre-computing the **batch accurate views**
3. The **Serving Layer** indexes the batch views so they can be queried in low-latency ad-hoc way
4. The **Speed Layer** compensates for the high latency of updates to the serving layer and deals with **recent data** only
5. Any incoming **Query** can be answered by mergint results from batch views and real-time views

## Useful readings

- [**Lambda Architecture Repository**](http://lambda-architecture.net) - A repository dedicated to the Lambda Architecture
