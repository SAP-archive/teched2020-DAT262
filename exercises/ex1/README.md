# Exercise 1 - Replicating data from ABAP CDS Views in SAP Data Intelligence

In this exercise, we will leverage in Data Intelligence Pipelines those ABAP CDS Views that got created during the first Deep Dive demo in the connected S/4HANA system.<br><br>
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
   - In the pop-up window, enter ***"default"*** as the tenant name.<br><br>
   ![](/exercises/ex1/images/ex1-001b.JPG)<br><br>
   - In the next pop-up window, enter you assigned user name (e.g. ***"TA99"***) and your password (initially ***"Welcome01"***).<br><br>
   ![](/exercises/ex1/images/ex1-002b.JPG)<br><br>
   - From the Launchpad, start the ***Modeler*** application by clicking on the corresponding tile.<br><br>
   ![](/exercises/ex1/images/ex1-003b.JPG)<br><br>

2.	Make sure you are in the ***Graphs*** tab of the Modeler UI (see left side). Then click the ***+*** symbol in order to create a new Pipeline.<br><br>
![](/exercises/ex1/images/ex1-004b.JPG)<br><br>

3.	Now a new Pipeline canvas is opened on the right side and the Modeler UI automatically switches to the ***Operators*** tab (see left side). In the list of operators, drag the ABAP CDS Reader and drop it into the Pipeline canvas. Click the ABAP CDS Reader node in the canvas one time and then click the ***configuration*** icon.<br><br>
![](/exercises/ex1/images/ex1-006b.JPG)<br><br>

4.	In the configuration panel for the ABAP CDS Reader, specify the S/4HANA Connection: Select `S4_RFC_TechEd`. Then click on the ***Version*** selection icon.<br><br>
![](/exercises/ex1/images/ex1-007b.JPG)<br><br>

5.	In the pop-up window, mark the different versions of the ABAP CDS Reader and read through their documentation. As you can see, the ***ABAP CDS Reader V2*** provides a standard message type output (no conversion from ABAP object required). select this entry and click ***OK***.<br><br>
![](/exercises/ex1/images/ex1-008b.JPG)<br><br>

6.	The ABAP CDS Reader operator shell has received the operator's metadata from S/4HANA and has configured the Pipeline node (for example, the operator now has an output port). Now fill in the configuration parameters in the panel:
      - Label: `Buyer - S/4 ABAP CDS`
      - Subscription Type: `New`
      - Subscription Name: `BP001_XXXX`, where XXXX is your user name, for example "BP001_TA99"
      - ABAP CDS Name: `Z_CDS_BUYER_Delta`, which is the "simple" CDS View that got created in the Deep Dive 1 demo
      - Transfer Mode: `Initial Load`
      - Leave all other parameters as-is.<br>
![](/exercises/ex1/images/ex1-009b.JPG)<br><br>

7.	From the operator list on the left side, drag the ***Terminal*** operator into the Pipeline canvas. Then connect the output port of the ABAP CDS Reader with the input port of the Terminal operator by pulling the mouse pointer from one port to the other while the left mouse button is pressed.<br><br>
![](/exercises/ex1/images/ex1-010b.JPG)<br><br>

8.	The Terminal displays string type data. Because the output of the ABAP CDS Reader is a message, its body/payload needs to be converted into a string. After you have connected the operator ports you are, hence, prompted for a conversion approach. Select the first of the two options (only the body will be processed) and click ***OK***.<br><br>
![](/exercises/ex1/images/ex1-011b.JPG)<br><br>

9.	The selected converter is now added to the Pipeline. Please click on the ***Auto Layout*** button to arrange the Pipeline nodes.<br><br>
![](/exercises/ex1/images/ex1-012b.JPG)<br><br>

