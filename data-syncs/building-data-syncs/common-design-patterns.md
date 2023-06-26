# Common Design Patterns

## 1. Overview

We have outlined some common design patterns below. You can use this non-exhaustive list as a jumping off point for desigining your own data syncs.

## 2. Syncing Changes vs. Syncing Full Datasets

When creating data sync, you need to know if you are synchronizing the full data set or simply a subset of the data.

### **2.1 Full Data Synchronization**

Set `<DroppedRecordBehaviour type="DELETE" />` so that any records that are no longer in the source are deleted in the target.

### **2.2 Partial Data Synchronization**

Set `<DroppedRecordBehaviour type="IGNORE" />` otherwise, it will delete any records that are not in your partial recordset. You are unable to delete any records during partial data synchronization.

### **2.3 Partitioning your Data Synchronization**

You can create a full data synchronization on a partial dataset by filtering the source and target. For example, if you want to sync transactions from one system to another but there are a lot of transactions, you can run a data sync where you filter by`<Filter>[Transaction Date] > 100000 </Filter>` In both the source and the target. This way, you can sync a smaller dataset while still being able to perform deletions and ensuring a full synchronization of that partition of the data.

## 3. Syncing Linked Data

When syncing data that is linked to other data the order of the data sync is important. The data sync should be ordered as to what data is linked to first.&#x20;

For example, if you have customers and invoices and the invoices are linked to each customer, then the customer data should be synced first. Therefore, when the invoices are synced they will be linked to the appropriate customer as the customer data is already in the target.&#x20;

## 4. Populating Reference Data

To create a reference data set, for example, a country code, based on a set of shipping labels then the data should first be run against the Country Codes table before the shipping labels are synced to the label's table. &#x20;

In this scenario a `supressDuplicateError=”false”` should be set up when running the data against the Country Codes table, this is because duplicates are expected which are not ‘error’s’ and therefore, this must be identified.&#x20;

## 5. Creating a Reference Master Data Set

Sometimes different reference data values can mean the same thing, but different systems use different codes. In Cinchy’s country code example under ‘Populating Reference Data’;&#x20;

* System A uses the full country name (ex. Canada, United States),&#x20;
* System B uses the 2 letter ISO code (ex. CA, US), and&#x20;
* System C uses the 3 letter ISO code (ex. CAN, USA).&#x20;

All three of these systems can sync into one shipping label table, with a link to Country, but depending on the system, we use a different link column reference. The same column mapping will look slightly different in each data sync to the Shipping Labels table.&#x20;

Note that you can change the display column in the Shipping Labels table to switch between the columns if you decide on a different code as the golden standard, without having to modify your data sync configurations.

#### System A Config

```markup
<ColumnMapping sourceColumn="Country" targetColumn="Country" linkColumn="ISO 3166 Name" />
```

#### System B Config

```markup
<ColumnMapping sourceColumn="ShippingCountry" targetColumn="Country" linkColumn="ISO 3166-2 Letter Code" />
```

#### System C Config

```markup
<ColumnMapping sourceColumn="Country" targetColumn="Country" linkColumn="ISO 3166-3 Letter Code" />
```

## 6. Multiple Sources - Separate Records

When syncing from different sources into the same data set, the source of the data will need to be added as a column, this is to avoid overwriting records from other sources. This column will be added to your sync key as well. For contacts you might have:

```markup
<CalculatedColumn name="Source System" formula="CONCAT('','Salesforce')" dataType="Text" /> 
...
<SyncKey>
            <SyncKeyColumnReference name:"Source System" />
            <SyncKeyColumnReference name:"Email Address" />
</SyncKey>
```

Once all your data from various systems in Cinchy, you can master that dataset and sync the master record back into any source systems where you wish to do so. These syncs would simply filter the source by where the \[Master Record] column is set to true, and sync on the unique identifier without the source system. We would also likely only want to update records already in the source, rather than deleting unmastered data or adding all records from other systems.

```markup
<SyncKey>
           <SyncKeyColumnReference name="Email Address" />
</SyncKey>
<NewRecordBehaviour type="IGNORE" />
<ChangedRecordBehaviour type="UPDATE" />
<DroppedRecordBehaviour type="IGNORE"/>
```

## **7. Multiple sources - Enriching Fields**&#x20;

