# Exercise 2 - Triggering the execution of a function module in a remote S/4HANA system

So far, we have missed to proof whether or not our ABAP CDS Views and the consuming Data Intelligence Pipelines are really providing delta information. This would require access to the S/4HANA system in order to conduct changes on the data basis, hence on the Business Partner table or on the Sales Order object.

The Enterprise Procurement Model (EPM) demo application comes with a report that allows you to generate EPM Sales Order data. This report **`SEPM_DG_EPM_STD_CHANNEL`** can be started with the transaction **`SEPM_DG`** in S/4HANA.<br><br>
![](/exercises/ex2/images/ex2-000b.JPG)<br><br>

As a matter of fact, we couldn't provide an SAP GUI based access to the connected S/4HANA system.

For still being able to trigger changes on EPM data in that system, we have 

```abap
  METHOD on_data.
    DATA lv_data TYPE string.
    mo_in->read_copy( IMPORTING ea_data = lv_data ).

    SUBMIT SEPM_DG_EPM_STD_CHANNEL USING SELECTION-SET 'SEPM_TECHED_SO' AND RETURN.
    lv_data = 'Your entry: ' && lv_data && '. --> One additional EPM Sales Order with five related Sales Order Items created.'.

    mo_out->write_copy( lv_data ).
  ENDMETHOD.
```

In this exercise, we will trigger the execution of an ABAP Function Module in a remote S/4HANA system.

## Exercise 2.1 - Making custom ABAP Operators available in SAP Data Intelligence

After completing these steps you will have created a Pipeline that calls a Function Module in the connected S/4HANA and return a confirmation text.

1. Log on to SAP Data Intelligence and enter the Launchpad application. Then start the ***Modeler*** application.
   - Follow the link to your assigned Data Intelligence instance, e.g. https://vsystem.ingress.xyz.dhaas-live.shoot.live.k8s-hana.ondemand.com/app/datahub-app-launchpad/.
   - In the pop-up window, enter ***"default"*** as the tenant name.<br><br>
   ![](/exercises/ex2/images/ex2-001b.JPG)<br><br>
   - In the next pop-up window, enter you assigned user name (e.g. ***"TA99"***) and your password (initially ***"Welcome01"***).<br><br>
   ![](/exercises/ex2/images/ex2-002b.JPG)<br><br>
   - From the Launchpad, start the ***Modeler*** application by clicking on the corresponding tile.<br><br>
   ![](/exercises/ex2/images/ex2-003b.JPG)<br><br>

2.	Make sure you are in the ***Graphs*** tab of the Modeler UI (see left side). Then click the ***+*** symbol in order to create a new Pipeline.<br><br>
![](/exercises/ex2/images/ex2-004b.JPG)<br><br>

3.	Now a new Pipeline canvas is opened on the right side and the Modeler UI automatically switches to the ***Operators*** tab (see left side). In the list of operators, drag the ***Custom ABAP Operator*** and drop it into the Pipeline canvas. Click the ABAP CDS Reader node in the canvas one time and then click the ***configuration*** icon.<br><br>
![](/exercises/ex2/images/ex2-005b.JPG)<br><br>

4.	In the configuration panel for the ***Custom ABAP Operator***, specify the S/4HANA Connection: Select `S4_RFC_TechEd`. Then click on the ***Version*** selection icon.<br><br>
![](/exercises/ex2/images/ex1-006b.JPG)<br><br>



## Exercise 2.2 - Using a custom ABAP Operator to verify your Delta Replication of EPM Sales Orders

...to be continued.



## Summary

...to be continued.
