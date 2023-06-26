# Kafka Topic Example Config

## 1. Overview

In this example, we are syncing **from a Kafka Topic** source **to a Cinchy Table** target.

We want to sync the following data from Kafka and map it to the appropriate column in the **"Sync Target 2"** table in the **"Kafka Sync"** domain.

| Kafka Source | Cinchy Column |
| ------------ | ------------- |
| $.employeeId | Employee Id   |
| $.name       | Name          |

## 2. UI Example

This is what the Connections UI will look like with the aforementioned example parameters and data.

### 2.1 Source Tab

Your source tab should be set to **"Kafka Topic"** and have the following information _(Image 1):_

{% hint style="info" %}
Tip: Click on an image in this document to enlarge it.
{% endhint %}

| Column 1 (Standard Column) Parameters | Example Data |
| ------------------------------------- | ------------ |
| Name                                  | $.employeeid |
| Alias                                 | Employee Id  |
| Data Type                             | Number       |

| Column 2 ( Standard Column) Parameters | Example Data |
| -------------------------------------- | ------------ |
| Name                                   | $.name       |
| Alias                                  | Name         |
| Data Type                              | Text         |
| Trim Whitespace                        | True         |

<figure><img src="../../../.gitbook/assets/image (275).png" alt=""><figcaption><p>Image 1: Inserting your source data</p></figcaption></figure>

### 2.2 Destination Tab

Your destination tab should be set to **"Cinchy Table"**, and have the following information _(Image 2):_

**Domain:** The domain where your destination table resides. In our example we are using the **"Kafka Sync" domain.**

**Table:** The name of your destination table. In our example we are using the **"Sync Target 2" table.**

**Degree of Parallelism:** This is the number of parallel batch inserts and updates that can be run. Set this to **1** for our example.

| Column 1 (Standard Column) Parameters | Example Data |
| ------------------------------------- | ------------ |
| Source Column                         | Employee Id  |
| Target Column                         | Employee Id  |

| Column 2 (Standard Column) Parameters | Example Data |
| ------------------------------------- | ------------ |
| Source Column                         | Name         |
| Target Column                         | Name         |

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption><p>Image 2: Inserting your destination data</p></figcaption></figure>

### 2.3 Sync Behaviour

Under the Sync Behaviour tab, we want to use the following parameters:

* **Synchronization Pattern:** Full File
* **Sync Key Column Reference Name:** Employee Id
* **New Record Behaviour:** Insert
* **Dropped Record Behaviour:** Delete
* **Change Record Behavior:** Update

## 3. XML Example

The following code is what the XML for our example connection would look like:

```xml
<?xml version="1.0" encoding="utf-16"?>
<BatchDataSyncConfig name="Kafka Sync" version="1.0.0" xmlns="http://www.cinchy.co">
    <KafkaTopicDataSource>
        <Schema>
            <Column name="$.employeeId" label="Employee Id" dataType="Number" isMandatory="false" validateData="false"/>
            <Column name="$.name" label="Name" dataType="Text" trimWhitespace="true" isMandatory="false" validateData="false"/>
        </Schema>
    </KafkaTopicDataSource>
    <CinchyTableTarget reconcileData="true" domain="Kafka Sync" table="Sync Target 2" suppressDuplicateErrors="false" degreeOfParallelism="1">
        <ColumnMappings>
            <ColumnMapping sourceColumn="$.employeeId" targetColumn="Employee Id"/>
            <ColumnMapping sourceColumn="$.name" targetColumn="Name"/>
        </ColumnMappings>
        <SyncKey>
            <SyncKeyColumnReference name="Employee Id"/>
        </SyncKey>
        <NewRecordBehaviour type="INSERT"/>
        <DroppedRecordBehaviour type="DELETE"/>
        <ChangedRecordBehaviour type="UPDATE"/>
        <PostSyncScripts/>
    </CinchyTableTarget>
</BatchDataSyncConfig>
```
