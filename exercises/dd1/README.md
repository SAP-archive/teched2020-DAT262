# Deep Dive 1 - ABAP CDS View based data extraction in SAP Data Intelligence

As a matter of fact, it was neither possible to make the ABAP Development Tools (ADT) available for the participants of this virtual hands-on workshop, nor was it feasible to provide a SAP GUI. For this reason, **this Deep Dive section is presented by the trainer** live in ADT and the SAP GUI connected to the underlying SAP S/4HANA system.<br>
<br>
>**IMPORTANT NOTE**<br>
>The documentation in this section is intended as a later guidance for you to reproduce the described steps in your own environments.<br>
>Even though it contains step by step instructions, **this part does not belong to the hands-on exercises!**
<br>

**ABAP Development Tools (ADT)**, formerly known as "ABAP in Eclipse", is the integrated ABAP development environment built on top of the Eclipse platform. Its main objective is to support developers in today’s increasingly complex development environments by offering state-of the art ABAP development tools. You can find more information about ADT **[here](https://tools.hana.ondemand.com/#abap)**.<br>
<br>
CDS is an extension of the ABAP Dictionary that allows you to define semantically rich data models in the database and to use these data models in your ABAP programs. CDS is a central part of enabling code push-down in ABAP applications.<br>
You can find more information about CDS in the **[ABAP keyword documentation](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abencds.htm)** and the **[SAP Community](https://community.sap.com/topics/abap)**.<br>
<br>
Starting with SAP S/4HANA Cloud 1905 and SAP S/4HANA 1909 FPS01 (on-premise), Change Data Capturing (CDC) is supported for ABAP CDS Views. For a CDS view using this delta method, changes in tables belonging to this view are recorded by the Change Data Capture mechanism. From a technology perspective this delta method makes use of real-time database triggers on table level based on SLT technology. INSERT, UPDATE and DELETE operations can be recorded by the framework.<br>
- In case of an INSERT or UPDATE operation a scheduled job is selecting the records based on these key combinations from the CDS view and pushes them as complete records into the Operational Delta Queue (ODQ).
- In case of a DELETE operation the job generates an empty record with just the key field(s) filled, sets the deletion indicator and hands them over to ODQ.

Those who are interesed in more information about Change Data Capture for ABAP CDS Views may like to read the related part of the **[Data Intelligence - ABAP Integration Guide](https://help.sap.com/viewer/3a65df0ce7cd40d3a61225b7d3c86703/Cloud/en-US/55b2a17f987744cba62903e97dd99aae.html)** or the blog **[CDS based data extraction – Part II Delta Handling](https://blogs.sap.com/2019/12/16/cds-based-data-extraction-part-ii-delta-handling/)**.<br><br>

After having completed the steps in this Deep Dive section we will have created two new delta-enabled custom ABAP CDS Views on our SAP S/4HANA system. Our goal is to leverage these CDS Views later to access the Customer and Sales Order data of the Enterprise Procurement Model (our demo dataset) from Pipelines in SAP Data Intelligence.<br><br>

## Deep Dive 1.1 - Create a simple ABAP CDS View (projections) in ADT

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


## Deep Dive 1.2 - Delta-enablement for simple ABAP CDS Views (projections)

Delta-enablement for simple ABAP CDS Views is pretty easy! The only step to do is adding the `@Analytics` annotation to the view that sets the enabled flag and the change data capturing approach.<br>

Let's continue with the simple ABAP CDS View that we have implemented in the previous section and introduce the CDC delta for **`Z_CDS_EPM_BUPA`**.

1. In ADT's Project Explorer, navigate to ***Core Data Services --> Data Definitions*** and double-click on the ABAP CDS View `Z_CDS_EPM_BUPA`.<br><br>
![](/exercises/dd1/images/dd1-010a.JPG)<br><br>

2. Under the existing list of annotations, enter the following lines:<br>
```abap
@Analytics:{
    dataExtraction: {
        enabled: true,
        delta.changeDataCapture.automatic: true
    }
}
```
<br>
![](/exercises/dd1/images/dd1-011a.JPG)<br><br>

3. ***Save*** (CTRL+S or ![](/exercises/dd1/images/1-008a.JPG)) and ***Activate*** (CTRL+F3 or ![](/exercises/dd1/images/1-008b.JPG)) the CDS View.<br><br>

In this simple case, the framework can derive the relation between the fields of the CDS view and key fields of the underlying table itself. Whenever a record is inserted, updated or deleted in the underlying table, a record with the respective table key is stored in a generated logging table. Based on this information, the scheduled job selects the data record from the CDS view and pushes it into the ODQ.<br><br>



## Deep Dive 1.3 - Create a more complex ABAP CDS View in ADT (joining multiple tables)

In this part of the Deep Dive you can learn how to create a more complex CDS View, again using the ABAP Development Tools (ADT). Now, the trainer will show the implementation of a CDS View which will join the tables SNWD_SO, SNWD_SO_I, SNWD_PD, and SNWD_TEXTS in order to fetch all Sales Order relevant data, including all positions, products, and product names.<br>
In a later step, also this CDS View will be enabled for Change Data Capturing (CDC) for an event based processing of Sales Order related delta information to the target storage.

...to be completed

## Deep Dive 1.3 - Enable Delta Extraction in simple and complex ABAP CDS Views

...to be completed

## Deep Dive 1.4 - Delta-enablement for complex ABAP CDS Views (joining multiple tables)

...to be completed
The Change Data Capture recording mechanism uses database triggers to record any changes to the tables that belong to an ABAP CDS view. To do this, the key fields of all underlying tables need to be mapped to the fields of the CDS view.

## Deep Dive 1.5 - Integrate ABAP CDS Views in SAP Data Intelligence Pipelines

...to be continued

## Summary

to be completed

Continue to [Deep Dive 2 - Calling an ABAP function module in SAP S/4HANA from SAP Data Intelligence](../dd2/README.md)




