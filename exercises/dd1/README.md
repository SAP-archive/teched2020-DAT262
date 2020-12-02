# Deep Dive 1 - ABAP CDS View based data extraction in SAP Data Intelligence

As a matter of fact, it was neither possible to make the ABAP Development Tools (ADT) available for the participants of this virtual hands-on workshop, nor was it feasible to provide a SAP GUI. For this reason, **this Deep Dive section is presented by the trainer** live in ADT and the SAP GUI connected to the underlying SAP S/4HANA system.<br>
<br>
>**IMPORTANT NOTE**<br>
>The documentation in this section is intended as a later guidance for you to reproduce the described steps in your own environments.<br>
>Even though it contains step by step instructions, **this part does not belong to the hands-on exercises!**
<br>

ABAP Development Tools (ADT), formerly known as "ABAP in Eclipse", is the integrated ABAP development environment built on top of the Eclipse platform. Its main objective is to support developers in today’s increasingly complex development environments by offering state-of the art ABAP development tools. You can find more information about ADT **[here](https://tools.hana.ondemand.com/#abap)**.<br>
<br>
CDS is an extension of the ABAP Dictionary that allows you to define semantically rich data models in the database and to use these data models in your ABAP programs. CDS is a central part of enabling code push-down in ABAP applications.<br>
You can find more information about CDS in the **[ABAP keyword documentation](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abencds.htm)** and the **[SAP Community](https://community.sap.com/topics/abap)**.<br>
<br>
After completing the steps in this Deep Dive section we will have created two new custom ABAP CDS Views on our SAP S/4HANA system. Our goal is to leverage these CDS Views later to access the Customer and Sales Order data of the Enterprise Procurement Model (our demo dataset) from Pipelines in SAP Data Intelligence.

## Deep Dive 1.1 - Create a simple ABAP CDS View using ABAP Develoment Tools (ADT)

In this sub section you can track how to create a CDS (Core Data Services) View using ABAP Development Tools (ADT). In our specific case, it is demonstrated how to create a CDS View to access data of the EPM table SNWD_BPA, which contains the Business Partner record set.

1. Create a CDS View
In the context menu of your package choose ***New*** and then choose ***Other ABAP Repository Object***.<br><br>
![](/exercises/dd1/images/1-001a.JPG)

2.	Select ***Data Definition***, then choose ***Next***.<br><br>
![](/exercises/dd1/images/1-002a.JPG)


3. Enter the following values, then choose Next.
- Name, e.g. ```Z_CDS_EPM_BUPA```
- Description: **CDS View for EPM Business Partner Extraction**
- Referenced Object: **SNWD_BPA**<br><br>
![](/exercises/dd1/images/1-003a.JPG)

4.	Accept the default transport request (local) by simply choosing ***Next*** again.<br><br>
![](/exercises/dd1/images/1-004a.JPG)

5.	Select the entry ***Define View***, then choose ***Finish***.<br><br>
![](/exercises/dd1/images/1-005a.JPG)

6.	The new view appears in an editor, with an error showing up because of the still missing SQL View name.<br>
In this editor, enter value for the SQL View name in the annotation ```@AbapCatalog.sqlViewName```, e.g. ```Z_SQL_EPM_BUPA```.<br>
The SQL view name is the internal/technical name of the view which will be created in the database. 
```Z_SQL_EPM_BUPA``` is the name of the CDS view which provides enhanced view-building capabilities in ABAP. 
You should always use the CDS view name in your ABAP applications.<br><br>
The data source plus its fields have automatically been added to the view definition because of the reference to the data source object we gave in step 3.
If you haven't provided that value before, you can easily search for and add your data source using the keyboard shortcut **CTRL+SPACE**.<br><br>
![](/exercises/dd1/images/1-006a.JPG)

7.	Delete the not needed fields in the SELECT statement, add the annotation ```@ClientHandling.type: #CLIENT_DEPENDENT``` and beautify the view.<br><br>
![](/exercises/dd1/images/1-007a.JPG)<br><br>
The code may now look as follows:
```abap
@AbapCatalog.sqlViewName: 'Z_SQL_EPM_BUPA'
@AbapCatalog.compiler.compareFilter: true
@AbapCatalog.preserveKey: true
@ClientHandling.type: #CLIENT_DEPENDENT
@AccessControl.authorizationCheck: #CHECK
@EndUserText.label: 'CDS View for EPM Business Partner Extraction'

define view Z_CDS_EPM_BUPA
    as select from SNWD_BPA
    
{
    key node_key as NodeKey,
        bp_role as BpRole,
        email_address as EmailAddress,
        phone_number as PhoneNumber,
        fax_number as FaxNumber,
        web_address as WebAddress,
        address_guid as AddressGuid,
        bp_id as BpId,
        company_name as CompanyName,
        legal_form as LegalForm,
        created_at as CreatedAt,
        changed_at as ChangedAt,
        currency_code as CurrencyCode
}
```

8.	***Save (CTRL+S or disk symbol in menue bar)*** and ***Activate (CTRL+F3 or magic wand symbol in menue bar)*** the CDS View.<br>
(first ![](/exercises/dd1/images/1-008a.JPG) 
then ![](/exercises/dd1/images/1-008b.JPG))<br><br>

9.	We are now able to verify the results in the ***Data Preview*** by choosing ***F8***. Our CDS View data preview should look like this:<br><br>
![](/exercises/dd1/images/1-009a.JPG)<br><br>

We have now successfully created the first simple CDS View in SAP S/4HANA. In the next step we'll be implementing a more complex CDS view that joins multiple tables.


## Deep Dive 1.2 - Create a complex ABAP CDS View in ADT (joining multiple tables)

In this part of the Deep Dive you can learn how to create a more complex CDS View, again using the ABAP Development Tools (ADT). Now, the trainer will show the implementation of a CDS View which will join the tables SNWD_SO, SNWD_SO_I, SNWD_PD, and SNWD_TEXTS in order to fetch all Sales Order relevant data, including all positions, products, and product names.<br>
In a later step, this CDS View will then be enabled for Change Data Capturing (CDC) for an event based processing of Sales Order related delta information to the target storage.

After completing these steps you will have...

...to be completed

## Deep Dive 1.3 - Enable Delta Extraction in simple and complex ABAP CDS Views

to be completed

## Deep Dive 1.4 - Integrate ABAP CDS Views in SAP Data Intelligence Pipelines

to be completed

## Summary

to be completed

Continue to [Deep Dive 2 - Calling an ABAP function module in SAP S/4HANA from SAP Data Intelligence](../dd2/README.md)




