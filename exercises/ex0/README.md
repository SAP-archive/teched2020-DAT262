# Getting Started

The idea of ABAP Integration is to establish a unified model to consolidate all interaction scenarios between SAP Data Intelligence and an ABAP-based SAP system (directional and bi-directional).

The primary use cases for ABAP integration in SAP Data Intelligence are metadata retrieval, data provisioning, and functional execution.
![](/images/0-001a.JPG)

## Level 2 Heading

After completing these steps you will have....

1.	Click here. 


2.	Insert this code.
```
 DATA(lt_params) = request->get_form_fields(  ).
 READ TABLE lt_params REFERENCE INTO DATA(lr_params) WITH KEY name = 'cmd'.
  IF sy-subrc <> 0.
    response->set_status( i_code = 400
                     i_reason = 'Bad request').
    RETURN.
  ENDIF.
```

## Summary

Now that you have ... 
Continue to - [Deep Dive 1 - Exercise 1 Description](../dd1/README.md)
