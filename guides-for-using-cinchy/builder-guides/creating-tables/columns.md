---
description: This page guides you through the various column types available on Cinchy,
---

# Columns

## System Columns

Cinchy has system columns used to perform various functionality. These columns can't be modified directly by a user.

{% hint style="warning" %}
You can't create a new column with the same name as a system column.
{% endhint %}

### Cinchy Id

Cinchy Id is a unique identifier assigned automatically to all records within a normal table. The Cinchy Id is associated with the record permanently and is never reassigned even if the record is deleted.

### Version & Draft Version

Version and Draft Version are used to track changes and versions.

#### Without Change Approvals Enabled

Any changes made to a record increments the `Version`. `Draft Version` is always 0.

#### With Change Approvals Enabled

Any data approval increments `Version` and resets `Draft Version` to 0. Any proposed changes increments the `Draft Version`.

### Approval State

This is a legacy column. It's always blank.

### Created By & Created

`Created By` is a linked column to the `[Cinchy].[Users]` table, of the user who created the record.

`Created` is the time when the record was created, per the logged-in user's timezone.

#### Without Change Approvals Enabled

`Created By` and `Created` will be the same for all records with the same Cinchy Id.

#### With Change Approvals Enabled

`Created By` and `Created` is based on the first user to make changes on an approved record.

### Modified By and Modified

`Modified By` is a linked column to the `[Cinchy].[Users]` table, of the user who last modified the record.

#### Without Change Approvals Enabled

The last user to modify the record, and when it happened, per the logged-in user's timezone.

#### With Change Approvals Enabled

The last user to either modify the record (`Draft Version` != 0) or approve the record (`Draft Version` = 0). The timestamp for when that version was generated.

### Deleted By and Deleted

If a record is deleted, it will show up in the Recycle Bin.

#### Without Change Approvals Enabled

A deleted record will have `Deleted By` and `Deleted` filled in, with the timezone set to the logged-in user's.

#### With Change Approvals Enabled

`Deleted By` and `Deleted` are based on the user/time when the Delete Request was created, per the logged-in user's timezone., not when it was approved.

### Replaced

- Cinchy always has one latest/up to date record at a time. Anytime changes are made to a record, a new version (normal or draft) is created, and the previous version is updated with a `Replaced` timestamp.
- Any record where `Replaced` is empty is the current version of that record.

## Common Fields

### Name

