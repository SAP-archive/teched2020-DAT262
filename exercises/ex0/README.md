# Overview and Getting Started

The idea of ABAP Integration is to establish a unified model to consolidate all interaction scenarios between SAP Data Intelligence and an ABAP-based SAP system (directional and bi-directional).

The primary use cases for ABAP integration in SAP Data Intelligence are metadata retrieval, data provisioning, and functional execution.
![](images/0-001a.JPG)

This Deep Dive and Hands-On Session concentrates on the uses cases of **ABAP Data Provisioning** and **ABAP Functional Execution**.<br><br>

**1. ABAP Data Provisioning**<br>
Getting access to and using real business data in an SAP Data Intelligence pipeline helps you to build new intelligent applications and data flows.<br>
For example, you may want to obtain the replication data from an SAP S/4HANA system, enrich the data in an SAP Data Intelligence pipeline, and then feed it to a target storage in the cloud.<br>
The ABAP data provisioning gives you access to SAP S/4HANA and allows you to consume ABAP CDS views directly in a pipeline. ABAP CDS is the semantically rich data model in SAP S/4 HANA and allows the consistent representation of a business object (such as a business partner). It is feasible to just get this data in an initial load, but also to have a stream approach established to consume every update, insert, and delete that happens in the SAP S/4HANA system.<br>For details about what options you have with the different release levels and release combinations, see [SAP Note 2830276](https://launchpad.support.sap.com/#%2Fnotes%2F2830276).

**2. ABAP Functional Execution**<br>
In certain scenarios, it is required to enhance the scope of a data-driven application by accessing and writing data into an SAP S/4HANA system. For example, it may be necessary to execute a function module or BAPI within a pipeline to read data into SAP Data Intelligence, post information into an ABAP-based SAP system, or trigger an execution in the remote system. If you require this type, you can create your own operator in SAP Data Intelligence that references the corresponding ABAP functionality. You can find a list of all available operators in the ABAP section of the [Repository Object Reference for SAP Data Intelligence](https://help.sap.com/doc/d131eca2150049da86c541ee0895177c/Cloud/en-US/cloud_loiod131eca2150049da86c541ee0895177c.pdf).

## Deep Dive vs. Exercise sections in this document

Other than in the on-site TechEd events in the last years, it was not feasible to provide the Eclipse based ABAP Development Tools (ADT) and the SAP GUI to our participants. But it is a goal of this workshop to show and get the hands on the complete end-to-end implementation processes of ABAP integration with SAP Data Intelligence.<br>
For this reason, all parts of this session that require these (local) applications will be presented as a live ***Deep Dive demo*** in ADT and on SAP S/4HANA by the trainer.<br>
The ***Exercises*** are then intended to be performed by the participants in SAP Data Intelligence and will leverage the objects in S/4HANA that got created during the Deep Dive sections.


## Access to the exercise Data Intelligence environment

All participant should have received the URL of the assigned SAP Data Intelligence tenant and an individual user/password combination, e.g.
- URL to SAP Data Intelligence: https://vsystem.ingress.dh-xyz.dhaas-live.xyz.xyz.k8s-hana.ondemand.com/login/?redirectUrl=%2Fapp%2Fdatahub-app-launchpad%2F
- User: TAXX
- PW: Welcome00

All SAP Data Intelligence environments that are used for this hands-on workshop already have a Connection of type RFC to the backend SAP S/4HANA system set up (see in DI Connection Management). In the Pipelines that you create in SAP Data Intelligence, you can leverage the existing RFC Connection and find and integrate the provided CDS Views and custom ABAP Operators that were implemented during the Deep Dive parts of the workshop. 

## Short introduction to the Enterprise Procurement Model (EPM) in S/4

EPM Text

## Next Step

If you have made yourself familiar with
- the structure of the workshop,
- your credentials for accessing SAP Data Intelligence
- and the Enterprise Procurement Model (EPM) as the data basis for our Deep Deve demos and Exercises

please continue to the [Deep Dive 1 section](../dd1/README.md).
