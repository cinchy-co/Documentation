# REST API (Cinchy Event Triggered)

## Overview

Data changes in Cinchy (CDC) can be used to trigger a data sync from a REST API data source to a specified target. The attributes of the CDC Event are available to use as parameters within the REST API Data Source Definition to narrow the scope of the request, e.g. a lookup.&#x20;

**Example Use Case:** Let's assume an organization wants to use the Dun & Bradstreet API for enriching company information, e.g. # of Employees, Address, etc. When a company record is added or modified in a table called Companies inside of Cinchy, a D\&B API should be triggered with the Company Name (a mandatory field on the Companies table) passed in as a parameter, and the Company record should be enriched with the company information from the API response.&#x20;

## 2. Defining the Connection

{% hint style="success" %}
The options available to the REST API (Cinchy Event Triggered) connector are identical to the REST API source connector which can be found [here](broken-reference).
{% endhint %}

The following sections in the Source configuration of the Connections experience can reference attributes of the CDC Event as parameters:

* **Auth Request -> Body**
* **Auth Request -> Request Headers -> Header -> Header Value**
* **Auth Request -> Endpoint URL**
* **Body**
* **Request Headers -> Header -> Header Value**
* **API Endpoint URL**

Parameters use the column name or alias as defined in the CDC Event's Listener Config prefixed with an "@", e.g. @CompanyName would be the parameter name for the following Cinchy CDC listener Topic configuration.

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

In order to configure a REST API (Cinchy Event Triggered) connection, a listener must be configured. If configuring using the Listener Config table, you would select the Event Connector Type of Cinchy CDC.

Otherwise, you can set up your listener configuration for your data sync through the Connections UI, keeping the following constraints in mind:

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