Each column must have a unique name. They must also not conflict with system columns (even if you aren't using Change Approvals on the table).

### Data security classification

<!-- vale off -->

{% embed url="https://cinchy.tv/all-content/1497" %}

<!-- vale on -->

Each column has a data security classification. This defaults to blank, and can be set to one of 4 pre-configured settings (Public, Internal, Restricted, Confidential) or additional options can be created in the `[Cinchy].[Data Security Classifications]` table by an administrator.

{% hint style="info" %}
Currently there is no functionality tied directly to Data Security Classification - the tagging is just for internal auditing purposes. Future security settings will be tied to Data Security Classifications, rather than simply done at a column level.
{% endhint %}

- **Public**: This type of data is accessible to all employees and company personnel. It can be used, reused, and redistributed without repercussions. An example might be job descriptions, press releases or links to articles.
- **Internal:** This type of data is strictly accessible to internal company personnel or employees who are granted access. This might include internal-only memos, business performance, customer surveys or website analytics.
- **Confidential:** Often, access to confidential data requires additional authorization and explanation of why access to the data is needed. Examples of confidential data include social security numbers, credit card details, phone numbers or medical records. Depending on the industry, confidential data is protected by laws like GDPR, HIPAA, CASL and others.
- **Restricted:** Restricted data is the most sensitive data, so you would have to treat it extra carefully. If compromised or accessed without authorization, it could lead to criminal charges, massive legal fines, or cause irreparable damage to the company. Examples include intellectual property, proprietary information or data protected by state and federal regulations.

### Description

Each column can optionally have a description. The description is displayed when you hover on the column header in Data Management.

### GUID

A GUID is a globally unique identifier, formatted as a 128-bit text string, that represents a unique identification. Every column in Cinchy is automatically assigned one. For more information,see the [Table and column GUID page](table-and-column-guids.md)

{% hint style="danger" %}
Be careful when editing a GUID, as you can have unintended consequences.
{% endhint %}


### Common Parameters

#### Add to Default View

Checked by default. After saving your changes this will add the column to be displayed in the default table (All Data by default). Generally it makes sense to be checked since there should be a view where all columns are displayed.

{% hint style="info" %}
If you need to hide a column from certain users or groups you can do so in table controls. It's usually best to have a view where all table columns are displayed.
{% endhint %}

#### Mandatory

Makes the column a mandatory field. You won't be able to save or alter a record in a state where a mandatory field is blank.

#### Unique

Requires all values in the column to be unique. Adding a new record or modifying a previous record into a state where it's a duplicate of another record will cause an error and can't be saved.

{% hint style="info" %}
If you need uniqueness across multiple columns instead, you can create an index in Design Table, add those columns and set the index to unique. If it needs to be more complicated, you can also create a calculated column and set that column to unique. For example, First Name doesn't need to be unique, but First Name + Last Name needs to be unique.
{% endhint %}

![](<../../../.gitbook/assets/image (603).png>)

#### Multi-Select

Some fields can also be set to multi-select.

For example, the column `Players` in `[Football].[Teams]` can be a multi-select field since each team will have multiple players.

#### Allow Linking

Checked by default. This allows other tables to use the column as a link/relationship.

See [Linking data](../creating-tables/linking-data.md) to get more context on how they're used.

{% hint style="info" %}
You want to pick identifying columns for linking, such as IDs or Name. Generally you want to use unique columns, but in some cases it's a better user experience to pick an almost unique field for readability.

For example, Full name may not be unique, but it's much easier to understand than Employee ID.
{% endhint %}

#### Allow Display in Linked Views

Checked by default. Some columns may not make sense for linking but can be useful to display when someone is choosing an option.

See [Linking Data ](linking-data.md)to get more context and tips.

#### Encrypt

If [Data At Rest Encryption](../../additional-guides/enable-data-at-rest-encryption.md) is enabled, you will see the option of Encrypt for columns. If this is checked, the column will be encrypted within the database. This is useful for hiding sensitive information so that people with access to the database directly don't see these fields.

Selecting encryption makes no difference  to the user experience within the Cinchy platform. The data is displayed in plain text on the UI or via the query APIs.

## Regular Columns

### Text

Text columns have a maximum length, set to 500 by default.

{% hint style="info" %}
These are equivalent to `VARCHAR(n)` data type in SQL.
{% endhint %}

###  Number

You can choose from 3 display formats for number - regular, currency, and percentage. You can also decide how many decimal places to display (0 by default). Note that these are both display settings, and won't affect how the number is stored.

{% hint style="info" %}
These are equivalent to `FLOAT(53)` data type in SQL (default, 8-byte field).
{% endhint %}
### Date

Cinchy has several Date column type display format options available:

- MMM DD, YYYY (Oct 31, 2016)
- YY-MM-DD (16-10-31)
- DD-MM-YYYY (31-10-2016)
- DD-MMM-YY (31-Oct-16)
- Custom Format

![](<../../../.gitbook/assets/image (81).png>)

{% hint style="info" %}
The "Default Value" field isn't mandatory and should be left blank (best practice). However, if populated you won't be able to clear the default date value provided to a "blank" data (no date). You will only be able to overwrite it with another date value.
{% endhint %}

{% hint style="info" %}
These are equivalent to `DATE()` data type in SQL.
{% endhint %}
### Yes/No

You must select a default value of yes (1) or no (0) for yes/no fields.

{% hint style="info" %}
These are equivalent to `bit`data type in SQL.
{% endhint %}

### Choice

You can create a choice column (single or multi-select) in Cinchy. In this scenario, you specify all your choices (1 per newline) in the table design. A user will only be able to select from the options provided.
## Calculated Columns 

A calculated column uses values from other fields in the record for its evaluation. These columns also have a specified result type, which dictates the format of the calculated output.

**Example**:  

A `[Full Name]` column can be calculated as `CONCAT([First Name], ' ', [Last Name])`.

{% hint style="info" %}
These columns are similar to computed columns in SQL databases.
{% endhint %}

### Live vs Cached Calculated Columns

#### Choose Your Calculation Type

When creating a calculated column, you have two types to choose from: **cached** and **live**. This feature is accessible via the Advanced Settings and was a part of the 4.0 version update.

### Cached Calculated Columns

- **What It Does**: Speeds up data retrieval.
- **How It's Stored**: As an actual column based on a CQL formula.
- **When It Updates**: Updates only if the data in the same row changes.

**Example**:  

Changing a name in a single row only triggers a recalculation for that row's "Label" column.

#### Limitations

If a cached column relies on a column from another table, changes in the other table's column won't automatically update the cached column. Make sure to account for this when using cached columns that depend on external data.
### Live Calculated Columns

- **What It Does**: A live calculated column is a non-cached calculated column that provides real-time data.
- **How It's Stored**: As a formula executed on-the-fly during read or query.
- **When It Updates**: Refreshes automatically upon every query or screen refresh.
- **When to use**: 
  - Your calculated column depends on a value from a linked table and you need the latest value from the linked table.
  - Your table doesn't contain many records.

**Example**: 
 
A live "Label" column will update instantly if any referenced data changes, affecting all rows and tables.

#### Limitations

- Live columns consume more system resources.
- Using user-defined functions in live calculated columns can cause errors if they reference other live calculated columns. Only use inbuilt functions in live columns if they reference other live columns.
## Geospatial Columns

If you created a spatial table, you will have access to the geography and geometry column types. These columns also have the option to be indexed via Index in the advanced settings on the column itself.

![Check off Index to create an index on a geospatial column](<../../../.gitbook/assets/image (591).png>)

### Geometry

In the UI, this takes a well-known text (WKT) representation of a geometry object. You can modify or paste the WKT representation directly in the editor on the UI. Geometric functions can be performed on this column through CQL and calculated columns.

### Geography

In the UI, this takes a well-known text (WKT) representation of a geography object. You can modify or paste the WKT representation directly in the editor on the UI. Geographic functions can be performed on this column through CQL and calculated columns.

## Link Columns

Link columns allow you to establish inherent relationships with other records in other tables. See [Linking Data](linking-data.md) for more details.

## Hierarchy Columns

Hierarchy columns are link columns referencing the current table. For example, the below table contains a list of documentation pages, some of which also have sub-level pages (or even sub-sub-level pages). Using a **Hierarchy Column** shows the relationships between data.

**Example 1:** _API Overview_ is the **parent page**. It had four **sub-pages**: _API Authentication, API Saved Queries, ExecuteCQL, and Webhook Ingestion._ Clicking on any of the links within the Sub-Pages column would return you to the row for that specific data set.

**Example 2:** _Builder Guides_ is the **parent page.** It has five **sub-pages**: _Best Practices, Creating Tables, Saved Queries, Integration Guides, and CInchy DXD Utility._ In this example, we also have another level of hierarchy, wherein _Best Practices_ is also a **parent page,** and _Multilingual Support_ is its **sub-page.**

![](<../../../.gitbook/assets/image (214).png>)

Another common use of Hierarchy columns are to show Manager/Employee relationships
