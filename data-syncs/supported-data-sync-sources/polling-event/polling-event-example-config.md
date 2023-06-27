# Polling Event Example Config

## 1. XML Example

This example XML uses the following values:

<table><thead><tr><th width="343.2339286910899">Value</th><th width="189">Description</th><th>Example</th></tr></thead><tbody><tr><td>Column Name</td><td>The name(s) of your source column(s)</td><td>"Id"<br>"Name"<br>"Age"<br>"Address"<br>Salary"</td></tr><tr><td>dataType</td><td>The data type of your source column</td><td>"Number"<br>"Text"</td></tr><tr><td>isMandatory</td><td>Whether the column is mandatory or not</td><td>"false"</td></tr><tr><td>validateData</td><td>Whether the column data needs to be validated or not</td><td>"false"</td></tr></tbody></table>

### 1.1 Blank XML Example

```xml
<BatchDataSyncConfig name="Data Polling" version="1.0.0" xmlns="http://www.cinchy.co">
    <PollingEventBrokerDataSource>
        <Schema>
            <Column name="" dataType="" isMandatory="" validateData=""/>
            <Column name="" dataType="" isMandatory="" validateData=""/>
        </Schema>
    </PollingEventBrokerDataSource>
```

### 1.2 Populated XML Example

```xml
<?xml version="1.0" encoding="utf-16"?>
<BatchDataSyncConfig name="Data Polling" version="1.0.0" xmlns="http://www.cinchy.co">
    <PollingEventBrokerDataSource>
        <Schema>
            <Column name="ID" dataType="Number" isMandatory="false" validateData="false"/>
            <Column name="NAME" dataType="Text" isMandatory="false" validateData="false"/>
            <Column name="AGE" dataType="Number" isMandatory="false" validateData="false"/>
            <Column name="ADDRESS" dataType="Text" isMandatory="false" validateData="false"/>
            <Column name="SALARY" dataType="Number" isMandatory="false" validateData="false"/>
        </Schema>
    </PollingEventBrokerDataSource>
```

### 1.3 Example with Destination

```xml
<?xml version="1.0" encoding="UTF-16"?>
<BatchDataSyncConfig xmlns="http://www.cinchy.co" name="Data Polling" version="1.0.0">
  <PollingEventBrokerDataSource>
    <Schema>
      <Column name="ID" dataType="Number" isMandatory="false" validateData="false" />
      <Column name="NAME" dataType="Text" isMandatory="false" validateData="false" />
      <Column name="AGE" dataType="Number" isMandatory="false" validateData="false" />
      <Column name="ADDRESS" dataType="Text" isMandatory="false" validateData="false" />
      <Column name="SALARY" dataType="Number" isMandatory="false" validateData="false" />
    </Schema>
  </PollingEventBrokerDataSource>
  <CinchyTableTarget reconcileData="true" domain="Automation" table="Customer1" suppressDuplicateErrors="true">
    <ColumnMappings>
      <ColumnMapping sourceColumn="ID" targetColumn="Id" />
      <ColumnMapping sourceColumn="NAME" targetColumn="Name" />
      <ColumnMapping sourceColumn="AGE" targetColumn="Age" />
      <ColumnMapping sourceColumn="ADDRESS" targetColumn="Address" />
      <ColumnMapping sourceColumn="SALARY" targetColumn="Salary" />
    </ColumnMappings>
    <SyncKey>
      <SyncKeyColumnReference name="Id" />
    </SyncKey>
    <NewRecordBehaviour type="INSERT" />
    <DroppedRecordBehaviour type="DELETE" />
    <ChangedRecordBehaviour type="UPDATE" />
    <PostSyncScripts />
  </CinchyTableTarget>
</BatchDataSyncConfig>
```

## 2. Connections UI Example

This example shows you how to set up a polling event source using the Connections UI.

### 2.1 Schema

<figure><img src="../../../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

Our column parameters are set as follows:

| Column Name | Data Type |
| ----------- | --------- |
|  ID         | Number    |
| NAME        | Text      |
| AGE         | Number    |
| ADDRESS     | Text      |
| SALARY      | Number    |

<figure><img src="../../../.gitbook/assets/image (351).png" alt=""><figcaption></figcaption></figure>
