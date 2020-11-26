# Deep Dive 1:

As a matter of fact, it is neither possible to make the ABAP Development Tools (ADT) available for the participants of this virtual workshop, nor is it feasible to provide a SAP GUI. For this reason, this Deep Dive part is presented by the trainer directly in ADT and the SAP GUI of the S/4HANA system.
<br>
Here is the documentation of these steps that should allow you to reproduce the described steps in your own environments later on.

## Exercise 1.1 Sub Exercise 1 Description

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


