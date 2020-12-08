# DAT262 - Integrating SAP S/4HANA into SAP Data Intelligence: Deep Dive and Hands On

## Description

This repository contains the material for the SAP TechEd 2020 session called<br>
**DAT262 - Integrating SAP S/4HANA into SAP Data Intelligence: Deep Dive and Hands On**.

## Overview

SAP Data Intelligence is SAP's leading product for data integration, data management, intelligent processing and orchestration across different (hybrid) data environments.
One important integration aspect is the replication, consumption, and exchange of data and functionality with SAP's core Intelligent Enterprise and Business Suite systems. The ABAP integration capabilities in SAP Data Intelligence open seamless, "any-premise" interoperability with almost all the features available in these environments.<br>

This hands-on session will focus on connectivity and system interoperability to SAP S/4HANA systems and provides an insight on how to use the corresponding out-of-the-box operators in SAP Data Intelligence to smoothly integrate with the above SAP Business applications and beyond.


## Requirements

The requirements to follow the exercises in this repository are:
- Basic knowhow about the usage and scope of S/4HANA 
- General understanding of Enterprise Data Management and ETL (Extraction, Transformation, Load)
- Basic programming skills in ABAP are of advantage

## Deep Dive Demos and Hands-On Exercises

<!--
Provide the exercise content here directly in README.md using [markdown](https://guides.github.com/features/mastering-markdown/) and linking to the specific exercise pages, below is an example.
-->

The main use cases behind this workshop are
- The extraction the Business Partner master data in S/4HANA's demo application **Enterprise Procurement Model (EPM)** and making the records available for the corporate Data Analysts in an S3 object store.
- Also persist the transactional data in S3, i.e for EPM Sales Order objects which are built from joins over multiple EPM tables.
- In both cases, any single change of these data sources in the S/4HANA system has to be instantly and automatically replicated to the related files in S3.
- Additionally, the Sales Order data have to be enriched with Customer master data, for the initial load and then on every change committed to the EPM Sales Order data in S/4HANA.
- In order to create test changes on the EPM Sales Order data without having access to the SAP GUI in the S/4HANA system, (a variant of) the EPM Data Generator reports needs to be executed as an ABAP Function Modul call from a Data Intelligence Pipeline.<br>


The Deep Dive live demos and the hands-on Exercise contents reflect the above scenarios and provide a consecutive approach for their realization. But other than during the on-site TechEd events in the past years, it was not feasible to provide the Eclipse based ABAP Development Tools (ADT) and the SAP GUI to our participants in the this year's virtual version of the TechEd.<br><br>
Since it is a goal of this workshop to show and get the hands on the complete end-to-end implementation processes of ABAP integration with SAP Data Intelligence, all parts of this session that require these (local) applications will be presented as **live Deep Dive demos, conducted in ADT and in SAP S/4HANA by the trainer**, the **Exercises - in opposite - are then performed by the participants** in SAP Data Intelligence and will leverage those artifacts in S/4HANA that got created during the Deep Dive demos.<br><br>

- **[Overview and Getting Started](exercises/ex0/)**
     - [Deep Dive demos vs. Exercises](exercises/ex0#deep-dive-vs-exercise-sections-in-this-document)
     - [Short introduction to the Enterprise Procurement Model (EPM) in ABAP systems](exercises/ex0#short-introduction-to-the-enterprise-procurement-model-epm-in-sap-s4hana)
     - [Access to the exercises' Data Intelligence environment](exercises/ex0#access-to-the-exercises-data-intelligence-environment)

- **[Deep Dive 1 - ABAP CDS View based data extraction in SAP Data Intelligence](exercises/dd1/)**
    - [Deep Dive 1.1 - Create a simple ABAP CDS View in ABAP Develoment Tools (ADT)](exercises/dd1#deep-dive-11---create-a-simple-abap-cds-view-in-adt)
    - [Deep Dive 1.2 - Delta-enablement for simple ABAP CDS Views](exercises/dd1#deep-dive-12---delta-enablement-for-simple-abap-cds-views)
    - [Deep Dive 1.3 - Create a more complex ABAP CDS View in ADT (joining multiple tables)](exercises/dd1#deep-dive-13---create-a-more-complex-abap-cds-view-in-adt-joining-multiple-tables)
    - [Deep Dive 1.4 - Delta-enablement for complex ABAP CDS Views (joining multiple tables)](exercises/dd1#deep-dive-14---delta-enablement-for-complex-abap-cds-views-joining-multiple-tables)
    - [Deep Dive 1.5 - Integrate ABAP CDS Views in SAP Data Intelligence Pipelines](exercises/dd1#deep-dive-15---integrate-abap-cds-views-in-sap-data-intelligence-pipelines)
    
- **[Deep Dive 2 - Calling an ABAP function module in SAP S/4HANA from SAP Data Intelligence](exercises/dd2/)**
    - [Deep Dive 2.1 - Create a custom ABAP Operator in SAP S/4HANA](exercises/dd2#deep-dive-21---create-a-custom-abap-operator-in-sap-s4hana)
    - [Deep Dive 2.2 - Integrate the custom ABAP Operator in a SAP Data Intelligence Pipeline](exercises/dd2#deep-dive-22---integrate-the-custom-abap-operator-in-a-sap-data-intelligence-pipeline)
    
- **[Exercise 1 - Replicating data from S/4HANA ABAP CDS Views in SAP Data Intelligence](exercises/ex1/)**
    - [Exercise 1.1 - Consume the EPM Business Partner ABAP CDS Views in SAP Data Intelligence](exercises/ex1#exercise-11---consume-the-epm-business-partner-abap-cds-views-in-sap-data-intelligence)
    - [Exercise 1.2 - Extend the Pipeline to transfer Customer data to an S3 Object Store (Initial Load and Delta Load)](exercises/ex1#exercise-12---extend-the-pipeline-to-transfer-the-customer-data-into-an-s3-object-store-with-initial-load-and-delta-load-modes)
    - [Exercise 1.3 - Implement a Pipeline for replication of enhanced EPM Sales Order data to an S3 Object Store](exercises/ex1#exercise-13---implement-a-pipeline-for-delta-transfer-of-enhanced-epm-sales-order-data-from-s4hana-to-an-s3-object-store)
    - [Exercise 1.4 - Extend the Pipeline for joining Sales Order with Customer data for each change in Sales Orders and persist results in S3](exercises/ex1#exercise-14---extend-the-pipeline-for-joining-sales-order-with-customer-data-for-each-change-in-sales-orders-and-persist-results-in-s3)
    
- **[Exercise 2 - Triggering the execution of a function module in a remote S/4HANA system](exercises/ex2/)**
    - [Exercise 2.1 - Making custom ABAP Operators available in SAP Data Intelligence](exercises/ex2#exercise-21---making-custom-abap-operators-available-in-sap-data-intelligence)
    - [Exercise 2.2 - Using a custom ABAP Operator to verify your Delta Replication of EPM Sales Orders](exercises/ex2#exercise-22---using-a-custom-abap-operator-to-verify-your-delta-replication-of-epm-sales-orders)

<br>Here is a graphical overview about the topics and processes that we will focus on during this hands-on workshop and how they relate to each other.<br><br>
![](DAT262_Workshop_Scenarios.JPG)<br><br>

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.<br><br>

In case you have problems with implementing the pipelines, you can download (and then import into Data Intelligence Modeler) the example pipelines here:
- [Customer Master Data Replication Pipeline](images/teched.TA99.EPM_Customer_Replication_to_S3.json)

## License
Copyright (c) 2020 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
