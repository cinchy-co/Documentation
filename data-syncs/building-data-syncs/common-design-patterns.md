# Common design patterns

## Overview

This page outlines some common design patterns. You can use this basic list as a starting point to design your own data syncs.

## Syncing changes vs. syncing full datasets

When creating data sync, you need to know if you are synchronizing the full data set or just a subset of the data.

### Full data synchronization

Set `<DroppedRecordBehaviour type="DELETE" />` so that any records that are no longer in the source are deleted in the target.

### Partial data synchronization

Set `<DroppedRecordBehaviour type="IGNORE" />` otherwise, it will delete any records that aren't in your partial recordset. You are unable to delete any records during partial data synchronization.

### Partitioning your data synchronization

You can create a full data synchronization on a partial dataset by filtering the source and target. For example, if you want to sync transactions from one system to another but there are a lot of transactions, you can run a data sync where you filter by`<Filter>[Transaction Date] > 100000 </Filter>` In both the source and the target. This way, you can sync a smaller dataset while still being able to perform deletions and ensuring a full synchronization of that partition of the data.

## Syncing linked data

When syncing data that's linked to other data the order of the data sync is important. You should order the sync to the data is linked to first.

For example, if you have customers and invoices and the invoices link to each customer, then sync the customer data first. Therefore, when the invoices sync, they will link to the appropriate customer as the customer data is already in the target.

## Populating reference data

To create a reference data set (such as a country code) based on a set of shipping labels then you should first run the data against the Country Codes table before the shipping labels sync to the labels table.

In this scenario a `supressDuplicateError=”false”` should be set up when running the data against the Country Codes table as duplicates are expected, which aren't ‘error’s’ and must be identified.

## Creating a reference master data set

Sometimes different reference data values can mean the same thing, but different systems use different codes. In Cinchy’s country code example under ‘Populating Reference Data’:

- System A uses the full country name (ex. Canada, United States),
- System B uses the 2 letter ISO code (ex. CA, US), and
- System C uses the 3 letter ISO code (ex. CAN, USA).

All three of these systems can sync into one shipping label table, with a link to Country, but depending on the system, we use a different link column reference. The same column mapping will look slightly different in each data sync to the Shipping Labels table.

You can change the display column in the Shipping Labels table to switch between the columns if you decide on a different code as the golden standard, without having to modify your data sync configurations.

#### System A config

```markup
<ColumnMapping sourceColumn="Country" targetColumn="Country" linkColumn="ISO 3166 Name" />
```

#### System B config

```markup
<ColumnMapping sourceColumn="ShippingCountry" targetColumn="Country" linkColumn="ISO 3166-2 Letter Code" />
```

#### System C config

```markup
<ColumnMapping sourceColumn="Country" targetColumn="Country" linkColumn="ISO 3166-3 Letter Code" />
```

## Multiple sources - separate records

If you sync from different sources into the same data set, you need to add the source of the data as a column to avoid overwriting records from other sources. This column will also be added to your sync key. For contacts you might have:

```markup
<CalculatedColumn name="Source System" formula="CONCAT('','Salesforce')" dataType="Text" />
...
<SyncKey>
            <SyncKeyColumnReference name:"Source System" />
            <SyncKeyColumnReference name:"Email Address" />
</SyncKey>
```

Once all your data from various systems in Cinchy, you can master that dataset and sync the master record back into any source systems. These syncs would filter the source by where the [Master Record] column is set to true, and sync on the unique identifier without the source system. You would also only want to update records already in the source, rather than deleting unmastered data or adding all records from other systems.

```markup
<SyncKey>
           <SyncKeyColumnReference name="Email Address" />
</SyncKey>
<NewRecordBehaviour type="IGNORE" />
<ChangedRecordBehaviour type="UPDATE" />
<DroppedRecordBehaviour type="IGNORE"/>
```

## Multiple sources - enriching fields

