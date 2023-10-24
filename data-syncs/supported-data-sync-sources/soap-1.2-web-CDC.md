# SOAP web service (Cinchy Event Triggered)

## Overview

SOAP (Simple Object Access Protocol) is an XML-based protocol for accessing web
services over HTTP.

SOAP allows applications running on different operating systems to communicate
using different technologies and programming languages. You can use SOAP APIs to
create, retrieve, update or delete records, such as passwords, accounts, leads,
and custom objects, from a server.

{% hint style="success" %} The SOAP 1.2 Web Service source supports batch syncs.
{% endhint %}

## Info tab

You can find the parameters in the **Info** tab below _(Image 1)_.

#### Values

| Parameter   | Description                                                                                                                                                                                      | Example   |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------- |
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                   | SOAP Sync |
| Variables   | **Optional.** Review our documentation on [Variables here ](../building-data-syncs/advanced-settings/variables.md)for more information about this field.                                         |           |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. **Inputting at least an Admin Group is mandatory.** |           |

<figure><img src="../../.gitbook/assets/image (166).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## Source tab

The following table outlines the mandatory and optional parameters you will find
on the Source tab _(Image 2)._

{% tabs %} {% tab title="Source Details" %} The following parameters will help
to define your data sync source and how it functions.

| Parameter           | Description                                                                                                          | Example                                        |
| ------------------- | -------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| Source              | Mandatory. Select your source from the drop down menu.                                                               | SOAP 1.2 Web Service                           |
| authType            | Mandatory. Select the type of authentication you wish to use in this sync: None, WSSE, Basic.                        | Basic                                          |
| Use Password Digest | Use only with WSSE authType and Password Type as **PasswordDigest**. Otherwise, leave unchecked.                     |                                                |
| Request Timeout     | Mandatory. Set a timeout in milliseconds. No maximum value. Minimum greater than 0. Default is 100 milliseconds.     | 2000                                           |
| Endpoint            | Mandatory. Contains your SOAP 1.2 Web Service API endpoint.                                                          | `dataaccess`                                     |
| Has Mtom Response   | Required to be true if SOAP API response contains an attachment outside the message.                                 |                                                |
| Record Xpath        | Mandatory. The Xpath to select records to extract from the SOAP response. Starts with ‘//’ followed by the tag name. |                                                |
| Envelope Namespace  | Namespace prefix for SOAP request elements.                                                                          | "foo"                                          |
| Namespaces - Name   | Name of your SOAP namespace tags in request and response. Appears as "soap" in the snippet below.                    | "soap"                                         |
| Namespaces - Value  | URL describing this namespace in the response.                                                                       | "http://www.dataaccess.com/webservicesserver/" |

{% endtab %}

{% tab title="Schema" %} **The**
[**Schema**](../building-data-syncs/columns-and-mappings/#2.-schema-columns)
**section** is where you define which source columns you want to sync in your
connection. You can repeat the values for multiple columns.

| Parameter   | Description                                                                                                   | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------- | ------- |
| Name        | **Mandatory.** The name of your column as it appears in the source.                                           | Name    |
| Alias       | **Optional.** You may choose to use an alias on your column so that it has a different name in the data sync. |         |
| Data Type   | **Mandatory.** The data type of the column values.                                                            | Text    |
| Description | **Optional.** You may choose to add a description to your column.                                             |         |

Select **Show Advanced** for more options for the Schema section.

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Example |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Mandatory       | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Mandatory is checked</strong> on a column, then all rows are synced with the execution log status of failed, and the source error of <strong>"Mandatory Rule Violation"</strong></li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul> |         |
| Validate Data   | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul>                                                                                                                                                                                                                   |         |
| Trim Whitespace | **Optional if data type = text.** For Text data types, you can choose whether to **trim the whitespace**.\_                                                                                                                                                                                                                                                                                                                                                                           |         |
| Max Length      | **Optional if data type = text.** You can input a numerical value in this field that represents the maximum length of the data that can be synced in your column. If the value is exceeded, the row will be rejected (you can find this error in the Execution Log).                                                                                                                                                                                                                  |         |

You can choose to add in a **Transformation > String Replacement** by inputting
the following:

| Parameter   | Description                                                                       | Example |
| ----------- | --------------------------------------------------------------------------------- | ------- |
| Pattern     | **Mandatory if using a Transformation.** The pattern for your string replacement. |         |
| Replacement | What you want to replace your pattern with.                                       |         |

{% hint style="info" %} Note that you can have more than one String Replacement
{% endhint %} {% endtab %} {% endtabs %}

<figure><img src="../../.gitbook/assets/image (339).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

## Next steps

- Configure your [Destination](../supported-data-sync-destinations/)
- Define
  your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
- Add in your
  [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md),
  if required.
- Click **Jobs > Start a Job** to begin your sync.

1. Namespace Value
