---
description: This page guides you through the various column types available on Cinchy,
---

# Columns

## Table of Contents

|                                                                      |
| -------------------------------------------------------------------- |
| [#1.-system-columns](columns.md#1.-system-columns "mention")         |
| [#2.-common-fields](columns.md#2.-common-fields "mention")           |
| [#3.-regular-columns](columns.md#3.-regular-columns "mention")       |
| [#4.-geospatial-columns](columns.md#4.-geospatial-columns "mention") |
| [#5.-link-columns](columns.md#5.-link-columns "mention")             |
| [#6.-hierarchy-columns](columns.md#6.-hierarchy-columns "mention")   |

## 1. System Columns

Cinchy contains system columns used to perform various functionality. These columns cannot be modified directly by a user.

{% hint style="warning" %}
You cannot create a new column with the same name as a system column.
{% endhint %}

### 1.1 Cinchy Id

Cinchy Id is a unique identifier assigned automatically to all records within a normal table. The Cinchy Id is associated with the record permanently and is never reassigned even if the record is deleted.

### 1.2 Version & Draft Version

Version and Draft Version are used to track changes and versions.

#### Without Change Approvals Enabled

Any changes made to a record increments the `Version`. `Draft Version` is always 0.

#### With Change Approvals Enabled

Any data approval increments `Version` and resets `Draft Version` to 0. Any proposed changes increments the `Draft Version`.

### 1.3 Approval State

This is a legacy column. It is always blank.

### 1.4 Created By & Created

`Created By` is a linked column to the `[Cinchy].[Users]` table, of the user who created the record.

`Created` is the time when the record was created, per the logged-in user's timezone.

#### Without Change Approvals Enabled

`Created By` and `Created` will be the same for all records with the same Cinchy Id.

#### With Change Approvals Enabled

`Created By` and `Created` is based on the first user to make changes on an approved record. So the user where `Draft Version` = 1.

### 1.5 Modified By & Modified

`Modified By` is a linked column to the `[Cinchy].[Users]` table, of the user who last modified the record.

#### Without Change Approvals Enabled

The last user to modify the record, and when it happened, per the logged-in user's timezone.

#### With Change Approvals Enabled

The last user to either modify the record (`Draft Version` != 0) or approve the record (`Draft Version` = 0). The timestamp for when that version was generated.

### 1.6 Deleted By & Deleted

If a record is deleted, it will show up in the Recycle Bin.

#### Without Change Approvals Enabled

A deleted record will have `Deleted By` and `Deleted` filled in, with the timezone set to the logged-in user's.

#### With Change Approvals Enabled

`Deleted By` and `Deleted` are based on the user/time when the Delete Request was created, per the logged-in user's timezone., not when it was approved.

### 1.7 Replaced

* There is always only one latest/up to date record at a time. Anytime changes are made to a record, a new version (normal or draft) is created, and the previous version is updated with a `Replaced` timestamp.
* Any record where `Replaced` is empty is the current version of that record.

## 2. Common Fields

### 2.1 Name

Each column must have a unique name. They must also not conflict with system columns (even if you are not using Change Approvals on the table).

### 2.2 Data Security Classification

{% embed url="https://cinchy.tv/all-content/1497" %}

Each column has a data security classification. This defaults to blank, and can be set to one of 4 pre-configured settings (Public, Internal, Restricted, Confidential) or additional options can be created in the `[Cinchy].[Data Security Classifications]` table by an administrator.

{% hint style="info" %}
Currently there is no functionality tied directly to Data Security Classification - the tagging is just for internal auditing purposes. Future security settings will be tied to Data Security Classifications, rather than simply done at a column level.
{% endhint %}

* **Public**: This type of data is freely accessible to all employees and company personnel. It can be freely used, reused, and redistributed without repercussions. An example might be job descriptions, press releases or links to articles.
* **Internal:** This type of data is strictly accessible to internal company personnel or employees who are granted access. This might include internal-only memos, business performance, customer surveys or website analytics.
* **Confidential:** Often, access to confidential data requires additional authorization and explanation of why access to the data is needed. Examples of confidential data include social security numbers, credit card details, phone numbers or medical records. Depending on the industry, confidential data is protected by laws like GDPR, HIPAA, CASL and others.
* **Restricted:** Restricted data is what we can call "high stakes". If compromised or accessed without authorization, it could lead to criminal charges, massive legal fines, or cause irreparable damage to the company. This is the most sensitive data, so you would have to treat it extra carefully. Examples include intellectual property, proprietary information or data protected by state and federal regulations.

### 2.3 Description

Each column can optionally have a description. The description is displayed when you hover on the column header in Data Management.

### 2.4 GUID

A GUID is a globally unique identifier, formatted as a 128-bit text string, that represents a unique identification. Every column in Cinchy is automatically assigned one. For more information on GUIDs, you can [review the documentation here.](table-and-column-guids.md)

{% hint style="danger" %}
Be very careful when editing GUIDs, as you can have unintended consequences.
{% endhint %}

### 2.5 Common Parameters

#### Add to Default View

Checked by default. After saving your changes this will add the column to be displayed in the default table (All Data by default). Generally it makes sense to be checked since there should be a view where all columns are displayed.

{% hint style="info" %}
If you need to hide a column from certain users or groups you can do so in table controls. It is usually best to have a view where all table columns are displayed.
{% endhint %}

#### Mandatory

Makes the column a mandatory field. You will not be able to save or alter a record in a state where a mandatory field is blank.

#### Unique

Requires all values in the column to be unique. Adding a new record or modifying a previous record into a state where it is a duplicate of another record will cause an error and cannot be saved.

{% hint style="info" %}
If you need uniqueness across multiple columns instead (ex. First Name does not need to be unique, but First Name + Last Name needs to be unique), you can create an index in Design Table, add those columns and set the index to unique. If it needs to be more complicated, you can also create a calculated column and set that column to unique.
{% endhint %}

![](<../../../.gitbook/assets/image (603).png>)

#### Multi-Select

Some fields can also be set to multi-select.

For example, the column `Players` in `[Football].[Teams]` can be a multi-select field since each team will have multiple players.

#### Allow Linking

Checked by default. This allows other tables to use the column as a link/relationship.

See [Linking Data](broken-reference) to get more context on how they are used.

{% hint style="info" %}
You want to pick identifying columns for linking, such as IDs or Name. Generally you want to use unique columns, but in some cases it is a better user experience to pick an almost unique field for readability.

I.e. Full name may not be unique, but it is much easier to understand than Employee ID.
{% endhint %}

#### Allow Display in Linked Views

Checked by default. Some columns may not make sense for linking but can be useful to display when someone is choosing an option.

See [Linking Data ](linking-data.md)to get more context and tips.

#### Encrypt

If [Data At Rest Encryption](../../additional-guides/enable-data-at-rest-encryption.md) is enabled, you will see the option of Encrypt for columns. If this is checked, the column will be encrypted within the database. This is useful for hiding sensitive information so that people with access to the database directly don't see these fields.

There is no difference in user experience within the Cinchy platform. The data is displayed in plain text on the UI or via the query APIs.



## 3. Regular Columns

### 3.1 Text

Text columns have a maximum length, set to 500 by default.

_These are equivalent to `VARCHAR(n)` data type in SQL._

### 3.2 Number

You can choose from 3 display formats for number - regular, currency, and percentage. You can also decide how many decimal places to display (0 by default). Note that these are both display settings, and will not affect how the number is stored.

_These are equivalent to `FLOAT(53)` data type in SQL (default, 8-byte field)._

### 3.3 Date

There are several Date column type display format options available in Cinchy:

* MMM DD, YYYY (e.g. Oct 31, 2016)
* YY-MM-DD (e.g. 16-10-31)
* DD-MM-YYYY (e.g. 31-10-2016)
* DD-MMM-YY (e.g. 31-Oct-16)
* Custom Format

![](<../../../.gitbook/assets/image (81).png>)

{% hint style="info" %}
Please Note: the "Default Value" field is not mandatory and should be left blank (best practice). However, if populated you will not be able to clear the default date value provided to a "blank" data (no date). You will only be able to overwrite it with another date value.
{% endhint %}

_These are equivalent to `DATE()` data type in SQL._

### 3.4 Yes/No

You must select a default value of yes (1) or no (0) for yes/no fields.

_These are equivalent to `bit`data type in SQL._

### 3.5 Calculated

A calculated column is evaluated using other fields on the record. It will also have a result type - which is the form in which the calculated results will be stored.

For example, you can have a column `[Full Name]` that is `CONCAT([First Name], ' ', [Last Name])`_._

_These are equivalent to computed columns in SQL._

#### 3.5.1 Cached vs Live Calculated Columns

When creating a calculated column, you will notice the option to have it cached or not, located under Advanced Settings. This was an option introduced in version 4.0 of the platform.

![](<../../../.gitbook/assets/image (304).png>)

A **cached calculated column** stores your data for fast retrieval and querying. Calculated columns are defaulted to cached. This is an actual column in your table that is based on a defined CQL formula. This column will recalculate when data is changed **in the same row.**

In the below example, the **Label** column is a calculated column that connects various name columns together. You can see that _"Connect your data"_ appears in each label. If you then wanted to change the name in row one from _"Connect your data"_ to _"Connect **all** your data"_, only that specific row would recalculate automatically to update the label. To update any other row (within this table or another) with a calculated column that references that data, you would need to manually make changes to each to prompt a recalculation.

![](<../../../.gitbook/assets/image (205).png>)

The other option is to uncheck the cached button, and to have a **live calculated column** instead. A live column runs the query in real time, on the fly: it is essentially a stored formula that is only executed when you read or query the record, and is executed each time you do so.

In the above example, if the **Label** column was a live calculation, the formula would run every time you refresh the screen or requery the data. If changes have been made (in this example, changing _"Connect your data"_ to _"Connect **all** your data"_), the calculated column will automatically reflect them, regardless of if it references another row or even another table.

While live calculated columns can be very powerful this way, they also have their drawbacks, including possibly throwing errors and being very resource intensive. In some cases, live calculated columns can even break a table (for example if you have a live calculated column linking to **another** live calculated column, you may run into errors).

### 3.6 Choice

You can create a choice column (single or multi-select) in Cinchy. In this scenario, you specify all your choices (1 per newline) in the table design. A user will only be able to select from the options provided.

## 4. Geospatial Columns

If you created a spatial table, you will have access to the geography and geometry column types. These columns also have the option to be indexed via Index in the advanced settings on the column itself.

![Check off Index to create an index on a geospatial column](<../../../.gitbook/assets/image (591).png>)

### 4.1 Geometry

In the UI, this takes a well-known text (WKT) representation of a geometry object. You can modify or paste the WKT representation directly in the editor on the UI. Geometric functions can be performed on this column through CQL and calculated columns.

### 4.2 Geography

In the UI, this takes a well-known text (WKT) representation of a geography object. You can modify or paste the WKT representation directly in the editor on the UI. Geographic functions can be performed on this column through CQL and calculated columns.

## 5. Link Columns

Link columns allow you to establish inherent relationships with other records in other tables. See [Linking Data](linking-data.md) for more details.

## 6. Hierarchy Columns

Hierarchy columns are simply link columns referencing the current table. For example, the below table contains a list of documentation pages, some of which also have sub-level pages (or even sub-sub-level pages). Using a **Hierarchy Column** allows you to quickly view the relationships between data.

**Example 1:** _API Overview_ is the **parent page**. It had four **sub-pages**: _API Authentication, API Saved Queries, ExecuteCQL, and Webhook Ingestion._ Clicking on any of the links within the Sub-Pages column would return you to the row for that specific data set.

**Example 2:** _Builder Guides_ is the **parent page.** It has five **sub-pages**: _Best Practices, Creating Tables, Saved Queries, Integration Guides, and CInchy DXD Utility._ In this example, we also have another level of hierarchy, wherein _Best Practices_ is also a **parent page,** and _Multilingual Support_ is its **sub-page.**

![](<../../../.gitbook/assets/image (214).png>)

Another common use of Hierarchy columns are to show Manager/Employee relationships
