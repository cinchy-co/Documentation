# Filters

## Overview

Filters can be used in your source and target configurations to define specific subsets of data that you want to use in your syncs.

## 1. Source Filters

When syncing a Data Source, you may have the option to add in additional configuration sections, such as a Filter, under the "Add a Section" drop down tab in the Connection Experience _(Image 1)._

{% hint style="warning" %}
Note that if your source only has one of the listed options, it will appear by default instead of in a drop-down.
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (332).png" alt=""><figcaption><p><em>Image 1: Adding a Filter</em></p></figcaption></figure>

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

## 2. Source Filter Examples:

**Example 1:** Using a filter to sync only source records with \[net worth] > 10000 _(Image 2)._

<figure><img src="../../../.gitbook/assets/image (496).png" alt=""><figcaption><p>mage 2: Source Filter Example 1</p></figcaption></figure>

**Example 2:** Using a filter to sync only source records with a status like "Complete" _(Image 3)._

<figure><img src="../../../.gitbook/assets/image (227).png" alt=""><figcaption><p>Image 3: Source Filter Example 2</p></figcaption></figure>

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

**Example 1:** Filtering only target records where the Assignee is Null _(Image 4)._

<figure><img src="../../../.gitbook/assets/image (125).png" alt=""><figcaption><p>Image 4: Target Filter Example 1</p></figcaption></figure>

**Example 2:** Filtering only target records where the Override ID is not Null _(Image 5)._

<figure><img src="../../../.gitbook/assets/image (273).png" alt=""><figcaption><p>Image 5: Target Filter Example 2</p></figcaption></figure>

**Example 3:** Filtering only target records from a specific source _(Image 6)._

<figure><img src="../../../.gitbook/assets/image (271).png" alt=""><figcaption><p>Image 6: Target Filter Example 3</p></figcaption></figure>
