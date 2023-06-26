# Polling Event

## 1. Overview

Version 5.4 of the Cinchy platform introduced data polling, a source option which uses the Cinchy Event Listener to continuously monitor and sync data entries from your Oracle, SQLServer, or DB2 server into your Cinchy table. This capability makes data polling a much easier, effective, and streamlined process and avoids implementing the complex orchestration logic that was previous necessary.

{% hint style="success" %}
The Polling Event source supports real-time syncs.
{% endhint %}

{% hint style="success" %}
The Polling Event Source supports Oracle, DB2 and SQL Server databases.
{% endhint %}

## 2. Info Tab

You can review the parameters that can be found in the info tab below _(Image 1)._

#### Values

| Parameter   | Description                                                                                                                                                                                       | Example            |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ |
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                    | Polling Event Sync |
| Variables   | **Optional.** Review our documentation on [Variables here ](../../building-data-syncs/advanced-settings/variables.md)for more information about this field.                                       |                    |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. **Inputting at least an Admin Group is mandatory.**  |                    |

<figure><img src="../../../.gitbook/assets/image (734).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## 3. Source Tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>Polling Event</td></tr></tbody></table>
{% endtab %}

{% tab title="Listener Configuration" %}
To set up a real-time sync, you must configure your Listener values. You can do so through the Connections UI.

Note that If there is more than one listener associated with your data sync, you will need to configure the addition listeners via [the Listener Configuration table.](../../supported-real-time-sync-stream-sources/the-listener-configuration-table.md)

#### Reset Behaviour

