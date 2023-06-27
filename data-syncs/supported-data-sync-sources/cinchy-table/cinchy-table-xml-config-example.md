# Cinchy Table XML Config Example

## 1. **Overview**

**This page** highlights a few example XML configs that you can review when setting up your own Cinchy Table data source.

You can review the source only example or the full example that shows both source and destination.

## 2. Source Only Example

The below example shows what the source parameters would look like in XML.

```xml
<?xml version="1.0" encoding="utf-16"?>
<BatchDataSyncConfig name="Batch Cinchy Table to Mongo Collection" version="1.0.0" xmlns="http://www.cinchy.co">
    <CinchyTableDataSource domain="Sales" table="Customer Info" suppressDuplicateErrors="false">
        <Schema>
            <Column name="Name" dataType="Text" trimWhitespace="true" isMandatory="false" validateData="false"/>
            <Column name="Customer Number" dataType="Number" isMandatory="false" validateData="false"/>
    </CinchyTableDataSource>
```

## 3. Full Example

**Example Use Case:** You want to set up a **batch sync,** that you can run when needed, between a Cinchy Table and a MongoDB Collection. This sync will push out Client Name and Customer Number information.

```xml
<?xml version="1.0" encoding="utf-16"?>
<BatchDataSyncConfig name="Batch Cinchy Table to Mongo Collection" version="1.0.0" xmlns="http://www.cinchy.co">
    <CinchyTableDataSource domain="Sales" table="Customer Info" suppressDuplicateErrors="false">
        <Schema>
            <Column name="Client Name" dataType="Text" trimWhitespace="true" isMandatory="false" validateData="false"/>
            <Column name="Customer Number" dataType="Number" isMandatory="false" validateData="false"/>
    </CinchyTableDataSource>
    <MongoCollectionTarget reconcileData="true" connectionString="V9xIf5DJNRQFkOn/y5AL7YdMVFR+ZAXeMRW4HC2ZfEAA4MTfBNJ7Z9kAtspWMtkyHDn2G8AJ1N9YeTMYcudKtoeLnA3P9Y8vSdVjD+QDOc/AHZEXYMvD8DgNThXo/yxusHQ0z6HLaXJ7mwlkv6a+4AN8Mj7rDMbe2c6gG/uTZKmvKFr1yNRyYoGwE792DGUNErrJ72nmPScmPGNOYjsSzkLLFHnRZqJClc4/aDekRkbdAc3/9MuLjFzBwa+OHoB54M=" database="QA1" collection="BatchTargetForQA1">
        <ColumnMappings>
            <ColumnMapping sourceColumn="Client Name" targetColumn="name"/>
            <ColumnMapping sourceColumn="Customer Number" targetColumn="Customer Number"/>
        </ColumnMappings>
        <SyncKey readonly="false">
            <SyncKeyColumnReference name="id"/>
        </SyncKey>
        <NewRecordBehaviour type="INSERT"/>
        <DroppedRecordBehaviour type="DELETE"/>
        <ChangedRecordBehaviour type="UPDATE"/>
        <PostSyncScripts/>
    </MongoCollectionTarget>
</BatchDataSyncConfig>
```
