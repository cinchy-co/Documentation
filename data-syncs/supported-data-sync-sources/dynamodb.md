# DynamoDB

## Overview

[Amazon DynamoDB](https://aws.amazon.com/dynamodb/?trk=d1003b1b-ffc2-4fbd-9ce6-e70c668663bc&sc_channel=ps&s_kwcid=AL!4422!3!536393505298!e!!g!!dynamodb&ef_id=Cj0KCQjwteOaBhDuARIsADBqRehoQ4LyBjuhkAYGKfx15DT4NXjMrNVjbVFUYbYb_5uQOrcctpV9A-8aAihsEALw_wcB:G:s&s_kwcid=AL!4422!3!536393505298!e!!g!!dynamodb) is a managed NoSQL database service that's offered by Amazon as part of the AWS portfolio.

## Example use case

You currently use DynamoDB to store metrics on product use and growth, but being stuck in the DynamoDB silo means that you can't easily use this data across a range of business use cases or teams. You can use a batch sync to liberate your data into Cinchy.

{% hint style="success" %}
The DynamoDB source supports batch syncs.
{% endhint %}

## Info tab

You can find the parameters in the **Info** tab below _(Image 1)_.

#### Values

| Parameter   | Description                                                                                                                                                                                      | Example         |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------- |
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                   | Product Metrics |
| Variables   | **Optional.** Review our documentation on [Variables here ](../building-data-syncs/advanced-settings/variables.md)for more information about this field.                                         |                 |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. **Inputting at least an Admin Group is mandatory.** |                 |

<figure><img src="../../.gitbook/assets/image (384).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## Source tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

| Parameter                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Example   |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| Source                     | Mandatory. Select your source from the drop down menu.                                                                                                                                                                                                                                                                                                                                                                                                                                             | DynamoDB  |
| Entity                     | Mandatory. The name of the entity you want to sync as it appears in DynamoDB.                                                                                                                                                                                                                                                                                                                                                                                                                      | Metrics   |
| AWS Access Key (Client ID) | Mandatory. The encrypted AWS Access Key (Client ID) used to access your DynamoDB.                                                                                                                                                                                                                                                                                                                                                                                                                  |
| AWS Secret (Client Secret) | Mandatory. The encrypted AWS Secret (Client Secret) used to access your DynamoDB.                                                                                                                                                                                                                                                                                                                                                                                                                  |
| AWS Region                 | Mandatory. The name of the region for your AWS instance.                                                                                                                                                                                                                                                                                                                                                                                                                                           | US-East-1 |
| Username                   | Mandatory. The name of a user with access to connect to your DynamoDB server.                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Password                   | Mandatory. The password associated with the above user.                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| AuthType                   | This field defines the authentication type for your data sync. Cinchy supports "Access Key" and "IAM" role. When selecting **Access Key**, you must provide the key and key secret. When selecting **IAM role**, a new field will appear for you to paste in the role's Amazon Resource Name (ARN). You also must ensure that:The role must be configured to have at least read access to the source. The Connections pods' role must have permission to assume the role specified in the data sync config |

{% endtab %}

{% tab title="Schema" %}
**The** [**Schema**](../building-data-syncs/columns-and-mappings/#2.-schema-columns) **section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

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

You can choose to add in a **Transformation > String Replacement** by inputting the following:

| Parameter   | Description                                                                       | Example |
| ----------- | --------------------------------------------------------------------------------- | ------- |
| Pattern     | **Mandatory if using a Transformation.** The pattern for your string replacement. |         |
| Replacement | What you want to replace your pattern with.                                       |         |

{% hint style="info" %}
Note that you can have more than one String Replacement
{% endhint %}
{% endtab %}

{% tab title="Filter" %}
You have the option to add a source filter to your data sync. Please review the documentation here for more information on [source filters.](../building-data-syncs/advanced-settings/filters.md)
{% endtab %}
{% endtabs %}

<figure><img src="../../.gitbook/assets/image (360).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

## Next steps

- Configure your [Destination](../supported-data-sync-destinations/)
- Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
- Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
- Click **Jobs > Start a Job** to begin your sync.
