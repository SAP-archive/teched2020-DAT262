# Exercise 1 - Replicating data from ABAP CDS Views in SAP Data Intelligence

In this exercise, we will leverage the ABAP CDS Views that got created during the first Deep Dive demo in the connected S/4HANA system in a Pipeline.<br><br>
The use case is
- to obtain the Business Partner master data in S/4HANA's demo application **Enterprise Procurement Model (EPM)** and make the records available for the corporate Data Analysts in an S3 object store.
- to also persist the transactional data for EPM Sales Orders in S3.
- In both cases, any single change of these data sources in the S/4HANA system has to be instantly and automatically replicated to the related files in S3 in append mode.

(As a reminder: You can recap the relationship between the relevant EPM table entities that used in this exercise [here](../ex0#short-introduction-to-the-enterprise-procurement-model-epm-in-s4))<br>

## Exercise 1.1 - Consume ABAP CDS Views in SAP Data Intelligence

After completing these steps you will have created...

1. Click here.
<br>![](/exercises/ex1/images/01_01_0010.png)

2.	Insert this line of code.
```abap
response->set_text( |Hello World! | ). 
```



## Exercise 1.2 Sub Exercise 2 Description

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

