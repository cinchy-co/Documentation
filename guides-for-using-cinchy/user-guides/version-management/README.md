---
description: >-
  This page details how Cinchy approaches Version Management within the
  platform.
---

# Version Management

## 1. Data Version Management

Cinchy natively and automatically manages data versioning in the platform through the ‘always-on’ version tracking, collaboration logging, and recycle bin features (data restore).

Cinchy maintains a version history of all changes to every data element stored in Cinchy. The version history is queryable in Cinchy to accelerate analysis, and can also be viewed through the [Collaboration Log](../data-management.md#6.-collaboration-log), which tracks changes made by users, systems, or external applications _(Image 1)._ When required, you can easily revert data to previous states using the [**Recycle Bin** ](https://platform.docs.cinchy.com/guides-for-using-cinchy/user-guides/data-management#recycle-bin)**or the Revert** button.

<figure><img src="../../../.gitbook/assets/image (128).png" alt=""><figcaption><p>Image 1: The Collaboration Log</p></figcaption></figure>

## 2. Schema Version Management

{% hint style="info" %}
This section refers to data schemas/models, **not** data values themselves.
{% endhint %}

Your schema/data model version can also be managed when you are using **multiple environments.** For example, if you have a DEV environment and make a change to a table design (ex: changing a column name), you can export and deploy your data model to a PROD environment and Cinchy will intelligently consolidate and merge the schema changes to adhere to the latest version.

To export a table (**i.e. your data model)**, navigate to the **Design Table > Export** button _(Image 2)._ You can then **import your data model** into any other environment using the [model loader ](https://platform.docs.cinchy.com/api-guide/api-overview#2.2-apps-modelloader)_(Image 3)._

<figure><img src="../../../.gitbook/assets/image (601).png" alt=""><figcaption><p>Image 2: Exporting a data model </p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (681).png" alt=""><figcaption><p>Image 3: Importing a data model</p></figcaption></figure>

This functionality is achieved through the **use and synchronization of GUIDs.** Each data element in Cinchy (table, column, etc.) will have a matching GUID, which stays consistent even across multiple environments. That means that changes made in your source environment will automatically and accurately be applied once promoted to your higher environment.&#x20;

{% hint style="info" %}
**A GUID **_**(globally unique identifier)**_** is a 128-bit text string that represents an identification (ID).**
{% endhint %}

You can find the GUID for your object by navigating to the applicable System Table. Ex: **Column GUIDs** can be found in the **Columns table** _(Image 4)._

<figure><img src="../../../.gitbook/assets/image (437).png" alt=""><figcaption><p>Image 4: GUIDs</p></figcaption></figure>

## 3. Creating Data Packages

While you are able to manually export/import data models across environments, you may want to **package up** multiple objects (tables, queries, reference data, etc.) and push that all together between environments. This method still adheres to schema version control and management.

This can be accomplished using the **Cinchy DXD Utility**, which you can learn more about by [reviewing the documentation here](../../builder-guides/cinchydxd-utility/).
