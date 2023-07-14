# Cinchy Event Broker/CDC XML Config Example

## Overview

This page highlights a few example XML configs that you can review when setting up your own Cinchy Event Broker/CDC data source.

You can review the source only example, the full example that shows both source and destination, and the listener config example.

## Source example

The below example shows what the source parameters would look like in XML.

```xml
<?xml version="1.0" encoding="utf-16"?>
<BatchDataSyncConfig name="New Hires" version="1.0.0" xmlns="http://www.cinchy.co">
    <CinchyEventBrokerDataSource runQuery="false">
        <Schema>
            <Column name="Name" dataType="Text" trimWhitespace="true" isMandatory="false" validateData="false"/>
            <Column name="Title" dataType="Text" trimWhitespace="true" isMandatory="false" validateData="false"/>
        </Schema>
    </CinchyEventBrokerDataSource>
```

## Full example

### Use case

You want to set up a **real-time sync** between two Cinchy tables so that any time specific data is added, updated, or deleted from Table A it gets propagated to Table B. As long as you enable change notifications on your Cinchy table, you can do so by setting up a data sync and listener config with your source as the Cinchy Event Broker/CDC.

```xml
<?xml version="1.0" encoding="utf-16"?>
<BatchDataSyncConfig name="New Hires" version="1.0.0" xmlns="http://www.cinchy.co">
    <CinchyEventBrokerDataSource runQuery="false">
        <Schema>
            <Column name="Name" dataType="Text" trimWhitespace="true" isMandatory="false" validateData="false"/>
            <Column name="Title" dataType="Text" trimWhitespace="true" isMandatory="false" validateData="false"/>
        </Schema>
    </CinchyEventBrokerDataSource>
    <CinchyTableTarget reconcileData="true" domain="sandbox" table="New Employees" suppressDuplicateErrors="false" degreeOfParallelism="1">
        <ColumnMappings>
            <ColumnMapping sourceColumn="Name" targetColumn="Name"/>
            <ColumnMapping sourceColumn="Title" targetColumn="Title"/>
        </ColumnMappings>
        <SyncKey readonly="false">
            <SyncKeyColumnReference name="Name"/>
        </SyncKey>
        <NewRecordBehaviour type="INSERT"/>
        <DroppedRecordBehaviour type="DELETE"/>
        <ChangedRecordBehaviour type="UPDATE"/>
        <PostSyncScripts/>
    </CinchyTableTarget>
</BatchDataSyncConfig>
```

## Listener config example

The following is the Cinchy CDC listener config Topic and Connection Attributes as it would be set for the above real time sync example to work.

#### Topic

```xml
{
    "tableGuid": "da747426-ca9f-4952-8738-a85c0d7d5c4f",
    "fields": [
        {
            "column": "Name"
        },
        {
            "column": "Title"
        }
    ],
}
```

#### Connection Attributes

```
{}
```
