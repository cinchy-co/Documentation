# REST API

## 1. Overview

A REST API is an application programming interface that conforms to the constraints of REST (representational state transfer) architectural style and allows for interaction with RESTful web services.

REST APIs work by fielding **requests for a resource** and **returning all relevant information** about the resource, translated into a format that clients can easily interpret (this format is determined by the API receiving requests). Clients can also **modify** items on the server and even **add new items** to the server through a REST API.

{% hint style="success" %}
The REST API source support batch syncs.
{% endhint %}

## 2. Info Tab

You can review the parameters that can be found in the info tab below _(Image 1)._

#### Values

| Parameter  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Example       |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| Title      | **Mandatory.** Input a name for your data sync                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | REST API Sync |
| Version    | **Mandatory.** This is a pre-populated field containing a version number for your data sync. You can override it if you wish.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 1.0.0         |
| Parameters | <p><strong>Optional.</strong> Review our <a href="../building-data-syncs/advanced-settings/parameters.md">documentation on Parameters here</a> for more information about this field.<br><br>Cinchy v5.6 added support for the <a href="../../cql/the-basics-of-cql/cinchy-supported-functions/connections-functions.md"><strong>URL_ESCAPE</strong> and <strong>JSON_ESCAPE</strong> functions</a> to be used in REST API data syncs <strong>anywhere a "parameter" could be utilized (Ex: Endpoint URL, Post Sync Script, etc.)</strong>. These functions escape parameter values to be safe inside of a URL or JSON document respectively.</p> |               |

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## 3. Source Tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>REST API</td></tr><tr><td>HTTP Method</td><td><strong>Mandatory.</strong> </td><td>This will be either GET or POST.</td></tr><tr><td>API Response Format</td><td><strong>Mandatory.</strong> Use this field to specify a response format of the endpoint. Currently, the Connections UI only supports JSON responses.</td><td>JSON</td></tr><tr><td>Records Root JSONPath</td><td><p><strong>Mandatory.</strong> Specify the JSON path for the results. This path will depend on the format of your response.<br><br>If the <strong>JSON response top-element is an object</strong>, <strong>$</strong> will refer to the top-element of the response.<br></p><p>If the <strong>JSON response top-element is an array,</strong> the CLI code will wrap the array in an object. The object will contain the array under <strong>member name.</strong></p></td><td><strong>$.payload</strong></td></tr><tr><td>Path to Iterate</td><td>Specify the path to Iterate.</td><td></td></tr><tr><td>API Endpoint URL</td><td><strong>Mandatory.</strong> API endpoint, including URL parameters like API key</td><td><a href="https://www.quandl.com/api/v3/datatables/CLS/IDHP?fx_business_date=@date&#x26;amp;api_key=API_KEY">https://www.quandl.com/api/v3/datatables/CLS/IDHP?fx_business_date=@date&#x26;amp;api_key=API_KEY</a></td></tr><tr><td>Next Page URL JSONPath</td><td>Specify the path for the next page URL. This is only relevant for APIs that use cursor pagination</td><td></td></tr></tbody></table>
{% endtab %}

