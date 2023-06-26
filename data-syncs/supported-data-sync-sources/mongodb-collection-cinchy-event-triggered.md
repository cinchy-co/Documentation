# MongoDB Collection (Cinchy Event Triggered)

## 1. Overview

Data changes in Cinchy (CDC) can be used to trigger a data sync from a MongoDB data source to a specified target. The attributes of the CDC Event are available to use as parameters within the  Data Source Definition to narrow the scope of the request, e.g. a lookup.

{% hint style="success" %}
The MongoDB Collection (Cinchy Event Triggered) Source supports real-time syncs.
{% endhint %}

## 2. Defining the Connection

{% hint style="success" %}
The options available to the MongoDB Collection (Cinchy Event Triggered) connector are identical to the MongoDB source connector which can be found [here.](mongodb-collection/)
{% endhint %}

The following sections in the Source configuration of the Connections experience can reference attributes of the CDC Event as parameters:

* **Connection String**
* **Database Name**
* **Collection Name**
* **Query**
* **Projection**
* **Pipeline**

{% hint style="success" %}
In Cinchy v5.6+, you can also reference attributes of the CDC Event in Calculated Columns.

**Note that syncs making use of this must limit their batch size to 1.**
{% endhint %}

Parameters use the column name or alias as defined in the CDC Event Listener Config prefixed with an "@", e.g. @CompanyName would be the parameter name for the following Cinchy CDC listener Topic configuration.

<pre class="language-json"><code class="lang-json"><strong>{
</strong>	"tableGuid": "420c1851-31ed-4ada-a71b-31659bca6f92",
	"fields": [
		{
			"column": "Cinchy Id",
                        "alias":"CinchyId"
                },
		{
			"column": "Company Name",
                        "alias":"CompanyName"
		}
	]
}
</code></pre>

{% hint style="danger" %}
Parameter names are case sensitive when used in the Connection configuration. Parameter matching is performed using literal string replacements. Names should not contain spaces (spaces are automatically removed), and should have differing prefixes.&#x20;
{% endhint %}

The following set of parameters will be available on every event even if they're not present in the listener config

* **@Version**
* **@DraftVersion**
* **@CinchyRecordType**
* **@ApprovalState**
* **@ModifiedBy**
* **@Modified**
* **@Deleted**

## 3. Listener Config

In order to configure a MongoDB Collection (Cinchy Event Triggered) connection, a listener must be configured with an Event Connector Type of Cinchy CDC.

[Set up your listener configuration](broken-reference) for your data sync, keeping the following constraints in mind:

* **Column names** in the listener config should not contain spaces. If they do, they will be automatically removed. _E.g. a column named Company Name will become the replacement  parameter @CompanyName_
* The replacement parameter names are **case sensitive.**
* **Column names** in the listener config should not be prefixes of other column names. _E.g. if you have a column called "Name", you shouldn't have another called "Name2" as the value of @Name2 may end up being replaced by the value of @Name suffixed with a "2"._

#### Example Listener Configuration

```json
{
	"tableGuid": "420c1851-31ed-4ada-a71b-31659bca6f92",
	"fields": [
		{
			"column": "Cinchy Id",
                        "alias":"CinchyId"
                },
		{
			"column": "Company Name",
                        "alias":"CompanyName"
		}
	]
}
```
