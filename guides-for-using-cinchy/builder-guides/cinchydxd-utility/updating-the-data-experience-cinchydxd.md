---
description: "This page outlines Step 4 of Deploying CinchyDXD: Updating the Data Experience"
---

# Update the data experience (CinchyDXD)

## Introduction

The Data Experience has required updates you must create in your source environment. You don't want to have to repeat the updates in both the source and target environments. The upcoming section will show how to update the data experience in the source environment so that you can then re-package and reinstall in the target environment.

## Table updates

1. Log back into your source environment using the following:\
   **URL: \<Cinchy source url>** \
   **User ID: \<source user id>**\
   **Password: \<source password>**
2. Make the following changes to the **Currency Exchange Rate** table:

| Column Details | Values                                                                                                                                |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Column 1       | <p>Current Column Name Value = Currency 1</p><p>New Column NameValue = From Currency<br></p><p>All other settings remain the same</p> |
| Column 2       | <p>Current Column Name Value = Currency 2</p><p>New Column NameValue = To Currency<br></p><p>All other settings remain the same</p>   |

3\. Save your changes before leaving the table.

## Query updates

1. Update the **Currency Converter** query to reflect column name changes that were made in the Table Updates section above _(Image 1)._

![Image 1: Step 1](<../../../.gitbook/assets/image (456).png>)

{% hint style="success" %}
Be sure to update the @Currency_1 and @Currency_2 labels to better reflect the input fields
{% endhint %}

2. Test the query to validate that it's still functioning _(Image 2 and 3)._

![Image 2: Step 2](<../../../.gitbook/assets/image (705).png>)

![Image 3: Step 2](<../../../.gitbook/assets/image (241).png>)

3. Save your query.
