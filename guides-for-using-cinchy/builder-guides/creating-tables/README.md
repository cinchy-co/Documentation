---
description: This page guides you through creating table in Cinchy.
---

# Creating Tables

## Table of Contents  <a href="#standard-vs-spatial-table" id="standard-vs-spatial-table"></a>

| Table of Contents                                                        |
| ------------------------------------------------------------------------ |
| [#standard-vs-spatial-table-1](./#standard-vs-spatial-table-1 "mention") |
| [#import-a-csv](./#import-a-csv "mention")                               |
| [#default-view](./#default-view "mention")                               |
| [#bookmarks-and-marketplace](./#bookmarks-and-marketplace "mention")     |

## 1. Creating a Table from Scratch <a href="#standard-vs-spatial-table" id="standard-vs-spatial-table"></a>

{% embed url="https://cinchy.tv/all-content/1501" %}

1. Navigate to the Cinchy homepage. In the upper left-hand corner, click on **Create** to get started. _(Image 1)_

![Image 1: Step 1, Getting Started](<../../../.gitbook/assets/image (311).png>)

2\. Select either a Standard or a Spatial Table _(Image 2)_, per the descriptions below.

* **Spatial Table:** A spatial table allows you to create geography and geometry column types, as well as geospatial indexes. You will not be able to create partitions on a spatial table.
* **Standard Table:** You cannot create geography or geometry columns in a standard table.&#x20;

{% hint style="info" %}
You cannot convert from one type to another and will have to either recreate your table or link to another table with geospatial columns.
{% endhint %}

{% hint style="info" %}
Any existing tables created before installing Cinchy Platform v4.19.0 are standard tables.
{% endhint %}

![Image 2: Step 2, select either a Standard or Spatial Table](<../../../.gitbook/assets/image (241).png>)

3\. Select **From Scratch** _(Image 3)_.

![Image 3: Step 3, Select "From Scratch"](<../../../.gitbook/assets/image (618).png>)

4\. A new page will open with the **Table Info** tab (Image 4). Input the following information:

* **Table Name:** This is a mandatory field, and must be unique to the domain you select.
* **Icon:** You can pick an icon and colour to differentiate your table on thee home screen.
* **Domain:** This is a mandatory field. Select the domain that this table will reside under. If you are have administrative privileges, you can also create new domains from this screen.
* **Description:** You can give your table a description, which will be displayed on the homepage.

![Image 4: Step 4, Adding table info.](<../../../.gitbook/assets/image (187).png>)

5\. When you are finished with the **Info** page, select **Columns** from the left-hand navigation bar (Image 5). [See here ](columns.md)for additional information about column types.

![Image 5: Step 5, Selecting the Columns tab](<../../../.gitbook/assets/image (264).png>)

6\. A new page will open with the **Columns** tab _(Image 6)_. Every table in Cinchy must have at least one column. Input the following information:

* **Column Name:** Input a unique column name.
* **Data Type:** You can select from the following:
  * **Text:** All data in this column must be input as text
  * **Number:** All data in this column must be input numerically
  * **Date:** All data in this column must be in date format. The default is yyyy-mm-dd however you can change that.
  * **Yes/No:** All data in this column must be in Yes/No format
  * **Calculated:** Data is this column is calculated using a[ CQL](../../../cql/the-basics-of-cql/) expression
  * **Choice:** Data entered in this column must be selected from a set of choice answers that you provide
  * **Link:** Data in a link column is pulled from elsewhere on Cinchy

{% hint style="info" %}
[Watch the Cinchy TV episode on Data Types here.](https://cinchy.tv/all-content/1493)
{% endhint %}

* **Description:** Enter a description of your column
* **Data Security Classification:** You can select from Public, Internal, Confidential, or Restricted. Additional options can be created in the `[Cinchy].[Data Security Classifications]` table by an Administrator.
  * **Restricted:** Restricted data is what we can call "high stakes". If compromised or accessed without authorization, it could lead to criminal charges, massive legal fines, or cause irreparable damage to the company. This is the most sensitive data, so you would have to treat it extra carefully. Examples include intellectual property, proprietary information or data protected by state and federal regulations.
  * **Confidential:** Often, access to confidential data requires additional authorization and explanation of why access to the data is needed. Examples of confidential data include social security numbers, credit card details, phone numbers or medical records. Depending on the industry, confidential data is protected by laws like GDPR, HIPAA, CASL and others.
  * **Internal:** This type of data is strictly accessible to internal company personnel or employees who are granted access. This might include internal-only memos, business performance, customer surveys or website analytics.
  * **Public**: This type of data is freely accessible to all employees and company personnel. It can be freely used, reused, and redistributed without repercussions. An example might be job descriptions, press releases or links to articles.

{% hint style="info" %}
Currently there is no functionality tied directly to Data Security Classification - the tagging is just for internal auditing purposes. Future security settings will be tied to Data Security Classifications, rather than simply done at a column level.
{% endhint %}

- **Advanced Settings:** Select any checkboxes that pertain to your column. [See here](columns.md#common-parameters) for more information about these parameters.

{% hint style="info" %}
You may have further mandatory or optional data to input depending on your selected Data Type.
{% endhint %}

![Image 6: Step 6, defining your column ](<../../../.gitbook/assets/image (11).png>)

7\. Click on **Design Controls > Entitlements** in the left navigation pane to set your permissions _(Image 7)._ You may set these as granular as you choose. You may also set permissions on a [view by view basis.](./#default-view) [See here for more about data controls and entitlements.](data-controls/data-entitlements-and-access-controls.md)

![Image 7: Step 7, setting your permissions](<../../../.gitbook/assets/image (675).png>)

8\. Click **Save** to finalize your table.

9\. You may return to change the structure of the existing table (i.e. Rename columns, add new columns, change data type) by clicking on the **Design Table** button on the left-hand navigation _(Image 8)._

![Image 8: Step 9, changing an existing table structure](<../../../.gitbook/assets/image (53).png>)

## 2. Creating a Table by Importing a CSV <a href="#import-a-csv" id="import-a-csv"></a>

{% embed url="https://cinchy.tv/all-content/1505" %}

1. Navigate to the Cinchy homepage. In the upper left-hand corner, click on **Create** to get started _(Image 9)._

![Image 9: Step 1, Getting Started](<../../../.gitbook/assets/image (311).png>)

2\. Select either a Standard or a Spatial Table _(Image 10)_, per the descriptions below.

* **Spatial Table:** A spatial table allows you to create geography and geometry column types, as well as geospatial indexes. You will not be able to create partitions on a spatial table.
* **Standard Table:** You cannot create geography or geometry columns in a standard table.&#x20;

{% hint style="info" %}
You cannot convert from one type to another and will have to either recreate your table or link to another table with geospatial columns.
{% endhint %}

{% hint style="info" %}
Any existing tables created before installing Cinchy Platform v4.19.0 are standard tables.
{% endhint %}

![Image 10: Step 2, select either a Standard or Spatial Table](<../../../.gitbook/assets/image (241).png>)

3\. Select **Import a CSV** _(Image 11)_.

![Image 11: Step 3, Select "Import a CSV"](<../../../.gitbook/assets/image (618).png>)

4\. Enter the following information_:_

* Domain: Select the domain that you table will reside under. If you are have administrative privileges, you can also create new domains from this screen.
* File: In order to create the table, you must upload a .csv file.

{% hint style="warning" %}
The column names in your .csv file must not conflict with[ System Columns.](columns.md#1.-system-columns)
{% endhint %}

5\. When creating a table via Import a CSV, a few settings will be set by default:

* **Default Table Name:** The name of the file will be used as the name of the table (a number will be appended if there is a duplicate - ex. uploading Teams.csv will create a table named Teams 1, then Team 2 if uploaded again). You can always rename the table after it has been created.
* **Default Table Icon:** The icon defaults to a green paintbrush.
* **Default Column Types:** Columns by default will be created as a text field, with a maximum length of the longest value in the column. If a column has only numeric values in it, it will be created as a numeric column.

6\. To update these settings, navigate to the **Design Table** tab on the left navigation bar _(Image 12)_.

![Image 12: Step 6, Design Table tab](<../../../.gitbook/assets/image (105).png>)

## 3. Table Views <a href="#default-view" id="default-view"></a>

{% embed url="https://cinchy.tv/all-content/1549" %}

1. When you first create a table, a default view called A**ll Data** will be created for you, which you can find on the left navigation bar under **Manage Data** _(Image 13)._

![Image 13: Step 1: The All Data view](<../../../.gitbook/assets/image (223).png>)

2\. You can create additional views by clicking on **"+Create View"** (_Image 14)._

![Image 14: Step 2, Select "Create View"](<../../../.gitbook/assets/image (125).png>)

3\. You may chose to create a view **From Scratch** or by **Copying an Existing** view _(Image 15)._

![Image 15: Step 3, Creating a View](<../../../.gitbook/assets/image (586).png>)

### From Scratch: <a href="#bookmarks-and-marketplace" id="bookmarks-and-marketplace"></a>

1. Select "From Scratch".
2. The **Columns tab** will open. Create a **Name** for your View _(Image 16)._
3. If you'd like this to become the default view, toggle the default setting to **On** _(Image 16)._

![Image 16: Steps 2,3](<../../../.gitbook/assets/image (195).png>)

4\. Select the column(s) that you want to be visible in this view _(Image 17)._ You may rearrange the column order using drag and drop.

![Image 17: Step 4, Selecting Columns](<../../../.gitbook/assets/image (510).png>)

5\. Click on the **Sort tab** in the left navigation bar _(Image 18)._

![Image 18: Step 5, Sorting](<../../../.gitbook/assets/image (325).png>)

6\. Use this screen to select which columns you'd like to sort your data by, and in which order. You may rearrange the columns using drag and drop _(Image 19)._

![Image 19: Step 6, Sorting your view](<../../../.gitbook/assets/image (671).png>)

7\. Click on the **Filter tab** in the left navigation bar. Here, you may use [query language](../../../cql/the-basics-of-cql/) to focus your view _(Image 20)._

![Image 20: Step 7, Using the Filter tab](<../../../.gitbook/assets/image (166).png>)

&#x20;

8\. Click on the **Permission tab** in the left navigation bar. Here, you may set permissions for who can use this view. By default, it is set to All Users _(Image 21)._

![Image 21: Step 8, setting permissions.](<../../../.gitbook/assets/image (404).png>)

9\. Select **Save** to finalize your view.

### From Existing: <a href="#bookmarks-and-marketplace" id="bookmarks-and-marketplace"></a>

1. Select **"From Existing".**
2. Select which view you would like to copy _(Image 22)._

![Image 22: Step 2, Importing a view](<../../../.gitbook/assets/image (621).png>)

### Updating a View

1. To update any view, including the Add Data view, click on the **pencil icon** next to the view's name under Manage Data _(Image 23)._

![Image 23: Step 1, Updating a View](<../../../.gitbook/assets/image (40).png>)

## 4. Bookmarks and the Homepage <a href="#bookmarks-and-marketplace" id="bookmarks-and-marketplace"></a>

Once you create a table, it will be added to your bookmarks by default. Other users (or if you un-star the table from your bookmarks) will see it in the Homepage if they have permissions to.