10.	***Save*** your Pipeline.
      - click on the Disk symbol in the menue bar.<br><br>
      ![](/exercises/ex1/images/ex1-013b.JPG)<br><br>
      - Because you save the Pipeline for the first time, you are prompted for some inputs.<br>
      As Name, enter **`teched.XXXX.EPM_Customer_Replication_to_S3`**, where **XXXX** is your user name, for example "teched.TA99.EPM_Customer_Replication_to_S3".<br>
      As Description, please enter **`XXXX - Replicate S/4HANA EPM Customer Data to S3`**,where **XXXX** is your user name, for example "TA99 - Replicate S/4HANA EPM Customer         Data to S3". (The description will show up in the Pipeline status information later on.)<br>
      As Catergory, enter **`dat262`**, which is the name under which you can find your Pipeline in the ***Graphs*** tab.<br>
      Click ***OK***.<br><br>
      ![](/exercises/ex1/images/ex1-014b.JPG)<br><br>

11.	After you have saved the Pipeline, it will get validated by SAP Data Intelligence. Check the validation results. If okay, you can now execute the Pipeline by clicking the ***Play*** icon in the menue bar. Then change to the ***Status*** tab in the Modeler UI status section on the lower right side.<br><br>
![](/exercises/ex1/images/ex1-015b.JPG)<br><br>

12.	Once the status of your Pipeline has changed to ***running***, click on the ***Terminal*** operator node one time and then on its ***Open UI*** icon.<br><br>
![](/exercises/ex1/images/ex1-016b.JPG)<br><br>

13.	You should now see that EPM Customer master data coming in, which proofs that the integration with the S/4HANA CDS View is working as expected. By the way: the truncation of payload content can be determined by the ***Max size (bytes)*** parameter in the Terminal operator configuration.<br><br>
![](/exercises/ex1/images/ex1-017b.JPG)<br><br>

14.	***Stop*** the Pipeline execution again by clicking the marked icons in the menue bar or in the status section. The status of the Pipeline will then change from ***running*** to ***stopping*** and finally to ***completed***.<br><br>
![](/exercises/ex1/images/ex1-018b.JPG)<br><br>

**Congratulations!** You have now successfully implemented a Pipeline that consumes an ABAP CDS View from the connected SAP S/4HANA system.
As a next step, you will persist the EPM Customer master data in an S3 Object Store and switch from the Initial Load to a Delta Load transfer mode.

## Exercise 1.2 - Extend the Pipeline to transfer the Customer data into an S3 Object Store with Initial Load and Delta Load modes

After completing the following steps you will have extended the Data Intelligence Pipeline with an additional persistency for the data in S3. And in order to also obtain any deltas that have occurred on the S/4HANA EPM Business Partner table `SNWD_BPA`, the transfer mode will get changed to Delta Load after initially loading the data into a file in S3.

1. If not already done, open the Pipeline from the previous section (**`teched.XXXX.EPM_Customer_Replication_to_S3`**). Click on the Terminal operator and then the "waste bin" icon in order to delete the Terminal operator. Do the same for the Converter operator. Just keep the the ABAP CDS Reader operator in the Pipeline canvas.<br><br>
![](/exercises/ex1/images/ex1-019b.JPG)<br><br>

2. Make sure that the ***Operators*** tab is in scope in the Modeler UI (see left side). From the list of operators, drag the ***Wiretap*** operator and drop it in the Pipeline canvas. Similar to a Terminal operator, this operator can wiretap a connection between two operators in a Pipeline and display the traffic to the browser window (or to an external websocket client that connects to this operator). But the Wiretap operator also supports throughput of type *message* (amongst others), so that no type conversion is needed between the ABAP CDS Reader and the Wiretap operator.<br>
Now connect the **output port of the ABAP CDS Reader** with the **input port of the Wiretap operator** by pulling the mouse pointer from one port to the other while the left mouse button is pressed.<br><br>
![](/exercises/ex1/images/ex1-020b.JPG)<br><br>

3. If not already done, open the Pipeline from the previous section (**`teched.XXXX.EPM_Customer_Replication_to_S3`**). Click on the Terminal operator and then the "waste bin" icon in order to delete the Terminal operator. Do the same for the Converter operator. Just keep the the ABAP CDS Reader operator in the Pipeline canvas.<br><br>
![](/exercises/ex1/images/ex1-019b.JPG)<br><br>


## Summary

You've now ...

Continue to - [Exercise 2 - Exercise 2 Description](../ex2/README.md)

