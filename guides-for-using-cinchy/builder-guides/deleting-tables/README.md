---
description: This page outlines how to delete tables in the Cinchy platform.
---

# Deleting Tables

## Overview

There are three ways to delete tables on your Cinchy platform:

1\) **Using the "Design Table" tab in the relevant table.** This option is available to any user with the "Design Table" entitlement on the table.

2\) **Using CQL.** This option is available to any user with the "Design Table" entitlement on the table.

3\) **Using the \[Tables] table.** This option is available to any user with the **"Delete Row"** entitlement on the table, generally this would be an Administrator.

To ensure that the relevant user has the correct entitlements, you can navigate to the **Data Controls > Entitlements t**ab of the relevant table. The "Design Table" column should be checked off _(Image 1)._

{% hint style="warning" %}
Deleting a table via any of the below methods results in your data becoming inaccessible, however it will technically still be available on the underlying database. To fully remove deleted data, **you must use the Data Erasure capability,** [outlined here.](../creating-tables/data-controls/data-erasure.md)
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (120).png" alt=""><figcaption><p>Image 1: Ensuring the correct entitlements</p></figcaption></figure>

## Deleting a Table Using "Design Table"

1. Navigate to the table you wish to delete as a user with **"Design Table"** access on it.
2. Select **"Design Table" > "Info" > Delete** (Image 2).

{% hint style="success" %}
Erroneously deleted tables can be restored via the **\[Tables] table**, [as outlined here.](restoring-tables-columns-and-rows.md#3.-restoring-a-deleted-table)
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (131).png" alt=""><figcaption><p>Image 2: Deleting using Design Table</p></figcaption></figure>

## Deleting a Table Using CQL

1. Navigate to the Query Builder as a user with "Design Access" on the table you wish to delete.
2. Use the [DROP TABLE](../../../cql/the-basics-of-cql/cql-statements-overview/cinchy-ddl-statements.md#drop-table) statement, shown below, to delete your table.

**Syntax**

```sql
DROP TABLE [Domain].[Table_Name]
```

**Example**

```sql
DROP TABLE [HR].[Employees]
```

{% hint style="success" %}
Erroneously deleted tables can be restored via the \[Tables] table, [as outlined here.](restoring-tables-columns-and-rows.md#3.-restoring-a-deleted-table)
{% endhint %}

## Deleting a Table Using the \[Tables] Table

1. Navigate to the **\[Tables] table** a user with **"Delete Row"** access, generally an [Administrator.](../../administrator-guide.md)
2. Find the row pertaining to the table that you want to delete.
3. **Right click** on the row > **"Delete".**

{% hint style="success" %}
Erroneously deleted tables can be restored via the \[Tables] table, [as outlined here.](restoring-tables-columns-and-rows.md#3.-restoring-a-deleted-table)
{% endhint %}
