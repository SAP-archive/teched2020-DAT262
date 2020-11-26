# Deep Dive 1 - ABAP CDS View based data extraction in SAP Data Intelligence

As a matter of fact, it was neither possible to make the ABAP Development Tools (ADT) available for the participants of this virtual hands-on workshop, nor is it feasible to provide a SAP GUI. For this reason, this Deep Dive section is presented by the trainer live in ADT and the SAP GUI connected to the underlying SAP S/4HANA system.<br>
<br>
The documentation in this section is intended as guidance for you to reproduce the described steps in your own environments later on.<br>
It is **not a part** of the hands-on exercises!<br>
<br>
ABAP Development Tools (ADT), formerly known as "ABAP in Eclipse", is the integrated ABAP development environment built on top of the Eclipse platform. Its main objective is to support developers in today’s increasingly complex development environments by offering state-of the art ABAP development tools.<br>
You can find more information about ADT **[here](https://tools.hana.ondemand.com/#abap)**.<br>
<br>
CDS is an extension of the ABAP Dictionary that allows you to define semantically rich data models in the database and to use these data models in your ABAP programs. CDS is a central part of enabling code push-down in ABAP applications.<br>
You can find more information about CDS in the **[ABAP keyword documentation](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abencds.htm)** and the **[SAP Community](https://community.sap.com/topics/abap)**.<br>
<br>
After completing the steps in this Deep Dive section we will have created two new custom ABAP CDS Views on our SAP S/4HANA system. Our goal is to leverage these CDS Views later to access the Customer and Sales Order data of the Enterprise Procurement Model (our demo dataset) from Pipelines in SAP Data Intelligence.

## Deep Dive 1.1 - Create a simple ABAP CDS View using ABAP Develoment Tools (ADT)

This sub section you will see how to create a CDS (Core Data Services) View using ABAP Development Tools (ADT). In particular, it is demonstrated how to create a CDS View to access data of the EPM table SNWD_BPA, which contains the Business Partner records.

1. Create a CDS View
In the context menu of your package choose New and then choose Other ABAP Repository Object.
![](/exercises/dd1/images/1-001a.jpg)

2.	Insert this line of code.
```abap
response->set_text( |Hello World! | ). 
```



## Deep Dive 1.2 - Create a complex ABAP CDS View in ABAP Development Tools (ADT) (joining multiple tables)

After completing these steps you will have...

1.	Enter this code.
```abap
DATA(lt_params) = request->get_form_fields(  ).
READ TABLE lt_params REFERENCE INTO DATA(lr_params) WITH KEY name = 'cmd'.
  IF sy-subrc <> 0.
    response->set_status( i_code = 400
                     i_reason = 'Bad request').
    RETURN.
  ENDIF.

```

2.	Click here.
<br>![](/exercises/ex1/images/01_02_0010.png)


## Summary

You've now ...

Continue to - [Exercise 2 - Exercise 2 Description](../ex2/README.md)


