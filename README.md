# DAT262 - Integrating SAP S/4HANA into SAP Data Intelligence: Deep Dive and Hands On

## Description

This repository contains the material for the SAP TechEd 2020 session called<br>
**DAT262 - Integrating SAP S/4HANA into SAP Data Intelligence: Deep Dive and Hands On**.

## Overview

This session introduces attendees to the options that SAP Data Intelligence offers to integrate S/4HANA systems with its enterprise-wide, holistic Data Management and Data Orchestration capabilities for SAP and non-SAP environments.

## Requirements

The requirements to follow the exercises in this repository are:
- Basic knowhow about the usage and scope of S/4HANA 
- General understanding of Enterprise Data Management and ETL (Extraction, Transformation, Load)
- Basic programming skills in ABAP are of advantage

## Deep Dive Demos and Hands-On Exercises

Provide the exercise content here directly in README.md using [markdown](https://guides.github.com/features/mastering-markdown/) and linking to the specific exercise pages, below is an example.

- **[Getting Started](exercises/ex0/)**

- **[Deep Dive 1 - ABAP CDS View based data extraction in SAP Data Intelligence](exercises/dd1/)**
    - [Deep Dive 1.1 - Create a Simple ABAP CDS View in ABAP Develoment Tools (ADT)](exercises/dd1#deep-dive-11---create-a-simple-abap-cds-view-using-abap-develoment-tools-adt)
    - [Deep Dive 1.2 - Create a complex ABAP CDS View in ABAP Development Tools (ADT) (joining multiple tables)](exercises/dd1#deep-dive-11---create-a-simple-abap-cds-view-using-abap-develoment-tools-adt)
    - [Deep Dive 1.3 - Enable Delta Extraction in simple and complex ABAP CDS Views](exercises/dd1#exercise-12-sub-exercise-2-description)
    - [Deep Dive 1.4 - Integrate ABAP CDS Views in SAP Data Intelligence Pipelines](exercises/dd1#exercise-12-sub-exercise-2-description)
- **[Deep Dive 2 - Calling an ABAP function module in SAP S/4HANA from SAP Data Intelligence](exercises/dd2/)**
    - [Deep Dive 2.1 - Create a custom ABAP Operator in SAP S/4HANA](exercises/dd2#exercise-21-sub-exercise-1-description)
    - [Deep Dive 2.2 - Integrate the custom ABAP Operator in a SAP Data Intelligence Pipeline](exercises/dd2#exercise-22-sub-exercise-2-description)
    
- **[Exercise 1 - Replicating data from ABAP CDS Views in SAP Data Intelligence](exercises/ex1/)**
    - [Short introduction to the Enterprise Procurement Model (EPM)](exercises/ex1#exercise-10-sub-exercise-1-description)
    - [Exercise 1.1 - Consume ABAP CDS Views in SAP Data Intelligence](exercises/ex1#exercise-11-sub-exercise-1-description)
    - [Exercise 1.2 - Model a Pipeline that extracts Customer data from an ABAP CDS View](exercises/ex1#exercise-12-sub-exercise-1-description)
    - [Exercise 1.3 - Extend the Pipeline with Sales Order data from an ABAP CDS View](exercises/ex1#exercise-13-sub-exercise-1-description)
    - [Exercise 1.4 - Persist the results in an S3 Object Store](exercises/ex1#exercise-14-sub-exercise-1-description)
    - [Exercise 1.5 - Implement Delta Replication of Sales Order data in Pipelines](exercises/ex1#exercise-15-sub-exercise-2-description)
- **[Exercise 2 - Triggering the execution of a function module in a remote S/4HANA system](exercises/ex2/)**
    - [Exercise 2.1 - Making custom ABAP Operators available in SAP Data Intelligence](exercises/ex2#exercise-21-sub-exercise-1-description)
    - [Exercise 2.2 - Using a custom ABAP Operator to verify your Delta Replication of EPM Sales Orders](exercises/ex2#exercise-22-sub-exercise-2-description)


You can also find the exercise documentation [here](exercises/myPDFDoc.pdf).
    


## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.

## License
Copyright (c) 2020 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
