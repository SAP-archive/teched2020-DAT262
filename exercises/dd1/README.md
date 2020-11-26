# Deep Dive 1 - ABAP CDS View based data extraction in SAP Data Intelligence

As a matter of fact, it was neither possible to make the ABAP Development Tools (ADT) available for the participants of this virtual hands-on workshop, nor is it feasible to provide a SAP GUI. For this reason, this Deep Dive section is presented by the trainer live in ADT and the SAP GUI connected to the underlying SAP S/4HANA system.<br>
<br>
The documentation in this section is intended as guidance for you to reproduce the described steps in your own environments later on.<br>
It is **not a part** of the hands-on exercises!<br>
<br>
<br>
ABAP Development Tools (ADT), formerly known as "ABAP in Eclipse", is the integrated ABAP development environment built on top of the Eclipse platform. Its main objective is to support developers in today’s increasingly complex development environments by offering state-of the art ABAP development tools.
You can find more information about ADT ***[here](https://tools.hana.ondemand.com/#abap)***.<br>
<br>


## Deep Dive 1.1 - Create a Simple ABAP CDS View in ABAP Develoment Tools (ADT)

After completing these steps you will have created...

1. Click here.
<br>![](/exercises/ex1/images/01_01_0010.png)

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


