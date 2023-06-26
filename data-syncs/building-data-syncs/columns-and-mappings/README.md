# Columns and Mappings

## 1. Overview

This page provides information on both Schema Columns (used when configuring Data Sync Sources) and Column Mappings (used when configuring Data sync Destinations).

## 2. Schema Columns

Schema columns refer to your mapping on your data source. For example, if your source is a CSV with the columns 'Name', 'Age', and 'Company', you would set up three schema columns in the Connections UI or data sync XML to match. These schema columns are then mapped to your destination columns for your data sync target, so that the data knows where to go.

{% hint style="warning" %}
Note that you do not have to, and sometimes won't be able to, set up an exact 1:1 relationship between source columns/data and schema columns.
{% endhint %}

The only difference between setting up your schema columns in the Connections UI vs in a data sync XML is the addition of the Alias column, which is only in the Experience. This attribute allows the user to give an alternative name to the column mapping (usually used for easier readability). The various types of columns are detailed below.

{% hint style="info" %}
Note that some source types have unique parameters not otherwise specified in other sources. You can find information on those, where applicable, in the source's main page.
{% endhint %}

{% hint style="success" %}
You can review the various attribute descriptions [here.](./#id-less-than-column-greater-than-attributes)
{% endhint %}

## 2. Standard Column

Fill in the following attributes for a Standard Column _(Image 1)_:

* **Name:** The name of your column
* **Formula:** The formula associated with your calculated column
* **Data Type:** The return data type of your column, this can be either:
  * **Text**
  * **Date**
  * **Number**
  * **Bool**

{% hint style="warning" %}
If a source column (of any type) is being synced into a Cinchy Target Table[ link column](broken-reference), the source column needs to be of **dataType="Text",** no matter what type it actually is.
{% endhint %}

* **Description:** Describe your column
* **Advanced Settings:**
  * You can select if you want this column to be **mandatory**
  * You can choose whether your data must be **validated**

{% hint style="info" %}
* **If both Mandatory and Validated** **are checked** on a column, then rows where the column is empty are rejected
* **If just Mandatory is checked** on a column, then all rows are synced with the execution log status of failed, and the source error of **"Mandatory Rule Violation"**
* **If just Validated is checked** on a column, then all rows are synced.
{% endhint %}

* If your data type was chosen as "text", you can choose whether to **trim the whitespac**e or not

You can choose to add in a **Transformation > String Replacement** by inputting the following:

* **Pattern** for your string replacement
* **Replacement**

{% hint style="info" %}
Note that you can have more than one String Replacement
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (510).png" alt=""><figcaption><p>Image 1: Standard Column</p></figcaption></figure>

## 3. Standard Calculated Column

Fill in the following attributes for a Standard Calculated Column _(Image 2)_:

* **Name:** The name of your column
* **Formula:** The formula associated with your calculated column
* **Data Type:** The return data type of your column, this can be either:
  * **Text**
  * **Date**
  * **Number**
  * **Bool**

{% hint style="warning" %}
If a Destination column is being **used as a sync key**, its source column has to be set to **type=Text, regardless of its actual type.**
{% endhint %}

* **Description:** Describe your calculated column
* **Advanced Settings:**
  * You can select if you want this column to be **mandatory.**
  * You can choose whether your data must be **validated.**

{% hint style="info" %}
* **If both Mandatory and Validated** **are checked** on a column, then rows where the column is empty are rejected
* **If just Mandatory is checked** on a column, then all rows are synced with the execution log status of failed, and the source error of **"Mandatory Rule Violation"**
* **If just Validated is checked** on a column, then all rows are synced.
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (730).png" alt=""><figcaption><p>Image 2: Standard Calculated Columns</p></figcaption></figure>

## 4. Conditional Calculated Column

Fill in the following attributes for a Conditional Calculated Column _(Image 3)_:

<figure><img src="../../../.gitbook/assets/image (652).png" alt=""><figcaption><p>Image 3: Conditional Calculated Column</p></figcaption></figure>

* **Name:** The name of your column
* **Data Type:** The return data type of your column, this can be either:
  * **Text**
  * **Date**
  * **Number**
  * **Bool**

{% hint style="warning" %}
If a Destination column is being **used as a sync key**, its source column has to be set to **type=Text, regardless of its actual type.**
{% endhint %}

* **Description:** Describe your calculated column
* **Advanced Settings:**
  * You can select if you want this column to be **mandatory.**
  * You can choose whether your data must be **validated.**

{% hint style="info" %}
* **If both Mandatory and Validated** **are checked** on a column, then rows where the column is empty are rejected
* **If just Mandatory is checked** on a column, then all rows are synced with the execution log status of failed, and the source error of **"Mandatory Rule Violation"**
* **If just Validated is checked** on a column, then all rows are synced.
{% endhint %}

* **Condition:**
  * **Name:**&#x20;
  * **IF:** Click Edit to create the "if" for your Conditional Statement _(Image 4)_

<figure><img src="../../../.gitbook/assets/image (495).png" alt=""><figcaption><p>Image 4: Creating your Conditional statement</p></figcaption></figure>

* **Then:** Click _Edit_ to create the "then" for your Conditional Statement _(Image 5)_

<figure><img src="../../../.gitbook/assets/image (747).png" alt=""><figcaption><p>Image 5: Creating your Conditional Statement</p></figcaption></figure>

* **Default:** Click _Edit_ to create your default expression _(Image 6)_

<figure><img src="../../../.gitbook/assets/image (710).png" alt=""><figcaption><p>Image 6: Creating your Default Expression</p></figcaption></figure>

## 5. JavaScript Calculated Column

Fill in the following attributes for a JavaScript Calculated Column _(Image 7)_:

* **Name:** The name of your column
* **Data Type:** The return data type of your column, this can be either:
  * **Text**
  * **Date**
  * **Number**
  * **Bool**

{% hint style="warning" %}
If a Destination column is being **used as a sync key**, its source column has to be set to **type=Text, regardless of its actual type.**
{% endhint %}

* **Description:** Describe your calculated column
* **Advanced Settings:**
  * You can select if you want this column to be **mandatory.**
  * You can choose whether your data must be **validated.**

{% hint style="info" %}
* **If both Mandatory and Validated** **are checked** on a column, then rows where the column is empty are rejected
* **If just Mandatory is checked** on a column, then all rows are synced with the execution log status of failed, and the source error of **"Mandatory Rule Violation"**
* **If just Validated is checked** on a column, then all rows are synced.
{% endhint %}

* **Script:** Enter in your JavaScript

<figure><img src="../../../.gitbook/assets/image (494).png" alt=""><figcaption><p>Image 7: JavaScript Calculated Column</p></figcaption></figure>

## 6. Columns in XML

This XML element defines each column in the data set as well as their data type:

```markup
<Column
    name="string"
    dataType="Text"| "Date"| "Number"| "Bool"| "Geometry"| "Geography"
    ordinal="int"                     -- Depends on the data source
    maxLength="int"                   --OPTIONAL
    isMandatory=["true", "false"]     --OPTIONAL
    validateData=["true", "false"]    --OPTIONAL
    trimWhitespace=["true", "false"]  --OPTIONAL
    description="string"              --OPTIONAL
    inputFormat="string"              --OPTIONAL
    >
    ...
</Column>
```

### 6.1 Attributes Descriptions <a href="#id-less-than-column-greater-than-attributes" id="id-less-than-column-greater-than-attributes"></a>

**`name`**

The user defined name for each column. This is used in [\<ColumnMapping>](https://cinchy.atlassian.net/wiki/spaces/KB/pages/196575278) when you want to indicate the name of the _sourceColumn_.

**`dataType`**

The data type of each column could be Text, Date, Number, Bool, Geometry, or Geography.

{% hint style="warning" %}
If a Destination column is being **used as a sync key**, its source column has to be set to **type=Text, regardless of its actual type.**
{% endhint %}

To sync into a Cinchy table with a Geometry or Geography column, those respective data types must be used in the data sync, and the input should be in well-known text (WKT) format.

{% hint style="info" %}
The dataType affects how the source and target data is parsed, and also determines how the fields are compared for equality. If your sync keeps updating a field that has not changed, double check your data types.
{% endhint %}

For example, given line 1 of a csv file:

_`Name, Location, Age`_

The ordinal for Age would be 3.

**`maxLength`**

The max length of data in the column.

**`isMandatory`**

Boolean value determining if the field is a mandatory column to create a row entry.

A defined synckey column of any data type can be checked for NULL values using isMandatory="true”. When validation fails, an error message is displayed in the command line. For other columns when validation fails, the Execution Errors Table is updated with Error Type, Mandatory Rule violation for that column and row that failed.

**`validateData`**

Boolean value determining whether or not to validate the data before inserting. Valid data means to fit all the constraints of the column (dataType, maxLength, isMandatory, inputFormat). If the data is not valid and validateData is true, then the entry will not be synced into the table. The Execution Errors Table is also updated with the appropriate Error Type (Invalid Format Exception, Max Length Violation, Mandatory Rule Violation, Input Format Exception)

**`trimWhitespace`**

Boolean value determining whether or not to trim white space.

**`description`**

Description of the column.

**`inputFormat`**

Date fields support the inputFormat which adheres to the c# .net DateTime.ParseExact format. See [here](https://docs.microsoft.com/en-us/dotnet/standard/base-types/custom-date-and-time-format-strings) for reference.&#x20;

{% hint style="info" %}
`inputFormat` attribute is useful when source file need some format changes in the input data
{% endhint %}

### Elements: <a href="#id-less-than-column-greater-than-elements" id="id-less-than-column-greater-than-elements"></a>

[ **\<Transformations>**](https://cinchy.atlassian.net/wiki/spaces/KB/pages/186613813)

## **3. Column Mappings**

Column mappings defines how a single column from the data source maps to a column in a target table. Each `<ColumnMapping>`  has both a source and a target. If the destination is a Cinchy table and the target column is a link, then a third attribute becomes available called "linkColumn" which is used to specify the column that should be used to resolve the linked record from the source value. The value of sourceColumn should match name attribute of Source . The value of targetColumn should match that of the target table.

Below is an example of a Column Mapping in the experience followed by the equivalent XML. In the experience, the Source Column attribute is a dropdown of columns configured in the Source Section.

<figure><img src="../../../.gitbook/assets/image (738).png" alt=""><figcaption></figcaption></figure>

```markup
<ColumnMapping
    sourceColumn="string"
    targetColumn="string"
    linkColumn="string"
    >
</ColumnMapping>
```

### Attributes: <a href="#id-less-than-columnmapping-greater-than-attributes" id="id-less-than-columnmapping-greater-than-attributes"></a>

**`sourceColumn`**

The name of the column in the data source. The name corresponds to the user defined name from the [\<Column>](https://cli.docs.cinchy.co/\~/edit/drafts/-LOcwWriIK-GECPm03rw/syncdata-command/tags-and-attributes/column) elements in the schema.

**`targetColumn`**

The name of the column in the target table. This would be a table that's already created in Cinchy and defined in the Target.

**`linkColumn`**

The name of a column from the linked table. If the target column is a linked column from another table, you may input data based on any of the linked table's columns.

{% hint style="warning" %}
If a Destination column is being **used as a sync key**, its source column has to be set to **type=Text, regardless of its actual type.**
{% endhint %}