| Parameter             | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Example |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------- |
| **Auto Offset Reset** | <p><strong>Earliest, Latest or None.</strong> <br><br>In the case where the listener is started and either there is no last message ID, or when the last message ID is invalid (due to it being deleted or it's just a new listener), it will use this column as a fallback to determine where to start reading events from.<br></p><p><strong>Earliest</strong> will start reading from the beginning on the queue (when the CDC was enabled on the table). This might be a suggested configuration if your use case is recoverable or re-runnable and if you need to reprocess all events to ensure accuracy.<br><br><strong>Latest</strong> will fetch the last value after whatever was last processed. This is the typical configuration.<br><br><strong>None</strong> will not read start reading any events.<br><br>You are able to switch between Auto Offset Reset types after your initial configuration through the process outlined <a href="../../error-logging-and-troubleshooting.md">here.</a></p> | None    |

#### Topic JSON

The below table can be used to help create your Topic JSON needed to set up a real-time sync.

<table><thead><tr><th width="267.66666666666663">Parameter</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td>CursorConfiguration</td><td><p><strong>Mandatory.</strong> The parameters here are used in a basic query which searches for all records in a particular table.</p><p></p><p><em>Note that in our example we need to use a sub-query to prevent an infinite loop if the "CursorColumn" parameter is not unique.</em></p></td><td><p></p><p>Example basic query: </p><pre class="language-sql" data-overflow="wrap"><code class="lang-sql">SELECT Id, Name
FROM [SourceTable]
WHERE Id IN (SELECT TOP (100) Id
    FROM [SourceTable]
    WHERE Id > 0 AND Name IS NOT NULL
    ORDER BY Id)
 AND Id > 0 AND Name IS NOT NULL
</code></pre></td></tr><tr><td>FromClause</td><td><strong>Mandatory.</strong> This must contain at least the table name but can also contain Joined tables as written in SQL language.</td><td>Example: <strong>[Source Table]</strong></td></tr><tr><td>CursorColumn</td><td><strong>Mandatory.</strong> Column name that is used in any 'WHERE' condition(s) and for ordering the result of a query </td><td>Example: <strong>[Id]</strong></td></tr><tr><td>BatchSize</td><td><strong>Mandatory.</strong> Minimum size of a batch of data per query. This can be larger to prevent infinite loops if the CursorColumn is not unique.</td><td>Example: <strong>100</strong></td></tr><tr><td>FilterCondition</td><td>All filtering options used in any 'WHERE' condition(s) of the query</td><td>Example: <strong>"Name IS NOT NULL"</strong></td></tr><tr><td>Columns</td><td><strong>Mandatory.</strong> A list of columns that we want to show in a result.</td><td>Example:<strong>"Id", "Name"</strong></td></tr><tr><td>ReturnDataConfiguration</td><td><p></p><p>The parameters here are used in more complex queries.<br><br><em>In our example, we have 2 related tables, but want to show the contents of one of them based on the 'CursorColumn' from a second table. Since Timestamp values are not unique, we need to find all combinations of Id, Timestamp that match the filter condition in a subquery, and then join this result with the outer-query to get the final result.</em><br><br>Note that i<em>n "ReturnDataConfiguration", our parameters area of concern is everything outside of first open parenthesis "(" and last closing parenthesis ")", i.e.:</em></p><pre class="language-sql"><code class="lang-sql">FROM
(
...
) AS t INNER JOIN [Table1] ts ON ts.[Id] = t.[Id]
WHERE ts.[Id] > 0
ORDER BY Id
</code></pre></td><td><p></p><p>Example complex query:</p><pre class="language-sql"><code class="lang-sql">SELECT ts.[Id],ts.[Name] FROM
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
</code></pre></td></tr><tr><td>CursorAlias</td><td><strong>Mandatory.</strong> This is the alias for a subquery result table. It is used in 'JoinClause', and can be used in 'Columns' if we want to return values from a subquery table.</td><td>Example: <strong>"t"</strong></td></tr><tr><td>JoinClause</td><td><strong>Mandatory.</strong> Our result table to which we join the subquery result, plus the condition of the join.</td><td>Example: <strong>[Table1] ts ON ts.[Id] = t.[Id]</strong></td></tr><tr><td>FilterCondition</td><td>All filtering options used in any 'WHERE' conditions.</td><td>Example: "<strong>ts.[Id] > 0"</strong></td></tr><tr><td>OrderByClause</td><td><strong>Mandatory.</strong> This is the column(s) that we want to order our final result by.</td><td>Example: "<strong>Id"</strong></td></tr><tr><td>Columns</td><td><strong>Mandatory</strong>. A list of columns that we want to show in the final result.</td><td>Example: "<strong>ts.[Id]" "ts.[name]"</strong></td></tr><tr><td>Delay</td><td><strong>Mandatory.</strong> This represents the delay, in second, between data sync cycles once it no longer finds any new data.</td><td>Example: <strong>10</strong></td></tr><tr><td>messageKeyExpresssion</td><td>Optional, but recommended to mitigate data loss. <a href="./#appendix-a"><strong>See Appendix A</strong></a> for more information on this parameter.</td><td>id</td></tr></tbody></table>

**Example Topic JSON**

```json
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

#### Connection Attributes

The below table can be used to help create your Connection Attributes JSON needed to set up a real-time sync.

| Parameter        | Description                                                               | Example |
| ---------------- | ------------------------------------------------------------------------- | ------- |
| databaseType     | **Mandatory.** TSQL, Oracle, or DB2                                       | TSQL    |
| connectionString | **Mandatory.** This should be the connection string for your data source. |         |

```json
{
  "databaseType": "TSQL",
  "connectionString": "Server=;Database=;User ID=cinchy;password=example;Trusted_Connection=False;Connection Timeout=30;Min Pool Size=10;",
}
```
{% endtab %}

{% tab title="Schema" %}
**The** [**Schema** ](../../building-data-syncs/columns-and-mappings/#2.-schema-columns)**section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

| Parameter   | Description                                                                                                   | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------- | ------- |
| Name        | **Mandatory.** The name of your column as it appears in the source.                                           | Name    |
| Alias       | **Optional.** You may choose to use an alias on your column so that it has a different name in the data sync. |         |
| Data Type   | **Mandatory.** The data type of the column values.                                                            | Text    |
| Description | **Optional.** You may choose to add a description to your column.                                             |         |



There are other options available for the Schema section if you click on **Show Advanced.**

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Example |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Mandatory       | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Mandatory is checked</strong> on a column, then all rows are synced with the execution log status of failed, and the source error of <strong>"Mandatory Rule Violation"</strong></li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul> |         |
| Validate Data   | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul>                                                                                                                                                                                                                   |         |
| Trim Whitespace | **Optional if data type = text.**  If your data type was chosen as "text", you can choose whether to **trim the whitespac**e _(that is, spaces and other non-printing characters)._                                                                                                                                                                                                                                                                                                   |         |
| Max Length      | **Optional if data type = text.** You can input a numerical value in this field that represents the maximum length of the data that can be synced in your column. If the value is exceeded, the row will be rejected (you can find this error in the Execution Log).                                                                                                                                                                                                                  |         |

You can choose to add in a **Transformation > String Replacement** by inputting the following:

| Parameter   | Description                                                                                                                           | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Pattern     | **Mandatory if using a Transformation.** The pattern for your string replacement, i.e. the string that will be searched and replaced. |         |
| Replacement | What you want to replace your pattern with.                                                                                           |         |

{% hint style="info" %}
Note that you can have more than one String Replacement
{% endhint %}
{% endtab %}

{% tab title="Filter" %}
You have the option to add a source filter to your data sync. Please review the documentation here for more information on [source filters.](../../building-data-syncs/advanced-settings/filters.md)
{% endtab %}
{% endtabs %}

<div data-full-width="true">

<figure><img src="../../../.gitbook/assets/image (674).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

</div>

## 4. Next Steps

* Configure your [Destination](../../supported-data-sync-destinations/)
* Define your[ ](../../building-data-syncs/sync-actions.md)[Sync Actions.](../../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* If more than one listener is needed for a real-time sync, configure it/them via [the Listener Config table.](../../supported-real-time-sync-stream-sources/the-listener-configuration-table.md)
* To run a real-time sync, enable your Listener from [the Execution tab.](../../building-data-syncs/)

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
