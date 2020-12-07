# Exercise 1 - Replicating data from ABAP CDS Views in SAP Data Intelligence

In this exercise, we will leverage in Data Intelligence Pipelines those ABAP CDS Views that got created during the first Deep Dive demo in the connected S/4HANA system.<br><br>
The use case is
- to obtain the Business Partner master data in S/4HANA's demo application **Enterprise Procurement Model (EPM)** and make the records available for the corporate Data Analysts in an S3 object store.
- to also persist the transactional data for EPM Sales Orders in S3.
- In both cases, any single change of these data sources in the S/4HANA system has to be instantly and automatically replicated to the related files in S3 in append mode.
- Additionally, the Sales Order data have to be enriched with Customer master data. For the initial load and then on every change committed to the EPM Sales Order data in S/4HANA.

(As a reminder: You can recap the relationship between the relevant EPM table entities that are used in this exercise [here](../ex0#short-introduction-to-the-enterprise-procurement-model-epm-in-sap-s4hana))<br>

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
      - Click on the Disk symbol in the menue bar.
      - Because you save the Sales Order Pipeline for the first time, you are prompted for some inputs.<br>
      - As Name, enter **`teched.XXXX.EPM_SalesOrder_Replication_to_S3`**, where **XXXX** is your user name, for example "teched.TA99.EPM_SalesOrder_Replication_to_S3".<br>
      - As Description, please enter **`XXXX - Replicate S/4HANA EPM Customer Data to S3`**,where **XXXX** is your user name, for example "TA99 - Replicate S/4HANA EPM Sales  Order Data to S3". (The description will show up in the Pipeline status information later on.)<br>
      - As Catergory, enter **`dat262`**, which is the name under which you can find your Pipeline in the ***Graphs*** tab.<br>
      - Click ***OK***.<br><br>
      ![](/exercises/ex1/images/ex1-044b.JPG)<br><br>

13.	After you have saved the Pipeline, it will get validated by SAP Data Intelligence. Check the validation results. If okay, you can now execute the Pipeline by clicking the ***Play*** icon in the menue bar. Then change to the ***Status*** tab in the Modeler UI status section on the lower right side.<br><br>
![](/exercises/ex1/images/ex1-045b.JPG)<br><br>

14.	Once the status of your Pipeline has changed to ***running***, click on the ***Wiretap*** operator node one time and then on its ***Open UI*** icon.<br><br>
![](/exercises/ex1/images/ex1-046b.JPG)<br><br>

15. In the ***Wiretap UI*** you should now see the processed Sales Order messages coming from the ABAP CDS Reader. This proves that the integration with the S/4HANA CDS View is working as expected.<br><br>
![](/exercises/ex1/images/ex1-047b.JPG)<br><br>

16. For validating the correct creation of the file in S3, please leverage the Data Intelligence Metadata Explorer again. Go back to the Launchpad and start the Metadata Explorer application, if not already launched from the previous exercise.<br><br>

17. In the ***Metadata Explorer*** application main menue, click on ***Browse Connections***.<br><br>
![](/exercises/ex1/images/ex1-030b.JPG)<br><br>

18. In order to easily find our connection to the target S3 Object Store, you may leverage the search functionality. Enter `TechEd` into the search field and click on the spyclass icon. Click on the **`TechEd2020_S3`** tile.<br><br>
![](/exercises/ex1/images/ex1-031b.JPG)<br><br>

19. On the next drill-down view, click on the **`DAT262`** directory that you had specified in the ***Write File*** operator, and then drill down to your specific User directory, for example **`TA99`**.<br><br>
![](/exercises/ex1/images/ex1-032b.JPG)<br><br>

20. If your Pipeline ran successfully, you'll find a file with your specified name (`Sales_Order.csv`) Open the ***More Actions*** menue and select ***View Fact Sheet***.<br><br>
![](/exercises/ex1/images/ex1-048b.JPG)<br><br>

21. In the ***Fact Sheet***, which provides some more overview and statistical information about the new file, go to tab ***Data Preview***.<br><br>
![](/exercises/ex1/images/ex1-049b.JPG)<br><br>

22. Success! Now you can see that the EPM Customer data got loaded into the target file in S3. While the Pipeline is running, this file would get automatically updated with each change in the S/4HANA data sources (the tables `SNWD_SO`, `SNWD_SO_I`, `SNWD_PD`, `SNWD_TEXTS` joined through the ABAP CDS View `Z_CDS_SO_SOI_Delta`).<br><br>
![](/exercises/ex1/images/ex1-050b.JPG)<br><br>

21. However, in order to go on with the exercise, please stop the Pipeline for now. We will validate the delta processing in Exercise 2.<br><br>
![](/exercises/ex1/images/ex1-051b.JPG)<br><br>

**Congratulations!** You have created the Sales Order extraction from a delta-enabled, more complex ABAP CDS View into the S3 Object Store. Because you have chosen the transfer mode ***"Replication"*** in the CDS Reader operator configuration, the Pipeline has conducted the Initial Load and would now wait for any changes (inserts, updates, deletions) in the S/HANA EPM Sales Order object as long as it is running.

As a next step, you will enrich the Sales Order data with some Customer Details (Name and Legal Form) during the Replication process.


## Exercise 1.4 - Extend the Pipeline for joining Sales Order with Customer data for each change in Sales Orders and persist results in S3

In this last part of the S/4HANA ABAP CDS View intergration exercise, you will establish a Pipeline that replicated the Sales Order data from the delta-enabled ABA CDS View in S/4HANA and joins it with some of the details that you have replicated from the Customer master data, i.e. company name and legal form.

1. In SAP Data Intelligence, open the ***Modeler*** application. Make sure the scope of the Modeler UI is on tab ***Graphs*** (see left side). If not still open from the previous exercise part, search for your Sales Order Pipeline by entering your user name into the search field on top of the list of Pipelines. Click on Pipeline with the label `XXXX - Replicate S/4HANA EPM Sales  Order Data to S3`, where XXXX is your user name, for example "TA99 - Replicate S/4HANA EPM Sales  Order Data to S3". The Pipeline will get opened then or will get the focus on the UI if it was alraedy opened. If you cannot see the complete label of the Pipeline, just hover with your mouse over the pipeline item in the list.<br><br>
![](/exercises/ex1/images/ex1-052b.JPG)<br><br>

2. Click on the pull-down menue of the ***Save*** icon (disk symbol) and choose ***Save As***.<br><br>
![](/exercises/ex1/images/ex1-053b.JPG)<br><br>

3. You are prompted for some inputs. Please enter the following values.
    - As Name, enter **`teched.XXXX.EPM_SalesOrder_Replication_Enrich_to_S3`**, where **XXXX** is your user name, for example "teched.TA99.EPM_SalesOrder_Replication_Enrich_to_S3".<br>
    - As Description, please enter **`XXXX - Replicate and Enrich S/4HANA EPM Sales Order Data to S3`**,where **XXXX** is your user name, for example "TA99 - Replicate and Enrich S/4HANA EPM Sales Order Data to S3". (The description will show up in the Pipeline status information later on.)<br>
    - As Catergory, enter **`dat262`**, which is the name under which you can find your Pipeline in the ***Graphs*** tab.<br>
    - Then click ***OK***.<br><br>
![](/exercises/ex1/images/ex1-054b.JPG)<br><br>

4. A new Pipeline has been created as a copy of your original Sales Order replication Pipeline. You will be using the File Operator outputs (i.e. the file path and name) as a trigger for the join processes with the Customer master data.<br><br>

5. Open the configuration of the File Writer operator and enter the new target path/file name **`/DAT262/TA99/Sales_Order_Staging.csv`**. Change Mode to **`Overwrite`**. Then save the Pipeline.<br><br>
![](/exercises/ex1/images/ex1-055b.JPG)<br><br>

6. Click on the ***Operators*** tab on the left side in order to get the list of operators. Find the ***Graph Terminator*** operator and drag it from the operator list into the canvas. Then connect the upper output port ("file") of the File Writer with the input port of the Graph Terminator operator.<br><br>
The Graph Terminanor allows us to run the Pipeline once, and when the new file got generated, the File Writer will send a termination signal to the Graph Terminator, which then stops the Pipeline. We do this just for the purpose of generating the new output file as a reference / import sample for the next steps.<br><br>
![](/exercises/ex1/images/ex1-056b.JPG)<br><br>

7. ***Save*** and ***Run*** the Pipeline. Wait until the Pipeline status turns from ***pending*** to ***processing*** and finally to ***completed***.<br><br>

8. After the completion of the Pipeline execution, **remove the Graph Terminator** operator from the Pipeline again.<br><br>
![](/exercises/ex1/images/ex1-057b.JPG)<br><br>

9. From the list of operators on the left side, drag the ***Structured File Consumer*** operator and drop it into the Pipeline canvas. Then open the configuration panel and do the following entries:
   - **Storage Type**: `S3`
   - **S3 Configuration**: After clicking on the pencil icon, select `Configuration Manager`and as connection ID `TechEd2020_S3`<br><br>
   - Increase the **Fetch size** to `10000`
   - **Fail on string truncation** should be `False`<br><br>
     ![](/exercises/ex1/images/ex1-058b.JPG)<br><br>
   - Then click the **S3 Source File** folder icon to browse through the S3 bucket. Drill down to your individual folder under **DAT262** and select the file **`Sales_Order_Staging`**. Then click ***OK***.<br><br>
     ![](/exercises/ex1/images/ex1-060b.JPG)<br><br>
     
10. You are now able to open the ***Data Preview*** of the connected file in S3. Give it a try, if you want!<br>
Now connect the **upper output port ("file") of the File Writer** operator with the **input port of the Structured File Consumer** operator. The ***Structured File Consumer*** operator takes the signal on the input port just as a trigger for commencing its logic. It's an optional input but our approach ensures that the operator is only executed after the file from the previous node has been successfully written on S3. (If the input port of a Structured File Consumer is not connected, the operator will start with the Pipeline execution.)<br>
When you create the link between the operators, a conversion of the data type is required from type `message.file` to type `message`. The ***Converter*** is automatically proposed when the link between the ports is established. Choose the option for ***Path extraction*** (which reflects the minimum transfer payload, as we use the input just as a trigger signal).<br><br>
![](/exercises/ex1/images/ex1-061b.JPG)<br><br>
![](/exercises/ex1/images/ex1-062b.JPG)<br><br>

11. We still need another ***Structured File Consumer*** operator for the Customer master data extraction from S3. In order to save typing efforts, just copy the existing operator:<br>
   - Click on the ***Structured File Consumer*** operator
   - Press <CTRL+C> on your keyboard
   - Press <CTRL+V> on your keyboard
A new copy of the operator is being included in the Pipeline canvas.<br><br>
![](/exercises/ex1/images/ex1-063b.JPG)<br><br>

12. On this new operator, open the configuration panel and click the **S3 Source File** folder icon to browse through the S3 bucket. Drill down to your individual folder under **DAT262** and select the file **`Customer_Master.csv`**. Then click ***OK***..<br><br>
![](/exercises/ex1/images/ex1-064b.JPG)<br><br>
![](/exercises/ex1/images/ex1-065b.JPG)<br><br>

13. From the list of operators on the left side, drag the ***Data Transform*** operator and drop it into the Pipeline canvas. Then connect the ***output ports of the Structured File Consumers*** with the ***Data Transform box***. This will create the needed input ports of the Data Transform operator.<br><br>
![](/exercises/ex1/images/ex1-066b.JPG)<br><br>

14. Double-click on the ***Data Transform*** operator.<br><br>
![](/exercises/ex1/images/ex1-067b.JPG)<br><br>

15. In the details view, from the list of Data Operations on the left side, drag the ***Join*** operation into the canvas with the existing two ***input nodes*** and connect these with the ***Join*** operation.<br><br>
![](/exercises/ex1/images/ex1-068b.JPG)<br><br>

16. Double-click on the ***Join*** operation.<br><br>
![](/exercises/ex1/images/ex1-069b.JPG)<br><br>

17. Re-arrange the position of input tables on the canvas as per your convenience and then connect the **"C5" field of Join_Input1** (Sales Order) with the **"C0" field of Join_Input2** (Customer). This will map the Customer GUID fields of the two tables. We're using an INNER JOIN.<br><br>
![](/exercises/ex1/images/ex1-070b.JPG)<br><br>

18. Proceed to tab ***Columns*** in order to select the output fields. Click on the ***Auto map by name*** icon (see red box), which enter all fields of the two input nodes to the list of output fields. Then mark the buttom occurrences of the output fields "C0" to "C7" (of Join_Input2!) and delete these.<br><br>
![](/exercises/ex1/images/ex1-073b.JPG)<br><br>

19. Now mark the buttom occurrences of the output fields "C10" to "C18" (of Join_Input2!) and delete these.<br><br>
![](/exercises/ex1/images/ex1-074b.JPG)<br><br>

20. Click on the ***pencil*** icon in the line of field "C8" in order to edit the entry. Rename **`C8`** to **`CUSTOMER_NAME`**. Click on the white space in the the form, then ***OK***.<br><br>
![](/exercises/ex1/images/ex1-075b.JPG)<br><br>

21. Click on the ***pencil*** icon in the line of field "C9" in order to edit the entry. Rename that field from **`C9`** to **`LEGAL_FORM`**. Click on the white space in the the form, then ***OK***. (Renaming the fields was necessary in order to avoid double names).<br><br>
![](/exercises/ex1/images/ex1-076b.JPG)<br><br>

22. Navigate back.<br><br>
![](/exercises/ex1/images/ex1-077b.JPG)<br><br>

23. Click with the right mouse button on the output port of the ***Join*** operation. Then click on ***Create Data target***.<br><br>
![](/exercises/ex1/images/ex1-078b.JPG)<br><br>

24. A new output node has been created, which will now be available on the Data Transfer operator. Navigate further back.<br><br>
![](/exercises/ex1/images/ex1-079b.JPG)<br><br>

25. Back on the Pipeline canvas, drag the ***Structured File Producer*** operator from the list of operators into the Pipeline canvas. Then connect the ***output port of the Data Transform*** with the ***input port of the Structured File Producer***.<br><br>
![](/exercises/ex1/images/ex1-080b.JPG)<br><br>

26. Open the configuration panel of the ***Structured File Producer*** operator and enter the following parameters:<br>
    - **Storage Type**: `S3`
    - **S3 Configuration**: After clicking on the pencil icon, select `Configuration Manager`and as connection ID `TechEd2020_S3`
    - **S3 file name:** `/DAT262/TA99/Enriched_Sales_Order.csv`
    - **Format:** `CSV`
    - Leave the **CSV Properties** as-is
    - Increase **Batch size:** to `10000`
    - **Write Part Files** should be `False`<br><br>
    - ***Save*** the Pipeline
    - ***Run*** the Pipeline<br><br>
![](/exercises/ex1/images/ex1-081b.JPG)<br><br>

27. When the Pipeline is in status ***running***, you can take a look into your folder in the S3 bucket, again using the ***Data Intelligence Metadata Explorer*** application. You should see the file that was specified in the *Structured File Producer* operator ("Enriched_Sales_Order.csv"). Click the ***Glasses*** icon on that tile in order to see the ***Data Preview*** of the ***Fact Sheet***.<br><br>
![](/exercises/ex1/images/ex1-082b.JPG)<br><br>

28. As you can see, the two columns with the company name and the legal form of the Customer master data table have been added to the Sales Order data.<br><br>
![](/exercises/ex1/images/ex1-083b.JPG)<br><br>

29. As long as the Pipeline is running, you would now receive any updates in S/4HANA on the EPM Sales Order data, enriched with the lookups of the EPM Customer master data. The file in S3 would look as displayed below. In column "C20", you get the ***update*** indicator "U". Columns "C21" and "C22" contain the enriched field contents.<br><br>
![](/exercises/ex1/images/ex1-084b.JPG)<br><br>

## Summary

**Congrats!** You have now implemented a Pipeline that receives the Initial Loads and the Change Data Capture (CDC) information from different S/4HANA ABAP CDS Views.<br>

As a matter of fact, in this virtual workshop at TechEd, we can't provide access to the SAP GUI of the connected SAP S/4HANA system in order to run the EPM data generation transaction **`SEPM_DG`**.

In the next exercise we will mitigate this problem and allow you to execute a variant of this ABAP Report, leveraging the ABAP Function Module calls from SAP Data Intelligence.
Please continue to [Exercise 2 - Triggering the execution of a function module in a remote S/4HANA system](../ex2/README.md).