{% tab title="Schema" %}
**The** [**Schema**](../building-data-syncs/columns-and-mappings/#2.-schema-columns) **section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

| Parameter   | Description                                                                                                   | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------- | ------- |
| Name        | **Mandatory.** The name of your column as it appears in the source.                                           | Name    |
| Alias       | **Optional.** You may choose to use an alias on your column so that it has a different name in the data sync. |         |
| Data Type   | **Mandatory.** The data type of the column values.                                                            | Text    |
| Description | **Optional.** You may choose to add a description to your column.                                             |         |



There are other options available for the Schema section if you click on **Show Advanced.**

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Example |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Mandatory       | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Mandatory is checked</strong> on a column, then all rows are synced with the execution log status of failed, and the source error of <strong>"Mandatory Rule Violation"</strong></li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul> |         |
| Validate Data   | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul>                                                                                                                                                                                                                   |         |
| Trim Whitespace | **Optional if data type = text.**  If your data type was chosen as "text", you can choose whether to **trim the whitespac**e _(that is, spaces and other non-printing characters)._                                                                                                                                                                                                                                                                                                   |         |
| Max Length      | **Optional if data type = text.** You can input a numerical value in this field that represents the maximum length of the data that can be synced in your column. If the value is exceeded, the row will be rejected (you can find this error in the Execution Log).                                                                                                                                                                                                                  |         |

You can choose to add in a **Transformation > String Replacement** by inputting the following:

| Parameter   | Description                                                                                                                           | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Pattern     | **Mandatory if using a Transformation.** The pattern for your string replacement, i.e. the string that will be searched and replaced. |         |
| Replacement | What you want to replace your pattern with.                                                                                           |         |

{% hint style="info" %}
Note that you can have more than one String Replacement
{% endhint %}
{% endtab %}

{% tab title="Other Sections" %}
There are a few more options available to you under the "Add a Section" drop down.

{% hint style="danger" %}
**Note that adding a Pagination Block is mandatory.**
{% endhint %}

You can learn more about these sections in [Appendix A - Other Sections.](rest-api.md#appendix-a-other-sections)
{% endtab %}
{% endtabs %}

<figure><img src="../../.gitbook/assets/image (172).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

## 4. Next Steps

* Configure your [Destination](../supported-data-sync-destinations/)
* Define your[ Sync Behaviour](../building-data-syncs/sync-behaviour.md).
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
* To run a real-time sync (using a Cinchy Event Triggered REST API), [set up your Listener Config](../supported-real-time-sync-stream-sources/) and enable it to begin your sync.
* To run a batch sync, select **Jobs > Start Job**

## Appendix A - Other Sections

### Auth Request

You can add in an Auth Request by [reviewing the documentation here.](../building-data-syncs/advanced-settings/auth-requests.md)

### Request Headers

You can add in Request Headers by [reviewing the documentation here.](../building-data-syncs/advanced-settings/request-headers.md)

### Body

You are able to use this section to add body content.

<figure><img src="../../.gitbook/assets/image (410).png" alt=""><figcaption></figcaption></figure>

### Pagination

**A pagination block is mandatory.** [Review the documentation here](rest-api.md#pagination) for more on pagination blocks.

### **Retry Configuration**

Cinchy v5.5 introduced the Retry Configuration for REST API targets. This will automatically retry HTTP Requests on failure based on a defined set of conditions. This capability provides a mechanism to recover from transient errors such as network disruptions or temporary service outages.

{% hint style="warning" %}
**Note: the maximum number of retries is capped at 10.**
{% endhint %}

To set up a retry specification:

1. Under the REST API source tab, select **API Specification > Retry Configuration**

<figure><img src="../../.gitbook/assets/image (409).png" alt=""><figcaption></figcaption></figure>

2\. Select your Delay Strategy.&#x20;

* **Linear Backoff:** Defines a delay of approximately n seconds where n = current retry attempt.
* **Exponential Backoff:** A strategy where every new retry attempt is delayed exponentially by 2^n seconds, where n = current retry attempt.&#x20;
  * _Example: you defined Max Attempts = 3. Your first retry is going to be in 2^1 = 2, second: 2^2 = 4, third: 2^3 = 8 sec._

3\. Input your Max Attempts. The maximum number of retries allowed is 10.

<figure><img src="../../.gitbook/assets/image (439).png" alt=""><figcaption></figcaption></figure>

4\. Define your Retry Conditions. You must define the conditions under which a retry should be attempted. For the Retry to trigger, **at least one** of the "Retry Conditions" has to evaluate to true.&#x20;

{% hint style="info" %}
Retry conditions are only evaluated if the response code is not 2xx Success.
{% endhint %}

Each Retry Condition contains **one or more "Attribute Match" sections**. This defines a Regex to evaluate against a section of the HTTP response. The following are the three areas of the HTTP response that can be inspected:

* Response Code
* Header
* Body

If there are multiple "Attribute Match" blocks within a Retry Condition, **all have to match for the retry condition to evaluate to true.**

{% hint style="warning" %}
Note that the Regex value should be entered as a regular expression. The Regex engine is .NET and expressions can be tested by using [this online tool](http://regexstorm.net/tester). In the below example, the Regex is designed to match any HTTP 5xx Server Error Codes, using a Regex value of "5\[0-9]\[0-9]".\
\
**For Headers,** the format of the Header string which the Regex is applied against is {Header Name}={Header Value}, e.g. "Content-Type=application/json".&#x20;
{% endhint %}

<figure><img src="../../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (646).png" alt=""><figcaption></figcaption></figure>
