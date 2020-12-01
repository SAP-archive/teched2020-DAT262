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
      - Leave all other parameters as-is.<br><br>
   ![](/exercises/ex1/images/ex1-009b.JPG)<br><br>

7.	From the operator list on the left side, drag the ***Terminal*** operator and drop it into the Pipeline canvas. Then connect the output port of the ABAP CDS Reader with the input port of the Terminal operator by pulling the mouse pointer from one port to the other while the left mouse button is pressed.<br><br>
![](/exercises/ex1/images/ex1-010b.JPG)<br><br>

8.	The Terminal displays string type data. Because the output of the ABAP CDS Reader is a message, its body/payload needs to be converted into a string. After you have connected the operator ports you are, hence, prompted for a conversion approach. Select the first of the two options (only the body will be processed) and click ***OK***.<br><br>
![](/exercises/ex1/images/ex1-011b.JPG)<br><br>

9.	The selected converter is now added to the Pipeline. Please click on the ***Auto Layout*** button to arrange the Pipeline nodes.<br><br>
![](/exercises/ex1/images/ex1-012b.JPG)<br><br>

10.	***Save*** your Pipeline.
      - Click on the Disk symbol in the menue bar.<br><br>
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

13.	You should now see that EPM Customer master data is coming in, which proves that the integration with the S/4HANA CDS View is working as expected. By the way: the truncation of payload content can be determined by the ***Max size (bytes)*** parameter in the Terminal operator configuration.<br><br>
![](/exercises/ex1/images/ex1-017b.JPG)<br><br>

14.	***Stop*** the Pipeline execution again by clicking the marked icons in the menue bar or in the status section. The status of the Pipeline will then change from ***running*** to ***stopping*** and finally to ***completed***.<br><br>
![](/exercises/ex1/images/ex1-018b.JPG)<br><br>

**Congratulations!** You have now successfully implemented a Pipeline that consumes an ABAP CDS View from the connected SAP S/4HANA system.
As a next step, you will persist the EPM Customer master data in an S3 Object Store and switch from the Initial Load to a Delta Load transfer mode.

## Exercise 1.2 - Extend the Pipeline to transfer the Customer data into an S3 Object Store with Initial Load and Delta Load modes

After completing the following steps you will have extended the Data Intelligence Pipeline with an additional persistency for the data in S3. And in order to also obtain any deltas that have occurred on the S/4HANA EPM Business Partner table `SNWD_BPA`, the transfer mode will get changed to Delta Load after initially loading the data into a file in S3.

1. If not already done, open the Pipeline from the previous section (**`teched.XXXX.EPM_Customer_Replication_to_S3`**, where XXXX is your user name). Click on the Terminal operator and then the "waste bin" icon in order to delete the Terminal operator. Do the same for the Converter operator. Just keep the the ABAP CDS Reader operator in the Pipeline canvas.<br><br>
![](/exercises/ex1/images/ex1-019b.JPG)<br><br>

2. Make sure that the ***Operators*** tab is in scope in the Modeler UI (see left side). From the list of operators, drag the ***Wiretap*** operator and drop it into the Pipeline canvas. Similar to a Terminal operator, this operator can wiretap a connection between two operators in a Pipeline and display the traffic to the browser window (or to an external websocket client that connects to this operator). But the Wiretap operator also supports throughput of type *message* (amongst others), so that no type conversion is needed between the ABAP CDS Reader and the Wiretap operator.<br>
Now connect the **output port of the ABAP CDS Reader** with the **input port of the Wiretap operator** by pulling the mouse pointer from one port to the other while the left mouse button is pressed.<br><br>
![](/exercises/ex1/images/ex1-020b.JPG)<br><br>

3. From the list of operators, drag the ***Write File*** operator and drop it in the Pipeline canvas. Then connect the **output port of the Wiretap operator** with the **input port of the Write File operator** by pulling the mouse pointer from one port to the other while the left mouse button is pressed.<br><br>
![](/exercises/ex1/images/ex1-021b.JPG)<br><br>

4. The message format from the Wiretap operator output must be transformed to a file format. For this reason you are prompted to choose an appropriate converter operator. On the pop-up window, select the first option (transfer the content). Click ***OK***.<br><br>
![](/exercises/ex1/images/ex1-022b.JPG)<br><br>

5. Click on the ***Write File*** operator and click its ***configuration*** icon.<br><br>
![](/exercises/ex1/images/ex1-023b.JPG)<br><br>

6. Enter the needed configuration parameters for the ***Write File*** operator. These are:
   - Label: **`Customer Master to S3`**
   - Connection: Choose type ***Connection Management*** and then the connection ID **`TechEd2020_S3`**
   - Path mode: **`Static (from configuration)`**
   - Path: **`/DAT262/XXXX/Customer_Master.csv`**, where XXXX is your User name, for example "/DAT262/TA99/Customer_Master.csv"
   - Mode: **`Append`**
   - Join batches: **`True`**<br><br>
