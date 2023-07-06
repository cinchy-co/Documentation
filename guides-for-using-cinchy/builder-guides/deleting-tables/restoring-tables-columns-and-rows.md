---
description: This page outlines how to restore tables, columns, and rows in Cinchy
---

# Restoring Tables, Columns, and Rows

## Overview

This page documents the method of resstoring deleted taables, columns, and rows in your Cinchy table.

{% hint style="success" %}
Remember that you can always **revert a changed or deleted record** to a previous state using the[ Collaboration Log.](https://platform.docs.cinchy.com/guides-for-using-cinchy/user-guides/data-management#6.-collaboration-log)
{% endhint %}

## 1. Restoring a Deleted Row

If you wish to restore a row that has been deleted, you can do so through the following steps:

1. In the table where you want to restore the row, navigate to the **Recycle Bin.**
2. Locate the deleted row.
3. Right click anywhere in the row > **Restore Row** _(Image 1)._
4. You should see a "**Restore Successful"** pop-up.

<figure><img src="../../../.gitbook/assets/image (165).png" alt=""><figcaption><p>Image 1: Restoring a Row</p></figcaption></figure>

## 2. Restoring a Deleted or Changed Column

If you wish to restore a column that has been deleted or changed (such as a name change), you can do so through the following steps:

{% hint style="warning" %}
Note: You need **insert access** on the **Table table** to complete these steps.
{% endhint %}

{% hint style="info" %}
This method will revert the entire table, including any changes made after the column was deleted.
{% endhint %}

1. Navigate to the **\[Cinchy].\[Tables]** table.
2. Find the row with **the table that has the column you want to restore > right click anywhere in the row > Collaboration Log > Revert to a previous version** _(Image 2)._
3. You should see a "**Revert Successful"** pop-up.

<figure><img src="../../../.gitbook/assets/image (644).png" alt=""><figcaption><p>Image 2: Restoring a Column</p></figcaption></figure>

## 3. Restoring a Deleted Table

If you wish to restore a table that has been deleted, you can do so through the following steps:

{% hint style="warning" %}
Note: You need **insert access** on the **Tables table** to complete these steps.
{% endhint %}

1. Navigate to the **\[Cinchy].\[Tables]** table.
2. Navigate to the **Recycle Bin.**
3. Find the row with **the table that you want to restore > right click > "Restore Row"** _(Image 3)_
4. You should see a "**Restore Successful"** pop-up.

<figure><img src="../../../.gitbook/assets/701FC46A-F5CB-4736-8CC1-CDB22D2E9601.png" alt=""><figcaption><p>Image 2: Restoring a Row</p></figcaption></figure>

{% hint style="info" %}
Make sure the Deleted date is the same, and you don't retrieve previously dropped columns.
{% endhint %}
