# Deep Dive 2 - Calling an ABAP function module in SAP S/4HANA from SAP Data Intelligence

In this Deep Dive, we will demonstrate how to create a custom ABAP operator in SAP S/4HANA and trigger its execution from a Pipeline in SAP Data Intelligence. The ABAP operators are developed in the S/4HANA system as normal development objects, hence, they can be transported within the system landscape.<br>

For SAP S/4HANA systems greater than 1909, you are good to start – no installation required.<br>
(You can also run this scenario with a SAP Business Suite system, but then it is required to install the (non-modifying) DMIS add-on on that system.)<br>

The basis for this integration technology is the ABAP Pipeline Engine in SAP S/4HANA and the ABAP Subengine in SAP Data Intelligence.
![](images/1-001a.JPG)

## Deep Dive 2.1 - Create a custom ABAP Operator in SAP S/4HANA

After completing these steps you will have created...

1. Click here.


2.	Insert this line of code.
```abap
response->set_text( |Hello World! | ). 
```



## Deep Dive 2.2 - Integrate the custom ABAP Operator in a SAP Data Intelligence Pipeline

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