![](/exercises/ex1/images/ex1-024b.JPG)<br><br>

7. Now ***Save*** your Pipeline, verify the validation results and - if okay - run the Pipeline by clicking on the ***Play*** symbol in the menue bar.<br><br>
![](/exercises/ex1/images/ex1-025b.JPG)<br><br>

8. Once the Pipeline has the status ***running***, click on the ***Wiretap*** operator and then click its ***Open UI*** icon.<br><br>
![](/exercises/ex1/images/ex1-026b.JPG)<br><br>

9. In the ***Wiretap UI*** you should now see the processed messages coming from the ABAP CDS Reader.<br><br>
![](/exercises/ex1/images/ex1-027b.JPG)<br><br>

10. ***Stop*** the Pipeline.<br><br>
![](/exercises/ex1/images/ex1-028b.JPG)<br><br>

11. As a verification of the successful load of the data to the file in S3, we can use the ***Metadata Explorer*** as integrated part of SAP Data Intelligence. Please go back to the Launchpad and click on the corresponding tile.<br><br>
![](/exercises/ex1/images/ex1-029b.JPG)<br><br>

12. In the ***Metadata Explorer*** application main menue, click on ***Browse Connections***.<br><br>
![](/exercises/ex1/images/ex1-030b.JPG)<br><br>

13. In order to easily find our connection to the target S3 Object Store, you may leverage the search functionality. Enter `TechEd` into the search field and click on the spyclass icon. Click on the **`TechEd2020_S3`** tile.<br><br>
![](/exercises/ex1/images/ex1-031b.JPG)<br><br>

14. On the next drill-down view, click on the **`DAT262`** directory that you had specified in the ***Write File*** operator, and then drill down to your specific User directory, for example **`TA99`**.<br><br>
![](/exercises/ex1/images/ex1-032b.JPG)<br><br>

15. If your Pipeline ran successfully, you'll find a file with your specified name (`Customer_Master.csv`) Open the ***More Actions*** menue and select ***View Fact Sheet***.<br><br>
![](/exercises/ex1/images/ex1-033b.JPG)<br><br>

16. In the ***Fact Sheet***, which provides some more overview and statistical information about the new file, go to tab ***Data Preview***.<br><br>
![](/exercises/ex1/images/ex1-034b.JPG)<br><br>

17. Now you can see that the EPM Customer data got loaded into the target file in S3. Success!<br><br>
![](/exercises/ex1/images/ex1-035b.JPG)<br><br>

In order to fetch any changes in S/4HANA on the Business Partner table `SNWD_BPA`, we can leverage the delta enablement in the ABAP CDS View that we had conducted during the Deep Dive 1 demo by adding the `@Analytics`annotation below:<br>
```abap
@Analytics:{
    dataExtraction: {
        enabled: true,
        delta.changeDataCapture.automatic: true
    }
}
```
<br>

17. Go back to the Modeler application and your Pipeline. Click on the ***ABAP CDS Reader*** operator and then on its ***configuration*** icon. In the configuration panel, change the entry for the ***Transfer mode*** parameter from `Initial Load`--> `Delta Load`.<br><br>
![](/exercises/ex1/images/ex1-036b.JPG)<br><br>

18. ***Save*** the Pipeline and ***Run*** it again. As long as the Pipeline stays in a ***running*** status, it will fetch any updates on the Business Partner table of the EPM demo application in S/4HANA by leveraging the Change Data Capturing (CDC) functionality in S/4HANA ABAP CDS Views. Your target persistency in S3 is instantly and automatically kept updated.<br><br>
![](/exercises/ex1/images/ex1-037b.JPG)<br><br>

**Very well done!** You have implemented a Pipeline that extracts Initial Load and Delta data fron an ABAP CDS View in S/4HANA and have interlinked it with a non-SAP target storage in S3.

## Exercise 1.3 - Implement a Pipeline for delta transfer of enhanced EPM Sales Order data from S/4HANA to an S3 Object Store

In the next section, we'll also take care for the Sales Order transaction data from EPM and will right away establish a replication (initial load plus following delta processing) transfer mode.

1. In SAP Data Intelligence, open the ***Modeler*** application. Make sure the scope of the Modeler UI is on tab ***Graphs*** (see left side). Then click the ***+*** button to create a new Pipeline.<br><br>
![](/exercises/ex1/images/ex1-004b.JPG)<br><br>

2. Now a new Pipeline canvas is opened on the right side and the Modeler UI automatically switches the scope to the ***Operators*** tab (see left side). In the list of operators, drag the ABAP CDS Reader and drop it into the Pipeline canvas. Click the ABAP CDS Reader node in the canvas one time and then click the ***configuration*** icon.<br><br>
![](/exercises/ex1/images/ex1-006b.JPG)<br><br>

3.	In the configuration panel for the ABAP CDS Reader, specify the S/4HANA Connection: Select `S4_RFC_TechEd`. Then click on the ***Version*** selection icon.<br><br>
![](/exercises/ex1/images/ex1-007b.JPG)<br><br>

