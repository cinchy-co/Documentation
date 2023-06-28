# Sync Behaviour

## 1. Overview

When configuring a data sync you must set your sync behaviour. You have two options for this: **Full File or Delta.**

**Full File syncs** intake both the source and the destination data and reconcile the records by matching up the sync key. This determines any differences and allows it to perform updates, inserts, ignores, or deletes at the destination.

**Delta syncs** skip the reconciliation process. In batch syncs, it simply grabs records from the source and inserts it into the destination. In real-time syncs, it may act differently depending on the event type. For example, when using the Cinchy Event Broker/CDC with an insert event, a delta sync will insert the data into the destination, an update event will update, etc.

Delta syncs also have the option to provide an **"Action Type Column"** for REST API destinations. This reads the value of the source record from a specified column. If the value is "INSERT", then it inserts the record, "UPDATE", then it updates, "DELETE", then it deletes

## 2. Full File Sync

When using the Full File synchronization pattern there are two distinct sections that must be configured: the Sync Key and the Sync Record Behaviour _(Image 1)._

<figure><img src="../../.gitbook/assets/image (383).png" alt=""><figcaption><p>Image 1: Full File Syncs</p></figcaption></figure>

### 2.1 Sync Key

**The Sync Key** is used as a unique key reference when syncing the data from the data source into your destination. It is used to match up the data between the source and the target, which allows for updates to occur on changed records.

To set this using a config XML, use the following guide:

* **Elements:** \<SyncKeyColumnReference>
  * This is used in the [\<SyncKey>](https://cinchy.atlassian.net/wiki/spaces/KB/pages/196804691) element when specifying which columns in the Target Table to be utilized as a unique key for the syncing process.
* Contained-In: [\<SyncKey>](https://cinchy.atlassian.net/wiki/spaces/KB/pages/196804691)
* Attributes: _name._ The name of a column in the destination that you are syncing data into.

```markup
<SyncKeyColumnReference
    name:"string">
</SyncKeyColumnReference>
```

### **2.2 Sync Record Behaviour**

**The Sync Record Behaviour** is broken down into three subsections which define what action will be taken on certain records _(Image 2)._

<figure><img src="../../.gitbook/assets/image (384).png" alt=""><figcaption><p>Image 2: Sync Record Behaviour</p></figcaption></figure>

{% hint style="warning" %}
Values in the attributes section of the config XML for record behaviour are **case sensitive.**
{% endhint %}

#### **2.2.1 New Record Behaviour**

**New Record Behaviour** defines what action is taken when a new record is found in the sync source. This can be either **Insert or Ignore.**

To set this using a config XML, use the following guide:

| Attribute | Description                                       | Values                                                                                                                                                                                |
| --------- | ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type      | The type defines the action upon the new record.  | <p>It can either be "INSERT" or "IGNORE".</p><ul><li><strong>"INSERT"</strong> will insert the new record.</li><li><strong>"IGNORE"</strong> will do nothing to the record.</li></ul> |

```xml
<NewRecordBehaviour type="INSERT" />
```

#### 2.2.2 Dropped Record Behaviour

**Dropped Record Behaviour** defines what action is taken when a new record is not found in the sync source, but exists in the target. This can be either **Delete, Ignore, or Expire.**

To set this using a config XML, use the following guide:

| Attribute                | Description                                                         | Values                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ------------------------ | ------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type                     | The type defines the action upon the dropped record.                | <p>It can either be "IGNORE", "EXPIRE", or "DELETE".</p><ul><li><strong>"IGNORE"</strong> will do nothing to the record.</li><li><strong>"EXPIRE"</strong> will populate a specified expiration timestamp field as the current time. AN <strong>expirationTimestampField</strong> must be provided. This is a reference to a date/time column in the target that should be updated with the execution timestamp if the record is dropped.</li><li><strong>"DELETE"</strong> will delete dropped records in the target data set.</li></ul> |
| expirationTimestampField | This attribute is only applicable if the type is equal to "EXPIRE". | The expirationTimestampField is the name of an existing date field to be filled with the current time.                                                                                                                                                                                                                                                                                                                                                                                                                                    |

```xml
<DroppedRecordBehaviour
    type="EXPIRE"
    expirationTimestampField="string">
    ...
</DroppedRecordBehaviour>
```

#### 2.2.3 Changed Record Behaviour <a href="#id-less-than-droppedrecordbehaviour-greater-than-attributes" id="id-less-than-droppedrecordbehaviour-greater-than-attributes"></a>

**Changed Record Behaviour** defines what action is taken when a new record with a sync key is found in the sync source and also exists in the target. This can be either **Update or Ignore.**

To set this using a config XML, use the following guide:

| Attribute | Description                                      | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| --------- | ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type      | The type defines the action upon the new record. | <p>It can either be "UPDATE", "IGNORE", or "CONDITIONAL".</p><ul><li><strong>"IGNORE"</strong> will do nothing to the record.</li><li><strong>"UPDATE"</strong> will update the record.</li><li><strong>"CONDITIONAL"</strong> will open a new UI section allowing you to define Conditions upon which to update your records. <a href="sync-behaviour.md#full-file-sync-conditional-changed-record-behaviour">Review Appendix A</a> for more information on the Conditional behaviour.</li></ul> |

```xml
<ChangedRecordBehaviour type="UPDATE" />
```

## Delta Sync

When using the Delta synchronization pattern there is one optional configuration that you can choose to provide when running a sync with a REST API destination _(Image 3)._

The **Action Type Column** reads the value of the source record from a specified column. If the value is "INSERT", then it inserts the record, "UPDATE", then it updates, "DELETE", then it deletes.

<figure><img src="../../.gitbook/assets/image (445).png" alt=""><figcaption><p>Image 3: Delta Syncs</p></figcaption></figure>

## Appendix A

### Full File Sync - Conditional Changed Record Behaviour

Added in Cinchy v5.6, the **Changed Record Behaviour - Conditional** feature allows you to define specific conditions upon which to update your records _(Image 4)._

* Multiple Conditions can be added to a single data sync by using the **AND/OR and +Rule buttons.**
* You are able to group your Rules into a Ruleset by using the **+Ruleset button.**
* If your Condition evaluates to **true** then it will update your records
* **The left-most drop down** is used to select either a source or a target column as defined in your Source and Destination tabs
* **The centre drop-down** is used to select from the following options:
  * \=
  * !=
  * Contains
  * Is Null
  * Is Not Null
* **The right-most drop-down** can either be used to:
  * A plain value (ex: text, numerical, etc.) This will adjust based on the column data type picked in the left-most drop down. For example, if in the source schema the column is a date, then it renders a date picker.
  * Select either a source or a target column as defined in your Source and Destination tabs (when used in conjunction with the **Use Columns** checkbox)

<figure><img src="../../.gitbook/assets/image (549).png" alt=""><figcaption><p>Image 4: Conditional record behaviours</p></figcaption></figure>

For example, the below condition would only update records where the **target column "Name" is null** _(Image 5)._

<figure><img src="../../.gitbook/assets/image (62).png" alt=""><figcaption><p>Image 5: Conditional Example</p></figcaption></figure>
