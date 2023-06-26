---
description: 'This page outlines Step 4 of Deploying CinchyDXD: Updating the Data Experience'
---

# Updating the Data Experience (CinchyDXD)

## Table of Contents

| Table of Contents                                                                         |
| ----------------------------------------------------------------------------------------- |
| [#introduction](updating-the-data-experience-cinchydxd.md#introduction "mention")         |
| [#1.-table-updates](updating-the-data-experience-cinchydxd.md#1.-table-updates "mention") |
| [#2.-query-updates](updating-the-data-experience-cinchydxd.md#2.-query-updates "mention") |

## Introduction

There are a few updates that are required in the Data Experience that has been created in your source environment. You do not want to have to repeat the updates in both the source and target environments. The upcoming section will show how to update the data experience in the source environment so that you can then re-package and reinstall in the target environment.

## 1. Table Updates

1. Log back into your source environment using the following:\
   **URL: \<cinchy source url>** \
   **User ID: \<source user id>**\
   **Password: \<source password>**
2. Make the following changes to the **Currency Exchange Rate** table:

| Column Details | Values                                                                                                                                |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Column 1       | <p>Current Column Name Value = Currency 1</p><p>New Column NameValue = From Currency<br></p><p>All other settings remain the same</p> |
| Column 2       | <p>Current Column Name Value = Currency 2</p><p>New Column NameValue = To Currency<br></p><p>All other settings remain the same</p>   |

3\. Save your changes before leaving the table.

## 2. Query Updates

1. Update the **Currency Converter** query to reflect column name changes that were made in the Table Updates section above _(Image 1)._

![Image 1: Step 1](<../../../.gitbook/assets/image (336).png>)

{% hint style="success" %}
Be sure to update the @Currency\_1 and @Currency\_2 labels to better reflect the input fields
{% endhint %}

2\. Test the query to validate that it is still functioning _(Image 2 and 3)._

![Image 2: Step 2](<../../../.gitbook/assets/image (418).png>)

![Image 3: Step 2](<../../../.gitbook/assets/image (302).png>)

3\. Save your query.
