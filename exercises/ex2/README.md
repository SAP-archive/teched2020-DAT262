# Exercise 2 - Triggering the execution of a function module in a remote S/4HANA system

**!!! CURRENTLY BEING REWORKED !!!**

So far, we have missed to proof whether or not our ABAP CDS Views and the consuming Data Intelligence Pipelines are really providing delta information. This would require access to the S/4HANA system in order to conduct changes on the data basis, hence on the Business Partner table or on the Sales Order object.

The Enterprise Procurement Model (EPM) demo application comes with a report that allows you to generate EPM Sales Order data. This report **`SEPM_DG_EPM_STD_CHANNEL`** can be started with the transaction **`SEPM_DG`** in S/4HANA.<br><br>
Here is how the Report looks like in the SAP GUI:<br><br>
![](/exercises/ex2/images/ex2-000b.JPG)<br><br>

As a matter of fact, we couldn't provide an SAP GUI based access to the connected S/4HANA system. For still being able to trigger changes on EPM data in that system, we have created a variant of the above report **`SEPM_DG_EPM_STD_CHANNEL`** and a Custom ABAP Operator **`customer.teched.socreate`** which executes this report variant.<br><br>
FYI, the following lines in the Local Class `lcl_process`, which instantiated by the `NEW_PROCES( )` method of the Operator Class `ZCL_DHAPE_OPER_CREATE_SO`, implements this functionality:

```abap
  METHOD on_data.
    DATA lv_data TYPE string.
    mo_in->read_copy( IMPORTING ea_data = lv_data ).

    SUBMIT SEPM_DG_EPM_STD_CHANNEL USING SELECTION-SET 'SEPM_TECHED_SO' AND RETURN.
    lv_data = ' --> One additional EPM Sales Order with five related Sales Order Items created.'.

    mo_out->write_copy( lv_data ).
  ENDMETHOD.
```
<br>
You are asked to leverage this Custom ABAP Operator in this exercise with the goals to
- experience how the execution of ABAP Function Modules in a remote S/4HANA system can be triggered from SAP Data Intelligence Pipeline and
- finally have an approach to change data on the Enterprise Procurement Model in S/4HANA in order to verify the delta capabilities of the ABAP CDS Views and your Pipeline implementations.<br><br>

>**Important Note**<br>
>Please consider that we are working on the same central data basis (the EPM tables in S/4HANA) which is used by all workshop participants. Hence, Sales Order records that you create by using the Custom ABAP Operator will become change data for all session participants.

## Exercise 2.1 - Making custom ABAP Operators available in SAP Data Intelligence

After completing these steps you will have created a Pipeline that triggers the execution of a Function Module in the connected S/4HANA for EPM data generation.

1. Log on to SAP Data Intelligence and enter the Launchpad application. Then start the ***Modeler*** application.
   - Follow the link to your assigned Data Intelligence instance, e.g. https://vsystem.ingress.xyz.dhaas-live.shoot.live.k8s-hana.ondemand.com/app/datahub-app-launchpad/.
   - In the pop-up window, enter ***"default"*** as the tenant name.<br><br>
   ![](/exercises/ex2/images/ex2-001b.JPG)<br><br>
   - In the next pop-up window, enter you assigned user name (e.g. ***"TA99"***) and your password (initially ***"Welcome01"***).<br><br>
   ![](/exercises/ex2/images/ex2-002b.JPG)<br><br>
   - From the Launchpad, start the ***Modeler*** application by clicking on the corresponding tile.<br><br>
   ![](/exercises/ex2/images/ex2-003b.JPG)<br><br>

2. Make sure you are in the ***Graphs*** tab of the Modeler UI (see left side). Then click the ***+*** symbol in order to create a new Pipeline.<br><br>
![](/exercises/ex2/images/ex2-004b.JPG)<br><br>

3. Now a new Pipeline canvas is opened on the right side and the Modeler UI automatically switches to the ***Operators*** tab (see left side). In the list of operators, drag the ***Custom ABAP Operator*** and drop it into the Pipeline canvas. Click the ABAP CDS Reader node in the canvas one time and then click the ***configuration*** icon.<br><br>
![](/exercises/ex2/images/ex2-005b.JPG)<br><br>

4. In the configuration panel for the ***Custom ABAP Operator***, specify the S/4HANA Connection: Select `S4_RFC_TechEd`. Then click on the ***ABAP Operator*** selection icon.<br><br>
![](/exercises/ex2/images/ex2-006b.JPG)<br><br>

5. In the pop-up windows, select the **"Creation of EPM Sales Order"** option. Then click ***OK***.<br><br>
![](/exercises/ex2/images/ex2-007b.JPG)<br><br>

7. From the list of Operators, drag the ***Terminal*** operator into the Pipeline canvas. Connect **the output port of the Custom ABAP Operator with the input port of the Terminal operator**. Then connect the **output port of the Terminal operator with the input port of the Custom ABAP Operator**.<br>
With this Pipeline setup, we can - from the Terminal UI - trigger the ABAP Operator call and the reception of confirmation message at the same time.<br><br>

8. ***Save*** the Pipeline and enter the following parameters prompted in the pop-up windows:<br><br>
   - Name: `teched.XXXX.EPM_FM_Call_SO_Generator`, where XXXX is your user name, for example "teched.TA99.EPM_FM_Call_SO_Generator"
   - Description: `XXXX - Generate EPM SO data via ABAP FM call`, where XXXX is your user name, for example "TA99 - Generate EPM SO data via ABAP FM call"
   - Category: `dat262'.
![](/exercises/ex2/images/ex2-008b.JPG)<br><br>

9.	In the pop-up windows, select the **"Creation of EPM Sales Order"** option. Then click ***OK***.<br><br>
![](/exercises/ex2/images/ex2-009b.JPG)<br><br>



## Exercise 2.2 - Using a custom ABAP Operator to verify your Delta Replication of EPM Sales Orders

...currently being reworked.



## Summary

...currently being reworked.
