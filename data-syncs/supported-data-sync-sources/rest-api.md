# REST API

## Overview

A REST API is an application programming interface that conforms to the constraints of REST (representational state transfer) architectural style and allows for interaction with RESTful web services.

REST APIs work by fielding **requests for a resource** and **returning all relevant information** about the resource, translated into a format that clients can easily interpret (this format is determined by the API receiving requests). Clients can also **modify** items on the server and even **add new items** to the server through a REST API.

{% hint style="success" %}
The REST API source support batch syncs.
{% endhint %}

## Info tab

You can find the parameters in the **Info** tab below _(Image 1)_.

#### Values

| Parameter   | Description                                                                                                                                                                                      | Example       |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- |
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                   | REST API Sync |
| Variables   | **Optional.** Review our documentation on [Variables here ](../building-data-syncs/advanced-settings/variables.md)for more information about this field.                                         |               |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. **Inputting at least an Admin Group is mandatory.** |               |

<figure><img src="../../.gitbook/assets/image (93).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## Source tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

|       Parameter        |                                                                                               Description                                                                                                |                                              Example                                              |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| Source                 | Mandatory. Select your source from the drop down menu.                                                                                                                                                   | REST API                                                                                          |
| HTTP Method            | Mandatory.                                                                                                                                                                                               | This will be either GET or POST.                                                                  |
| API Response Format    | Mandatory. Use this field to specify a response format of the endpoint. Currently, the Connections UI only supports JSON responses.                                                                      | JSON                                                                                              |
| Records Root JSONPath  | Mandatory. Specify the JSON path for the results. The default root of a JSON object is `$`. If the JSON response top-element is an array, use `$.data` as the root of the response, which will contain the array. See [Best practices](/data-syncs/supported-data-sync-sources/rest-api.md#best-practices) for more info.| `$.data`, `$`                                                                                     |
| Path to Iterate        | The path of the parameter to iterate. This is used to target subarrays within objects.                                                                                                                                       |                                                                                                   |
| API Endpoint URL       | Mandatory. API endpoint, including URL parameters like API key                                                                                                                                           | https://www.quandl.com/api/v3/datatables/CLS/IDHP?fx_business_date=@date&#x26;amp;api_key=API_KEY |
| Next Page URL JSONPath | Specify the path for the next page URL. This is only relevant for APIs that use cursor pagination                                                                                                        |                                                                                                   |
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

Here's the edited table, streamlined for clarity and easy translation:

|    Parameter    |                                                                        Description                                                                        | Example |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Mandatory       | - If both 'Mandatory' and 'Validated': empty rows rejected. <br> - If only 'Mandatory': rows synced but marked as failed with 'Mandatory Rule Violation'. |         |
| Validate Data   | - If both 'Mandatory' and 'Validated': empty rows rejected. <br> - If only 'Validated': all rows synced.                                                  |         |
| Trim Whitespace | Optional for text data. Choose to trim whitespace.                                                                                                        |         |
| Max Length      | Optional for text data. Set max length; exceeding values get rejected.                                                                                    |         |

I've simplified the descriptions and used a more structured list format to enhance readability.                                                                                                                                                                                                            |         |

You can choose to add in a **Transformation > String Replacement** by inputting the following:

| Parameter   | Description                                                                       | Example |
| ----------- | --------------------------------------------------------------------------------- | ------- |
| Pattern     | **Mandatory if using a Transformation.** The pattern for your string replacement. |         |
| Replacement | What you want to replace your pattern with.                                       |         |

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

<figure><img src="../../.gitbook/assets/image (298).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

## Best practices

### JSON array returns

If the API returns a top-level JSON array, use `$.data` in the **Records Root JSONPath**.  

For example, here is a sample JSON return object:
```json
[
  {
  "name": "John Doe",
  "age": 30,
  "hobbies": ["reading", "swimming", "coding"]
}
]
```
If you use `$` as the **Records Root JSONPath** and the top level is an array, then you must append `$.data` to each schema. For example, for the JSON array above, you can do either of the following:

#### Example 1

**Records Root JSONPath**: `$.data`
**Schema**: 
- `$.name` for "name"
- `$.age` for "age"

#### Example 2

**Records Root JSONPath**: `$`
**Schema**: 
- `$.data.name` for "name"
- `$.data.age` for "age"

## Next steps

* Configure your [Destination](../supported-data-sync-destinations/)
* Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* To run a batch sync, select **Jobs > Start Job**

## Appendix A - Other Sections

### Auth Request

You can add in an Auth Request by [reviewing the documentation here.](../building-data-syncs/advanced-settings/auth-requests.md)

### Request Headers

You can add in Request Headers by [reviewing the documentation here.](../building-data-syncs/advanced-settings/request-headers.md)

### Body

You are able to use this section to add body content.

<figure><img src="../../.gitbook/assets/image (396).png" alt=""><figcaption></figcaption></figure>

### Pagination

**A pagination block is mandatory.** [Review the documentation here](rest-api.md#pagination) for more on pagination blocks.

### **Retry Configuration**

Cinchy v5.5 introduced the Retry Configuration for REST API targets. This will automatically retry HTTP Requests on failure based on a defined set of conditions. This capability provides a mechanism to recover from transient errors such as network disruptions or temporary service outages.

{% hint style="warning" %}
**Note: the maximum number of retries is capped at 10.**
{% endhint %}

To set up a retry specification:

1. Under the REST API source tab, select **API Specification > Retry Configuration**

<figure><img src="../../.gitbook/assets/image (395).png" alt=""><figcaption></figcaption></figure>

2\. Select your Delay Strategy.

* **Linear Backoff:** Defines a delay of approximately n seconds where n = current retry attempt.
* **Exponential Backoff:** A strategy where every new retry attempt is delayed exponentially by 2^n seconds, where n = current retry attempt.
  * _Example: you defined Max Attempts = 3. Your first retry is going to be in 2^1 = 2, second: 2^2 = 4, third: 2^3 = 8 sec._

3\. Input your Max Attempts. The maximum number of retries allowed is 10.

<figure><img src="../../.gitbook/assets/image (431).png" alt=""><figcaption></figcaption></figure>

4\. Define your Retry Conditions. You must define the conditions under which a retry should be attempted. For the Retry to trigger, **at least one** of the "Retry Conditions" has to evaluate to true.

{% hint style="info" %}
Retry conditions are only evaluated if the response code isn't 2xx Success.
{% endhint %}

Each Retry Condition contains **one or more "Attribute Match" sections**. This defines a Regex to evaluate against a section of the HTTP response. The following are the three areas of the HTTP response that can be inspected:

* Response Code
* Header
* Body

If there are multiple "Attribute Match" blocks within a Retry Condition, **all have to match for the retry condition to evaluate to true.**

{% hint style="warning" %}
Note that the Regex value should be entered as a regular expression. The Regex engine is .NET and expressions can be tested by using [this online tool](http://regexstorm.net/tester). In the below example, the Regex is designed to match any HTTP 5xx Server Error Codes, using a Regex value of `5[0-9][0-9]`.\
\
**For Headers**, the format of the Header string which the Regex is applied against is {Header Name}={Header Value}. For example `"Content-Type=application/json"`.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (750).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (261).png" alt=""><figcaption></figcaption></figure>

;;
