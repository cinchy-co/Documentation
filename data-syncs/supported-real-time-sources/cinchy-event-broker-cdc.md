# Cinchy Event Broker/CDC

## Overview

The Cinchy Event Broker/CDC is an event streaming source used to listen for changes on Cinchy tables and push those changes to various data sync destinations.

## The Listener Config Table

To set up an Stream Source, you must  navigate to the Listener Config table and insert a new row for your data sync _(Image 1)_. Most of the columns within the Listener Config table persist across all Stream Sources, however exceptions will be noted. You can find all of these parameters and their relevant descriptions in the tables below.

<figure><img src="../../.gitbook/assets/image (481).png" alt=""><figcaption><p>Image 1: The Listener Config table</p></figcaption></figure>

{% tabs %}
{% tab title="General Parameters" %}
The following column parameters can be found in the Listener Config table:

<table><thead><tr><th width="201">Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Name</td><td><strong>Mandatory.</strong> Provide a name for your Listener Config.</td><td>CDC Real-Time Sync</td></tr><tr><td><strong>Event Connector Type</strong></td><td><strong>Mandatory.</strong> Select your Connector type from the drop down menu.</td><td>Cinchy CDC</td></tr><tr><td><strong>Topic</strong></td><td><strong>Mandatory.</strong> This field is expecting a JSON formatted value specific to the connector type you are configuring.</td><td>See the <strong>Topic</strong> tab.</td></tr><tr><td><strong>Connection Attributes</strong></td><td><strong>Mandatory.</strong> This field is expecting a JSON formatted value specific to the connector type you are configuring.</td><td>See the <strong>Connection Attributes</strong> tab.</td></tr><tr><td><strong>Status</strong></td><td><strong>Mandatory.</strong> This value refers to whether your listener config/real-time sync is turned on or off. Make sure you keep this set to Disabled until you are confident you have the rest of your data sync properly configured first.</td><td>Disabled</td></tr><tr><td><strong>Data Sync Config</strong></td><td><strong>Mandatory.</strong> This drop down will list all of the data syncs on your platform. Select the one that you want to use for your real-time sync.</td><td>CDC Data Sync</td></tr><tr><td><strong>Subscription Expires On</strong></td><td><strong>This value is only relevant for Salesforce Stream Sources.</strong> This field is a timestamp that is auto-populated when it has successfully subscribed to a topic. </td><td></td></tr><tr><td><strong>Message</strong></td><td><strong>Leave this value blank when setting up your configuration.</strong> This field will auto-populate during the running of your sync with any relevant messages. For instance "Cinchy listener is running", or "Listener is disabled". </td><td></td></tr><tr><td><strong>Auto Offset Reset</strong></td><td><p><strong>Earliest, Latest or None.</strong> <br><br>In the case where the listener is started and either there is no last message ID, or when the last message ID is invalid (due to it being deleted or it's just a new listener), it will use this column as a fallback to determine where to start reading events from.<br></p><p><strong>Earliest</strong> will start reading from the beginning on the queue (when the CDC was enabled on the table). This might be a suggested configuration if your use case is recoverable or re-runnable and if you need to reprocess all events to ensure accuracy.<br><br><strong>Latest</strong> will fetch the last value after whatever was last processed. This is the typical configuration.<br><br><strong>None</strong> will not read start reading any events.<br><br>You are able to switch between Auto Offset Reset types after your initial configuration through the below steps:<br>1. Navigate to the <strong>Listener Config table.</strong><br>2. Re-configure the <strong>Auto Offset Reset value.</strong><br>3. Set the <strong>"Status"</strong> column of the Listener Config to <strong>"Disabled".</strong><br>4. Navigate to the <strong>Event Listener State table.</strong><br>5. Find the column that pertains to your data sync's Listener Config and <strong>delete it.</strong><br>6. Navigate back to the <strong>Listener Config table.</strong><br>7. Set the <strong>"Status"</strong> column of the Listener Config to <strong>"Enabled"</strong> in order for your new Auto Offset Reset configuration to take effect.</p></td><td>Latest</td></tr></tbody></table>
{% endtab %}

{% tab title="Topic" %}
The below table can be used to help create your Topic JSON needed to set up a real-time sync.

If you are creating a CDC listener config for a **Cinchy Event Triggered REST API data source**, pay in mind the following unique constraints:

* **Column names** in the listener config should not contain spaces. If they do, they will be automatically removed. _E.g. a column named First Name will become @FirstName_
* The replacement variable names are **case sensitive.**
* **Column names** in the listener config should not be prefixes of other column names. E_.g. if you have a column called "Name", you shouldn't have another called "Name2" as the value of @Name2 may end up being replaced by the value of @Name suffixed with a "2"._

| Parameter  | Description                                                                                                                                                                                                                                                                                                                                                                           | Example                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Table GUID | **Mandatory.** The GUID of the table whose notifications you wish to consume. You can find this in the **Design Table** screen.                                                                                                                                                                                                                                                       | 16523e54-4242-4156-835a-0e572e862304                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Column(s)  | <p><strong>Mandatory.</strong> The names of the columns you wish to include in your sync.<br><br><strong>Note:</strong> If you will be using the <a href="https://cli.docs.cinchy.com/builder-guide/configuring-a-data-sync/supported-data-sources/cinchy-event-broker">runQuery=true</a> parameter in your data sync, you only need to include the Cinchy Id in the topic JSON. </p> | <p>Name<br>Age</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| BatchSize  | The desired result batch size. This will **default to 1** if not passed in. The maximum batch size is 1000; using a number higher than that will result in a **Bad Request** response.                                                                                                                                                                                                | 10                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Filter     | <p><strong>Optional.</strong> When CDC is enabled, you can set a filter on columns where you are capturing changes in order to receive specific data.<br></p>                                                                                                                                                                                                                         | <p><strong>Optional.</strong> When CDC is enabled, you can set a filter on columns where you are capturing changes in order to receive specific data.</p><p><br><strong>In the below example,</strong> we will only trigger changes on newly approved records by using the "New" filter to include all records where the [Approval State] is equal to 'Approved' AND the record is <strong>New</strong>, i.e. has not been synced to the target yet. <br><br>The filter also uses the "Old" filter to disinclude all records where the [Approval State] is not equal to approved AND the record is <strong>Old</strong>, i.e. has already been synced to the target.<br><br>Example:</p><p><em>"filter": "New.[Approval State] = 'Approved' AND Old.[Approval State] != 'Approved'"</em><br><br><em>(Hint: Click on the below image to enlarge)</em><br><img src="../../.gitbook/assets/image (659).png" alt=""></p> |

**Example Topic JSON**

```
{
    "tableGuid": "16523e54-4242-4156-835a-0e572e862304",
    "fields": [
        {
            "column": "Name"
        },
        {
            "column": "Age"
        }
    ],
"filter": "New.[Approval State] = 'Approved' AND Old.[Approval State] != 'Approved'",
   "batchSize": 10
}
```
{% endtab %}

{% tab title="Connections Attributes" %}
You do not need to provide Connections Attributes when using the Cinchy CDC Stream Source, however you cannot leave the field blank. Instead, insert the below text into the column:

```
{}
```
{% endtab %}
{% endtabs %}

## Appendix A

### messageKeyExpression

Each of your Event Listener message keys a message key. **By default, this key is dictated by the Cinchy ID of the record being changed.**

When the worker processes your Event Listener messages, it does so in batches, and for efficiency and to guarantee order, messages that contain the same key will not be processed in the same batch.

The **messageKeyExpression** property allows you to change the default message key to something else.

#### Possible Use Case

* Ensuring records with the same message key can be updated with the proper ordering to reflect an accurate collaboration log history.

#### Example Syntax

In this example, we want the message key to be based on the \[Employee Id] and \[Name] column of the table that CDC is enabled on.

```
{ "messageKeyExpression": "CONCAT(New.[Employee Id], '-', New.[Name])", … }
```

## Appendix B

### Old vs New Filter

The Cinchy Event Broker/CDC Stream Source has the unique capability to use "Old" and "New" parameters when filtering data. This filter can be a powerful tool for ensuring that you sync only the specific data that you want.

The "New" and "Old" parameters are based on updates to single records, not columns/rows.

**"New" Example:**

In the below filter, we only want to sync data where the \[Approval State] of a record is newly 'Approved'. For example, if a record was changed from 'Draft' to 'Approved', the filter would sync the record.

{% hint style="info" %}
Due to internal logic, newly created records will be tagged as both "New" **and** "Old".
{% endhint %}

```
"filter": "New.[Approval State] = 'Approved'
```

**"Old" Example:**

In the below filter, we only want to sync data where the \[Status] of a record was 'In Progress' but has since been updated to any other \[Status]. For example, if a record was changed from 'In Progress' to 'Done', the filter would sync the record.

{% hint style="info" %}
Due to internal logic, newly created records will be tagged as both "New" **and** "Old".
{% endhint %}

```
"filter": "Old.[Status] = 'In Progress'
```
