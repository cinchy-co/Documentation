# Cinchy Query XML Config Example

## 1. **Overview**

**This page** highlights a few example XML configs that you can review when setting up your own Cinchy Query data source.

You can review the source only example or the full example that shows both source and destination.

## 2. Source Only Example

The below example shows what the source parameters would look like in XML.

```xml
<?xml version=""1.0"" encoding=""utf-16""?>
<BatchDataSyncConfig name=""Open Approval Tasks"" version=""1.0.0"" xmlns=""http://www.cinchy.co"">
    <CinchyQueryDataSource domain=""Compliance"" name=""Open Tasks"" timeout=""120"">
        <Schema>
            <Column name=""Owner Cinchy Id"" dataType=""Text"" trimWhitespace=""true"" isMandatory=""false"" validateData=""false""/>
            <Column name=""Owner Group Id"" dataType=""Text"" trimWhitespace=""true"" isMandatory=""false"" validateData=""false""/>
            <Column name=""Due Date"" dataType=""Date"" isMandatory=""false"" validateData=""false""/>
            <Column name=""Task"" dataType=""Text"" trimWhitespace=""true"" isMandatory=""false"" validateData=""false""/>
        </Schema>
    </CinchyQueryDataSource>
```

## 3. Full Example

**Example Use Case:** You want to set up batch sync between a **Cinchy Query** and a **Cinchy Table**. You query polls for any unapproved timesheets, out of office requests, or sick hours and, if found, adds them to an "Open Approval Tasks" table.&#x20;

```xml
<?xml version=""1.0"" encoding=""utf-16""?>
<BatchDataSyncConfig name=""Open Approval Tasks"" version=""1.0.0"" xmlns=""http://www.cinchy.co"">
    <CinchyQueryDataSource domain=""Compliance"" name=""Open Tasks"" timeout=""120"">
        <Schema>
            <Column name=""Owner Cinchy Id"" dataType=""Text"" trimWhitespace=""true"" isMandatory=""false"" validateData=""false""/>
            <Column name=""Owner Group Id"" dataType=""Text"" trimWhitespace=""true"" isMandatory=""false"" validateData=""false""/>
            <Column name=""Due Date"" dataType=""Date"" isMandatory=""false"" validateData=""false""/>
            <Column name=""Task"" dataType=""Text"" trimWhitespace=""true"" isMandatory=""false"" validateData=""false""/>
        </Schema>
    </CinchyQueryDataSource>
    <CinchyTableTarget reconcileData=""true"" domain=""Cinchy Human Automation"" table=""Open Approval Tasks"" suppressDuplicateErrors=""false"">
        <ColumnMappings>
            <ColumnMapping sourceColumn=""Owner Cinchy Id"" targetColumn=""Owner"" linkColumn=""Cinchy User Id""/>
            <ColumnMapping sourceColumn=""Owner Group Id"" targetColumn=""Owner Group"" linkColumn=""Cinchy Id""/>
            <ColumnMapping sourceColumn=""Due Date"" targetColumn=""Due Date""/>
            <ColumnMapping sourceColumn=""Task"" targetColumn=""Task""/>
        <SyncKey>
            <SyncKeyColumnReference name=""Key""/>
        </SyncKey>
        <NewRecordBehaviour type=""INSERT""/>
        <DroppedRecordBehaviour type=""DELETE""/>
        <ChangedRecordBehaviour type=""UPDATE""/>
    </CinchyTableTarget>
</BatchDataSyncConfig>"
```
