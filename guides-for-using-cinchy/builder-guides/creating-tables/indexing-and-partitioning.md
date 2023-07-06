---
description: This page outlines the indexing and partitioning options on your tables.
---

# Indexing and Partitioning

## Table of Contents

| Table of Contents                                                                      |
| -------------------------------------------------------------------------------------- |
| [#1.-indexing](indexing-and-partitioning.md#1.-indexing "mention")                     |
| [#2.-full-text-indexing](indexing-and-partitioning.md#2.-full-text-indexing "mention") |
| [#3.-columnar-indexing](indexing-and-partitioning.md#3.-columnar-indexing "mention")   |
| [#4.-partitioning](indexing-and-partitioning.md#4.-partitioning "mention")             |

## 1. Indexing

Indexing is used to improve query performance on frequently searched columns within large data sets. Without an index, Cinchy begins a data search with the first row of a table and then follows through the entire table sequentially to find all relevant rows. The larger the table(s), the slower the search.

If the table you are searching for has an index for its column(s), however, Cinchy is able to search much quicker.

In the below example, we will set up a query for a **Full Name** field. When you create an index for that field, an indexed version of your table is created that is sorted sequentially/alphabetically.

When you run your query on this index, that table will be searched using a binary search.

A binary search will not start from the top record. It will check the _middle_ record with your search criteria for a match. If a match it not found, it will check whether the found value is larger or smaller than the desired value. If smaller, it reruns the data check with the top half of the data, finding the median record. If larger, it reruns the data check with the bottom half of the data, finding the median record. It will repeat until your data is found.

### **1.1 Setting up an Index**

In this example, we have a table with employee names _(Image 1)_. We want to search for "John Smith", using the **Full Name** column.

![Image 1: An example table](<../../../.gitbook/assets/image (628).png>)

1. To set up your index, select **Design Table** from the left navigation tab.

2\. Click **Indexes** _(Image 2)_.

![Image 2: Select Indexes from the list](<../../../.gitbook/assets/image (7) (1).png>)

3\. Select **"Click Here to Add"** and fill out the following information for a new index. Click save when done _(Image 3)_:

* **Index Name.**
* Select the column(s) to add to your index. For our example we have selected the **Full Name** column to be indexed.
  * You can select more than one column per index.
* Select the **Included column(s)** to add to your index, if applicable.
  * The difference between regular columns and Included columns is that indexes with included columns provide the greatest benefit when covering your query because you can include all columns your query may reference, such as columns with data types, numbers, or sizes not allowed as index key columns.
  * For more on Included Columns, [click here](https://www.sqlshack.com/sql-server-non-clustered-indexes-with-included-columns/)

![Image 3: An example index](<../../../.gitbook/assets/image (326).png>)

4\. We can now query our full name column for John Smith and receive our results quicker than if we hadn't set up our index _(Image 4)_.

![Image 4: An example query on an indexed column](<../../../.gitbook/assets/image (181).png>)

{% hint style="info" %}
Note that there is no UI change in the query builder or your results when running a query on an indexed column. The difference will be in the speed of your returned results.
{% endhint %}

## 2. Full Text Indexing

A full-text index is a special type of index that provides index access for full-text queries against character or binary column data. A full-text index breaks the column into tokens and these tokens make up the index data.

### 2.1 Setting up a Full Text Index

1. Click on **Design Table > Full-text Index**
2. Add in the desired column(s) and click **save** when done _(Image 5)._

![Image 5: Full text indexing](<../../../.gitbook/assets/image (369).png>)

## 3. Columnar Indexing

Columnar Indexing (also known as [Columnstore indexing](https://docs.microsoft.com/en-us/sql/relational-databases/indexes/columnstore-indexes-overview?view=sql-server-ver16)) is available when running SQL Server 2016+. It is not currently available on a PostgreSQL deployment of the Cinchy platform.

[Columnar indexes](https://www.c-sharpcorner.com/article/understanding-columnstore-indexes-in-sql-server-part-one/) are used for storing and querying large tables. This index uses column-based data storage and query processing to improve query performance. Instead of rowstore or b-tree indexes where the data is logically and physically organized and stored as a table with rows and column, the data in a columnstore indexes is _physically_ stored in columns and _logically_ organized in rows and columns.

You may want to use a columnar index when:

* Your table has over 1 million records.
* Your data is rarely modified. Having large numbers of deletes can cause fragmentation, which adversely affect compression rates. Updates are also processed as deletes followed by inserts, which will adversely affect the performance of your loading process.

### 3.1 Setting up Columnar Indexing

1. Click on **Design Table > Columnar Index**
2. Add in the desired column(s) and click **save** when done _(Image 6)._

![Image 6: Columnar Indexing](<../../../.gitbook/assets/image (575).png>)

{% hint style="warning" %}
When using a Columnar Index, you won't be able to add any new columns to your table. You will need to delete the index, add your column(s), and then re-add the index.
{% endhint %}

## 4. Partitioning

Partitioning data in a table is essentially organizing and dividing it into units that can then be spread across more than one file in a database. The benefits of this are:

* Improved efficiency of accessing and transferring data while maintaining its integrity.
* Maintenance operations can be performed on one or more partitions more efficiently.
* Query performance is improved based on the types of queries most frequently run.

When creating a partition in Cinchy, you use the values of a specified column to map the rows of a table into partitions.

### 4.1 Setting up a Partition

In this example we want to set up a partition that divides our employees based on a **Years Active** column _(Image 7)_. We want to divide the data into two groups: those who have been active for two years or more, and those who have only been active for one year.

1. Click on **Design Table > Partition**

![Image 7: Partitioning](<../../../.gitbook/assets/image (146).png>)

1. Fill in the following information and click save when done _(Image 8)_:

* **Partitioning Column:** this is the column value that will be used to map your rows. In this example we are using the **Years Active** column.
* **Type:** Select either Range Left (which means that your boundary will be **<=**) or Range Right (where you boundary is only **<**). In this example we want our boundary to be **Range Left.**
* **Add Boundary:** Add in your boundary value(s). Click the **+** key to add it to your boundary list. In this example we want to set our boundary to 2.

![Image 8: Creating your Partition](<../../../.gitbook/assets/image (596).png>)

Once set up, this partition will organize our data into two groups, based on our boundary of those who have a **Years Active value** of two or above.

2\. You can now run a query on your partitioned table _(Image 9)._

![Image 9: An example query on a partitioned table](<../../../.gitbook/assets/image (735).png>)

{% hint style="info" %}
Note that there is no UI change in the query builder or your results when running a query on a partitioned table. The difference will be in the speed of your returned results.
{% endhint %}

**For more formation on creating, modifying or managing Partitioning, please visit Microsoft's** [**Partitioned table and Indexes**](https://docs.microsoft.com/en-us/sql/relational-databases/partitions/partitioned-tables-and-indexes?view=sql-server-ver15) **documentation.**
