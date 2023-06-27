# Filters

## Overview

Filters can be used in your source and target configurations to define specific subsets of data that you want to use in your syncs.

## 1. Source Filters

When syncing a Data Source, you may have the option to add in additional configuration sections, such as a Filter, under the "Add a Section" drop down tab in the Connection Experience _(Image 1)._

{% hint style="warning" %}
Note that if your source only has one of the listed options, it will appear by default instead of in a drop-down.
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (727).png" alt=""><figcaption><p><em>Image 1: Adding a Filter</em></p></figcaption></figure>

A filter on your source is optional. It relies on a source specific syntax for filtering out records from your source target. The filter can reference execution parameters.

| Source     | Definition                                                                                                                  |
| ---------- | --------------------------------------------------------------------------------------------------------------------------- |
| File       | For a file data sources (delimited, fixed width & Excel) the syntax conforms to the .NET frameworks RowFilter on a DataView |
| Salesforce | For Salesforce syntax is the SOQL where clause (without the where expression)                                               |
| Dynamics   | For Dynamics syntax is the OData $filter clause (without the $filter=expression)                                            |
| Cinchy     | For Cinchy syntax is the CQL where clause (without the where expression)                                                    |
| SqlServer  | For SqlServer the syntax is the T-SQL where clause (without the where expression)                                           |

This is only available if using a table, not a query. For queries, include the filter in the query itself.&#x20;

{% hint style="warning" %}
There can only be one \<Filter>  for each source. To specify more than one condition, use AND/OR to allow logical combination of multiple expressions.
{% endhint %}

## **Conditional Filters**

**For REST API, SOAP 1.2, Kafka Topic, Platform Event, and Parquet sources**, there is a **"Conditional" option for source filters** in the Connections UI.&#x20;

Once selected you will be able to define the conditions upon which data is pulled into your source via the filter. After data is pulled from the source, new conditional UI filters down the set of returned records to ones that match the defined conditions.

* Multiple Conditions can be added to a single data sync by using the **AND/OR and +Rule buttons.**
* You are able to group your Rules into a Ruleset by using the **+Ruleset button.**
* **The left-most drop down** is used to select either a source or a target column as defined in your Source and Destination tabs
* **The centre drop-down** is used to select from the following options:
  * \=
  * !=
  * Contains
  * Is Null
  * Is Not Null
* **The right-most drop-down** can either be used for a plain value (ex: text, numerical, etc.) This will adjust based on the column data type picked in the left-most drop down. For example, if in the source schema the column is a date, then it renders a date picker.

For example, the below condition would only bring in records where the **source column "Employee Status" is not null** _(Image 2)._

<figure><img src="../../../.gitbook/assets/image (657).png" alt=""><figcaption><p>Image 2: Conditional Example</p></figcaption></figure>

## 2. Source Filter Examples:

**Example 1:** Using a filter to sync only source records with \[net worth] > 10000 _(Image 3)._

<figure><img src="../../../.gitbook/assets/image (445).png" alt=""><figcaption><p>Image 3: Source Filter Example 1</p></figcaption></figure>

**Example 2:** Using a filter to sync only source records with a status like "Complete" _(Image 4)._

<figure><img src="../../../.gitbook/assets/image (446).png" alt=""><figcaption><p>Image 4: Source Filter Example 2</p></figcaption></figure>

## Target Filters

A target destination filter is optional. It relies on a source specific syntax for filtering out records from your target. The filter can reference execution parameters.

| Source     | Definition                                                                        |
| ---------- | --------------------------------------------------------------------------------- |
| Salesforce | For Salesforce the syntax is the SOQL where clause (without the where expression) |
| Dynamics   | For Dynamics syntax is the OData $filter clause (without the $filter=expression)  |
| Cinchy     | For Cinchy syntax is the CQL where clause (without the where expression)          |
| SqlServer  | For SqlServer the syntax is the TSQL where clause (without the where expression)  |

{% hint style="warning" %}
There can only be one \<Filter>  for each target. To specify more than one condition, use AND/OR to allow logical combination of multiple expressions.
{% endhint %}

## Filter Examples

**Example 1:** Filtering only target records where the Assignee is Null _(Image 5)._

<figure><img src="../../../.gitbook/assets/image (432).png" alt=""><figcaption><p>Image 5: Target Filter Example 1</p></figcaption></figure>

**Example 2:** Filtering only target records where the Override ID is not Null _(Image 6)._

<figure><img src="../../../.gitbook/assets/image (581).png" alt=""><figcaption><p>Image 6: Target Filter Example 2</p></figcaption></figure>

**Example 3:** Filtering only target records from a specific source _(Image 7)._

<figure><img src="../../../.gitbook/assets/image (579).png" alt=""><figcaption><p>Image 7: Target Filter Example 3</p></figcaption></figure>