4.	In the pop-up window, select the option ***ABAP CDS Reader V2*** and click ***OK***.<br><br>
![](/exercises/ex1/images/ex1-008b.JPG)<br><br>

6.	The ABAP CDS Reader operator shell has received the operator's metadata from S/4HANA and has configured the Pipeline node (for example, the operator now has an output port). Now fill in the configuration parameters in the panel:
      - Label: `Sales Order - S/4 ABAP CDS`
      - Subscription Type: `New`
      - Subscription Name: `SO001_XXXX`, where XXXX is your user name, for example "BP002_TA99"
      - ABAP CDS Name: `Z_CDS_SO_SOI_Delta`, which is the "more complex" CDS View that got created in the Deep Dive 1 demo
      - Transfer Mode: `Replication`
      - Leave all other parameters as-is.<br><br>
   ![](/exercises/ex1/images/ex1-038b.JPG)<br><br>
   
7.	From the operator list on the left side, drag the ***Wiretab*** operator and drop it into the Pipeline canvas. Then connect the output port of the ABAP CDS Reader with the input port of the Wiretab operator by pulling the mouse pointer from one port to the other while the left mouse button is pressed.<br><br>
![](/exercises/ex1/images/ex1-039b.JPG)<br><br>

8. From the list of operators, drag the ***Write File*** operator and drop it in the Pipeline canvas. Then connect the **output port of the Wiretap operator** with the **input port of the Write File operator** by pulling the mouse pointer from one port to the other while the left mouse button is pressed.<br><br>
![](/exercises/ex1/images/ex1-040b.JPG)<br><br>

9. The message format from the Wiretap operator output must be transformed to a file format. For this reason you are prompted to choose an appropriate converter operator. On the pop-up window, select the first option (transfer the content). Click ***OK***.<br><br>
![](/exercises/ex1/images/ex1-041b.JPG)<br><br>

10. Click on the ***Write File*** operator and click its ***configuration*** icon.<br><br>
![](/exercises/ex1/images/ex1-042b.JPG)<br><br>

11. Enter the needed configuration parameters for the ***Write File*** operator. These are:
   - Label: **`Sales Order to S3`**
   - Connection: Choose type ***Connection Management*** and then the connection ID **`TechEd2020_S3`**
   - Path mode: **`Static (from configuration)`**
   - Path: **`/DAT262/XXXX/Sales_Order.csv`**, where XXXX is your User name, for example "/DAT262/TA99/Sales_Order.csv"
   - Mode: **`Append`**
   - Join batches: **`True`**<br><br>
![](/exercises/ex1/images/ex1-043b.JPG)<br><br>

12.	***Save*** your Pipeline.
      - Click on the Disk symbol in the menue bar.<br><br>
      - Because you save the Sales Order Pipeline for the first time, you are prompted for some inputs.<br>
      As Name, enter **`teched.XXXX.EPM_Customer_Replication_to_S3`**, where **XXXX** is your user name, for example "teched.TA99.EPM_Customer_Replication_to_S3".<br>
      As Description, please enter **`XXXX - Replicate S/4HANA EPM Customer Data to S3`**,where **XXXX** is your user name, for example "TA99 - Replicate S/4HANA EPM Customer         Data to S3". (The description will show up in the Pipeline status information later on.)<br>
      As Catergory, enter **`dat262`**, which is the name under which you can find your Pipeline in the ***Graphs*** tab.<br>
      Click ***OK***.<br><br>
      ![](/exercises/ex1/images/ex1-014b.JPG)<br><br>

11.	After you have saved the Pipeline, it will get validated by SAP Data Intelligence. Check the validation results. If okay, you can now execute the Pipeline by clicking the ***Play*** icon in the menue bar. Then change to the ***Status*** tab in the Modeler UI status section on the lower right side.<br><br>
![](/exercises/ex1/images/ex1-015b.JPG)<br><br>

12.	Once the status of your Pipeline has changed to ***running***, click on the ***Terminal*** operator node one time and then on its ***Open UI*** icon.<br><br>
![](/exercises/ex1/images/ex1-016b.JPG)<br><br>



12. Now ***Save*** your Pipeline, verify the validation results and - if okay - run the Pipeline by clicking on the ***Play*** symbol in the menue bar.<br><br>
![](/exercises/ex1/images/ex1-025b.JPG)<br><br>

13. Once the Pipeline has the status ***running***, click on the ***Wiretap*** operator and then click its ***Open UI*** icon.<br><br>
![](/exercises/ex1/images/ex1-026b.JPG)<br><br>

14. In the ***Wiretap UI*** you should now see the processed Sales Order messages coming from the ABAP CDS Reader.<br><br>
![](/exercises/ex1/images/ex1-027b.JPG)<br><br>


## Exercise 1.4 - Extend the Pipeline for joining Sales Order with Customer data for each change in Sales Orders and persist results in S3





## Summary

You've now ...

Continue to - [Exercise 2 - Exercise 2 Description](../ex2/README.md)