To use different sources to enrich different fields on the same record,you should set the dropped record behaviour to ignore, and update the columns based on a sync key on the record.

The ability to create new records depends on the source system. Internal systems, such as customer invoices, should be able to create new customer records.

External data sources, such as a published industry report, will only add noise to your table when you attempt to insert new records.

```markup
<SyncKey>
        <SyncKeyColumnReference name="Email Address" />
</SyncKey>
<NewRecordBehaviour type="IGNORE" />
<ChangedRecordBehaviour type="UPDATE" />
<DroppedRecordBehaviour type="IGNORE"/>
```

## Post processing

You can add post sync scripts to a data sync configuration that run after the data sync has been completed.

For example, you can run a query to update the assignee on a lead after the leads data sync runs.

For example, a query that only updates where the assignee is empty, except for a high-value lead, where it's reassigned to the most senior person on each team (based on another table in the instance that has the seniority and team of each sales director).

```markup
<CinchyTableTarget model="" domain="Revenue" table="Leads" suppressDuplicateErrors="true" >
     <PostSyncScripts>
          <PostSyncScript name="Script1" timeout="60">
               <CQL>INSERT CQL HERE</CQL>
          </PostSyncScript>
     </PostSyncScripts>
</CinchyTableTarget>
```

## No unique identifier

If you have a file source (delimited, csv or excel) and you want to sync data into Cinchy that doesn't have a sync key, you can add a calculated column for the row number of that record.

```markup
<CalculatedColumn name="RowNumber" formula="row_number()" dataType="Number" /
```

You will also want to add a unique calculated column for the file name to be able to re-run the data sync if any failures occur.

## **Bi-directional sync**

To run a bi-directional sync, you need to identify a source system unique identifier and run the following four data syncs for bidirectional syncing.

If one of the systems can't create new records, you can omit the data sync configuration where you create new records in the other system.

### 1. Sync from external source into Cinchy (new records)

Run a data sync from the source into Cinchy filtering the source by records where the Cinchy ID (a custom field you create in the source) is empty. Insert these records into Cinchy and make sure to populate the source unique identifier column in Cinchy.

### 2. Sync from Cinchy into external source (new records)

You can also sync data from Cinchy to the external source by inserting any records where a source unique identifier is empty.

### 3. Sync from external Source into Cinchy (existing records)

Now that all records exist in both the external system and Cinchy, you can sync data based on the target system's unique ID. iIn this case, you are syncing data from the external source into Cinchy based on the Cinchy ID. Filter out records where Cinchy ID is null here to avoid errors; the sync will pick up the new records the next time the sync runs.

### 4. Sync from Cinchy into external source (existing records)

You can sync any data changes from Cinchy into the external source using the external unique identifier as the sync key, filtering out records in Cinchy where the external identifier is empty.

## Caching query data

To run intensive summary queries on the platform, you must create a data sync to cache the data in a Cinchy table. To do so, sync the results of your Cinchy query into a Cinchy table with the same schema. You can then schedule the CLI to run as often as you would like your cache to expire. You can point other queries or reports to the query from this table, rather than the complex query itself.

## Configuration changes

### Adding fields

To add more fields to sync, make sure that the columns are in the target system first. You can then swap in the new data sync configuration whenever and it will get picked up for future execution.

### Removing fields

To remove fields from a data sync, swap out your data sync configuration. You can optionally delete the field in the source or target system afterward.

### Changing column mapping

If you aren't adding or deleting fields, swap out your sync configuration. To add or remove fields, follow the guidelines above for when to swap in the config versus making the data model changes (add columns first, swap out config, validate config, delete unneeded columns).

### Changing sync key

If you want to change the sync key, swap out your data sync configuration. It's a good idea to check if your new sync key is unique in both your source and target. The CLI worker will sync using the first record it finds in the source to the first record it finds in the target. Checking for duplicate sync keys will allow you to understand whether any unexpected behaviour will occur.\
