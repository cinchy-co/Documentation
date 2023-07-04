---
description: This page will describe how to attach files to your Cinchy table rows.
---

# Attaching Files

## Table of Contents

| Table of Contents                                                                          |
| ------------------------------------------------------------------------------------------ |
| [#1.-overview](attaching-files.md#1.-overview "mention")                                   |
| [#2.-how-to-attach-a-file](attaching-files.md#2.-how-to-attach-a-file "mention")           |
| [#3.-deleting-an-attached-file](attaching-files.md#3.-deleting-an-attached-file "mention") |

## Overview

You are able to attach files and images to any row in a Cinchy table by creating a linked column that is linked to the **'Files'** system table of your platform.

If you have access to view the 'Files' table, you can also view every attachment on your system.

{% hint style="info" %}
Cinchy supports the attaching of any file type.
{% endhint %}

## 2. How to Attach a File

1. Navigate to the table where you want to attach your file.
2. &#x20;Click **Design Table > Columns**
3. Add a new column with the following parameters _(Image 1)_:
   1. **Column Name:** We recommend you name this something easy to recognize, like _"Images and Files"_
   2. **Data Type:** Link
   3. **Linked Table:** You will link this to the _"Cinchy\Files"_ table
   4. **Linked Column:** File Name
   5. **Advanced Settings:** Make sure to select the "Multi-select" checkbox if you want the capability to add more than one file to a row.

![Image 1: Step 3, creating your Files column](<../../../.gitbook/assets/image (557).png>)

4\. Click **Save**

5\. Navigate back to your table and locate your newly created **files column.**

{% hint style="warning" %}
**Note** that you must first create the row prior to uploading a file.
{% endhint %}

6\. To attach a file, click on the upload button (located in the top right hand corner of any cell in the column) _(Image 2)_.

![Image 2: Step 6, uploading a file](<../../../.gitbook/assets/image (385).png>)

7\. From the pop-up window, select **Choose Files** to pick your file from your machine, then click **Submit** _(Image 3)._

![Image 3: Step 7, choosing your file to upload](<../../../.gitbook/assets/image (162).png>)

8\. Once uploaded, your cell should look like this _(Image 4)_:

![Image 4: Step 8, a completed upload](<../../../.gitbook/assets/image (51).png>)

## 3. Deleting an Attached File

To fully delete your attachment from the platform, you will need to navigate to the **Files system table** and delete the row associated with your attachment.

{% hint style="warning" %}
You will need edit access to the Files table in order to do so.
{% endhint %}

Your file will appear crossed out in your original table and cannot be opened or downloaded _(Image 5)._

![Image 5: Deleting an attached file](<../../../.gitbook/assets/image (486).png>)