To use different sources to enrich different fields on the same record, the dropped record behaviour should be set to ignore, and the columns should be updated based on a sync key on the record.&#x20;

Depending on the source system, it may or may not be allowed to create new records; Usually, internal systems, for example, customer invoices should be able to create new customer records.&#x20;

However, external data sources, for example, published company size or industry report will only add noise to your table when new records are attempted to be inserted**.**

```markup
<SyncKey>
        <SyncKeyColumnReference name="Email Address" />
</SyncKey>
<NewRecordBehaviour type="IGNORE" />
<ChangedRecordBehaviour type="UPDATE" />
<DroppedRecordBehaviour type="IGNORE"/>
```

## **8. Post Processing**&#x20;

Post sync scripts can be added to a data sync configuration. This would allow you to run post-processing after the data sync has been completed.&#x20;

&#x20;For example, you can run a query to update the assignee on a lead after the leads data sync runs.

This way more complex logic is created, for example, only updating where the assignee is empty, except in the case it is a high-value lead, it will be reassigned to the most senior person on each team (based on another table in the instance that has the seniority and team of each sales director).

```markup
<CinchyTableTarget model="" domain="Revenue" table="Leads" suppressDuplicateErrors="true" >
     <PostSyncScripts>
          <PostSyncScript name="Script1" timeout="60">
               <CQL>INSERT CQL HERE</CQL>
          </PostSyncScript>
     </PostSyncScripts>
</CinchyTableTarget>
```

## **9. No Unique Identifier**&#x20;

If you have a file source (i.e. delimited, csv or excel) and you want to sync data into Cinchy that does not have a sync key, you can add a calculated column for the row number of that record.

```markup
<CalculatedColumn name="RowNumber" formula="row_number()" dataType="Number" /
```

You will also want to add a calculated column for the file name (and ensure that it is unique) to be able to re-run the data sync if any failures occur.

## **10. Bi-Directional Sync** &#x20;

To run a bi-directional sync, you need to identify a source system unique identifier. You will then need to run the following four data syncs for bidirectional syncing.&#x20;

Note that if one of the systems cannot create new records, we can omit the data sync configuration where we are creating new records in the other system.

### **10.1 Sync from External Source into Cinchy (New Records)**

First, we run a data sync from the source into Cinchy filtering the source by records where the Cinchy ID (a custom field we create in the source) is empty. We insert these records into Cinchy and make sure to populate the source unique identifier column in Cinchy.

### **10.2 Sync from Cinchy into External Source (New Records)**

We can do the opposite as well by syncing data from Cinchy to the external source by inserting any records where a source unique identifier is empty.

### 10.3 Sync from External Source into Cinchy (Existing Records)

Now that all records exist in both the external system and Cinchy, we can sync data based on the target system's unique ID, in this case, we are syncing data from the external source into Cinchy, based on the Cinchy ID. We filter out records where Cinchy ID is null here to avoid errors (those will be picked up the next time the new records sync runs).

### 10.4 Sync from Cinchy into External Source (Existing Records)

Likewise, we can sync any data changes from Cinchy into the external source using the external unique identifier as the sync key, filtering out records in Cinchy where the external identifier is empty.

## **11. Caching Query Data**

In order to run intensive summary queries on the platform, data sync must be created to cache the data in a cinchy table. To do so simply sync the results of your Cinchy query into a Cinchy table with the same schema, and schedule the CLI to run as often as you would like your cache to expire. You can point other queries or reports to the query from this table, rather than the complex query itself.

## 12. Configuration Changes

### 12.1 Adding Fields

To add more fields to sync, ensure that the columns are in the target system first. You can then swap in the new data sync configuration whenever and it will get picked up for future execution.

### 12.2 Removing Fields

To remove fields from a data sync, swap out your data sync configuration. You can optionally delete the field in the source and/or target system afterwards.

### 12.3 Changing Column Mapping

If there are no fields being added or deleted, simply swap out your sync configuration. To add or remove fields, follow the guidelines above of when to swap in the config versus making the data model changes (add columns first, swap out config, validate config, delete unneeded columns).

### 12.4 Changing Sync Key

Simply swap out your data sync configuration if you are changing the sync key. It is a good idea to check if your new sync key is unique in both your source and target. The CLI worker will sync using the first record it found in the source to the first record it finds in the target. Checking for duplicate sync keys will allow you to understand whether any unexpected behaviour will occur.\
