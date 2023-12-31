# Data Polling

## 1. Overview

Version 5.4 of the Cinchy platform introduced data polling, which uses the Cinchy Event Listener to continuously monitor and sync data entries from your SQLServer or DB2 server into your Cinchy table. This capability makes data polling a much easier, effective, and streamlined process and avoids implementing the complex orchestration logic that was previous necessary.

This page outlines the necessary Listener Config values that need to be used prior to setting up your data sync.

## 2. The Listener Config Table

To set up an Stream Source, you must  navigate to the Listener Config table and insert a new row for your data sync _(Image 1)_. Most of the columns within the Listener Config table persist across all Stream Sources, however exceptions will be noted. You can find all of these parameters and their relevant descriptions in the tables below.

<figure><img src="../../.gitbook/assets/image (472).png" alt=""><figcaption><p>Image 1: The Listener Config table</p></figcaption></figure>

{% tabs %}
{% tab title="General Parameters" %}
The following column parameters can be found in the Listener Config table:

<table><thead><tr><th width="201">Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Name</td><td><strong>Mandatory.</strong> Provide a name for your Listener Config.</td><td>Data Polling Real-Time Sync</td></tr><tr><td><strong>Event Connector Type</strong></td><td><strong>Mandatory.</strong> Select your Connector type from the drop down menu.</td><td>Data Polling</td></tr><tr><td><strong>Topic</strong></td><td><strong>Mandatory.</strong> This field is expecting a JSON formatted value specific to the connector type you are configuring.</td><td>See the <strong>Topic</strong> tab.</td></tr><tr><td><strong>Connection Attributes</strong></td><td><strong>Mandatory.</strong> This field is expecting a JSON formatted value specific to the connector type you are configuring.</td><td>See the <strong>Connection Attributes</strong> tab.</td></tr><tr><td><strong>Status</strong></td><td><strong>Mandatory.</strong> This value refers to whether your listener config/real-time sync is turned on or off. Make sure you keep this set to Disabled until you are confident you have the rest of your data sync properly configured first.</td><td>Disabled</td></tr><tr><td><strong>Data Sync Config</strong></td><td><strong>Mandatory.</strong> This drop down will list all of the data syncs on your platform. Select the one that you want to use for your real-time sync.</td><td>Data Polling Data Sync</td></tr><tr><td><strong>Subscription Expires On</strong></td><td><strong>This value is only relevant for Salesforce Stream Sources.</strong> This field is a timestamp that is auto-populated when it has successfully subscribed to a topic. </td><td></td></tr><tr><td><strong>Message</strong></td><td><strong>Leave this value blank when setting up your configuration.</strong> This field will auto-populate during the running of your sync with any relevant messages. For instance "Cinchy listener is running", or "Listener is disabled". </td><td></td></tr><tr><td><strong>Auto Offset Reset</strong></td><td><p><strong>Earliest, Latest or None.</strong> <br><br>In the case where the listener is started and either there is no last message ID, or when the last message ID is invalid (due to it being deleted or it's just a new listener), it will use this column as a fallback to determine where to start reading events from.<br></p><p><strong>Earliest</strong> will start reading from the beginning on the queue (when the CDC was enabled on the table). This might be a suggested configuration if your use case is recoverable or re-runnable and if you need to reprocess all events to ensure accuracy.<br><br><strong>Latest</strong> will fetch the last value after whatever was last processed. This is the typical configuration.<br><br><strong>None</strong> will not read start reading any events.<br><br>You are able to switch between Auto Offset Reset types after your initial configuration through the below steps:<br>1. Navigate to the <strong>Listener Config table.</strong><br>2. Re-configure the <strong>Auto Offset Reset value.</strong><br>3. Set the <strong>"Status"</strong> column of the Listener Config to <strong>"Disabled".</strong><br>4. Navigate to the <strong>Event Listener State table.</strong><br>5. Find the column that pertains to your data sync's Listener Config and <strong>delete it.</strong><br>6. Navigate back to the <strong>Listener Config table.</strong><br>7. Set the <strong>"Status"</strong> column of the Listener Config to <strong>"Enabled"</strong> in order for your new Auto Offset Reset configuration to take effect.</p></td><td>Latest</td></tr></tbody></table>
{% endtab %}

{% tab title="Topic" %}
The below table can be used to help create your Topic JSON needed to set up a real-time sync.

| Parameter               | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Example                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CursorConfiguration     | <p><strong>Mandatory.</strong> The parameters here are used in a basic query which searches for all records in a particular table.</p><p></p><p><em>Note that in our example we need to use a sub-query to prevent an infinite loop if the "CursorColumn" parameter is not unique.</em></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | <p></p><p>Example basic query: </p><pre class="language-sql" data-overflow="wrap"><code class="lang-sql">SELECT Id, Name
FROM [SourceTable]
WHERE Id IN (SELECT TOP (100) Id
    FROM [SourceTable]
    WHERE Id > 0 AND Name IS NOT NULL
    ORDER BY Id)
 AND Id > 0 AND Name IS NOT NULL
</code></pre>                                                                                                                                      |
| FromClause              | **Mandatory.** This must contain at least the table name but can also contain Joined tables as written in SQL language.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Example: **\[Source Table]**                                                                                                                                                                                                                                                                                                                                                                                                                   |
| CursorColumn            | **Mandatory.** Column name that is used in any 'WHERE' condition(s) and for ordering the result of a query                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Example: **\[Id]**                                                                                                                                                                                                                                                                                                                                                                                                                             |
| BatchSize               | **Mandatory.** Minimum size of a batch of data per query. This can be larger to prevent infinite loops if the CursorColumn is not unique.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Example: **100**                                                                                                                                                                                                                                                                                                                                                                                                                               |
| FilterCondition         | All filtering options used in any 'WHERE' condition(s) of the query                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Example: **"Name IS NOT NULL"**                                                                                                                                                                                                                                                                                                                                                                                                                |
| Columns                 | **Mandatory.** A list of columns that we want to show in a result.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Example:**"Id", "Name"**                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ReturnDataConfiguration | <p></p><p>The parameters here are used in more complex queries.<br><br><em>In our example, we have 2 related tables, but want to show the contents of one of them based on the 'CursorColumn' from a second table. Since Timestamp values are not unique, we need to find all combinations of Id, Timestamp that match the filter condition in a subquery, and then join this result with the outer-query to get the final result.</em><br><br>Note that i<em>n "ReturnDataConfiguration", our parameters area of concern is everything outside of first open parenthesis "(" and last closing parenthesis ")", i.e.:</em></p><pre class="language-sql"><code class="lang-sql">FROM
(
...
) AS t INNER JOIN [Table1] ts ON ts.[Id] = t.[Id]
WHERE ts.[Id] > 0
ORDER BY Id
</code></pre> | <p></p><p>Example complex query:</p><pre class="language-sql"><code class="lang-sql">SELECT ts.[Id],ts.[Name] FROM
(
SELECT Id,Timestamp
FROM [Table2]
WHERE Timestamp IN (SELECT TOP (2)
Timestamp
FROM [Table2]
WHERE Timestamp > '2022-11-18 11:34:09 AM'
AND Timestamp &#x3C;= '2022-
11-19 11:34:09 AM'
AND 1=1
ORDER BY Timestamp)
AND 1=1
) AS t
INNER JOIN [Table1] ts ON ts.[Id] = t.[Id]
WHERE ts.[Id] > 0
ORDER BY Id
</code></pre> |
| CursorAlias             | **Mandatory.** This is the alias for a subquery result table. It is used in 'JoinClause', and can be used in 'Columns' if we want to return values from a subquery table.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Example: **"t"**                                                                                                                                                                                                                                                                                                                                                                                                                               |
| JoinClause              | **Mandatory.** Our result table to which we join the subquery result, plus the condition of the join.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Example: **\[Table1] ts ON ts.\[Id] = t.\[Id]**                                                                                                                                                                                                                                                                                                                                                                                                |
| FilterCondition         | All filtering options used in any 'WHERE' conditions.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Example: "**ts.\[Id] > 0"**                                                                                                                                                                                                                                                                                                                                                                                                                    |
| OrderByClause           | **Mandatory.** This is the column(s) that we want to order our final result by.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Example: "**Id"**                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Columns                 | **Mandatory**. A list of columns that we want to show in the final result.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Example: "**ts.\[Id]" "ts.\[name]"**                                                                                                                                                                                                                                                                                                                                                                                                           |
| Delay                   | **Mandatory.** This represents the delay, in second, between data sync cycles once it no longer finds any new data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Example: **10**                                                                                                                                                                                                                                                                                                                                                                                                                                |
| messageKeyExpresssion   | Optional, but recommended to mitigate data loss. **See Appendix A** for more information on this parameter.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | id                                                                                                                                                                                                                                                                                                                                                                                                                                             |

**Example Topic JSON**

```
{
  "CursorConfiguration": {
    "FromClause": "[Source Table]",
    "CursorColumn": "Id",
    "BatchSize": 100,
    "FilterCondition": "Name IS NOT NULL",
    "Columns": [
      "Id", "Name"
    ]
  },
  "ReturnDataConfiguration": {
    "CursorAlias": "t",
    "JoinClause": "[Table1] ts ON ts.[id] = t.[id]",
    "FilterCondition": "ts.[id] > 0",
    "OrderByClause": "id",
    "Columns": [
      "ts.[Id]"
      "ts.[Name]",
    ]
  
  },
  "Delay": 10
}
```
{% endtab %}

{% tab title="Connection Attributes" %}
The below table can be used to help create your Connection Attributes JSON needed to set up a real-time sync.

| Parameter        | Description                                                               | Example |
| ---------------- | ------------------------------------------------------------------------- | ------- |
| databaseType     | **Mandatory.** TSQL or DB2                                                | TSQL    |
| connectionString | **Mandatory.** This should be the connection string for your data source. |         |

```
{
  "databaseType": "TSQL",
  "connectionString": "Server=;Database=;User ID=cinchy;password=example;Trusted_Connection=False;Connection Timeout=30;Min Pool Size=10;",
}
```
{% endtab %}
{% endtabs %}

## Appendix A

### **messageKeyExpression**

The **messageKeyExpression** parameter is an optional, but recommended, parameter that can be used to ensure that you aren't faced with a unique constraint violation during your data sync. This violation could occur if both an insert and an update statement happened at nearly the same time. **If you choose not to use the messageKeyExpression parameter, you could face data loss in your sync.**

{% hint style="info" %}
This parameter was added to the Data Polling event stream in Cinchy v5.6.
{% endhint %}

Each of your Event Listener message keys a message key. **By default, this key is unique for every message in the queue.**

When the worker processes your Event Listener messages it does so in batches and, for efficiency and to guarantee order, messages that contain the same key will not be processed in the same batch.

The **messageKeyExpression** property allows you to change the default message key to something else.

Example:

```
  "MessageKeyExpression": "id"
```
