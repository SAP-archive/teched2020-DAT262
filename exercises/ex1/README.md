# Exercise 1 - Replicating data from ABAP CDS Views in SAP Data Intelligence

In this exercise, we will leverage the ABAP CDS Views that got created during the first Deep Dive demo in the connected S/4HANA system in a Pipeline.<br><br>
The use case is
- to obtain the Business Partner master data in S/4HANA's demo application **Enterprise Procurement Model (EPM)** and make the records available for the corporate Data Analysts in an S3 object store.
- to also persist the transactional data for EPM Sales Orders in S3.
- In both cases, any single change of these data sources in the S/4HANA system has to be instantly and automatically replicated to the related files in S3 in append mode.
- Additionally, the Sales Order data have to be enriched with Customer master data. For the initial load and then on every change committed to the EPM Sales Order data in S/4HANA.

(As a reminder: You can recap the relationship between the relevant EPM table entities that are used in this exercise [here](../ex0#short-introduction-to-the-enterprise-procurement-model-epm-in-s4))<br>

## Exercise 1.1 - Consume the EPM Business Partner ABAP CDS Views in SAP Data Intelligence

After completing these steps you will have created a Pipeline that reads EPM Customer data from an ABAP CDS View in S/4HANA and displays it in a Terminal UI.

1. Log on to SAP Data Intelligence and enter the Launchpad application. Then start the ***Modeler*** application.
- Follow the link to your assigned Data Intelligence instance, e.g. https://vsystem.ingress.xyz.dhaas-live.shoot.live.k8s-hana.ondemand.com/app/datahub-app-launchpad/.
- In the pop-up window, enter ***"default"*** as the tenant name.
<br>![](/exercises/ex1/images/ex1-001b.JPG)<br><br>
- In the next pop-up window, enter you assigned user name (e.g. ***"TA99"***) and your password (initially ***"Welcome01"***).
<br>![](/exercises/ex1/images/ex1-002b.JPG)<br><br>
- From the Launchpad, start the ***Modeler*** application by clicking on the corresponding tile.
<br>![](/exercises/ex1/images/ex1-003b.JPG)<br><br>

2.	Make sure you are in the ***Graphs*** tab of the Modeler UI (see left side). Then click the ***+*** symbol in order to create a new Pipeline.<br>
<br><br>![](/exercises/ex1/images/ex1-004b.JPG)<br><br>

3.	Now a new Pipeline canvas is opened on the right side and the Modeler UI automatically switches to the ***Operators*** tab (see left side). In the list of operators, drag the ABAP CDS Reader and drop it into the Pipeline canvas. Click the ABAP CDS Reader node in the canvas one time and then click the ***configuration*** icon.<br>
<br><br>![](/exercises/ex1/images/ex1-006b.JPG)<br><br>



## Exercise 1.2 Sub Exercise 2 Description

After completing these steps you will have...

1.	Enter this code.


2.	Click here.
<br>![](/exercises/ex1/images/01_02_0010.png)


## Summary

You've now ...

Continue to - [Exercise 2 - Exercise 2 Description](../ex2/README.md)

