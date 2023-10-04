# REST API

## Overview

A REST API is an application programming interface that conforms to the constraints of REST (representational state transfer) architectural style and allows for interaction with RESTful web services.

REST APIs work by fielding **requests for a resource** and **returning all relevant information** about the resource, translated into a format that clients can easily interpret (this format is determined by the API receiving requests). Clients can also **modify** items on the server and even **add new items** to the server through a REST API.

Cinchy v5.5 added support for the [**URL\_ESCAPE** and **JSON\_ESCAPE**](../../cql/the-basics-of-cql/cinchy-supported-functions/connections-functions.md) functions to be used in REST API data syncs **anywhere a "parameter" could be utilized (Ex: Endpoint URL, Post Sync Script, etc.)**. These functions escape parameter values to be safe inside of a URL or JSON document respectively.

{% hint style="info" %}
Before you set up your data sync destination, [make sure to configure your Source.](../supported-data-sync-sources/)
{% endhint %}

{% hint style="success" %}
The REST API destination supports batch and real-time syncs.
{% endhint %}

## Destination tab

The following table outlines the mandatory and optional parameters you will find on the Destination tab _(Image 1)._

{% tabs %}
{% tab title="Destination Details" %}
The following parameters will help to define your data sync destination and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Destination</td><td><strong>Mandatory.</strong> Select your destination from the drop down menu.</td><td>REST API</td></tr></tbody></table>
{% endtab %}

{% tab title="Column Mapping" %}
**The** [**Column Mapping**](../building-data-syncs/columns-and-mappings/#3.-column-mappings) **section** is where you define which source columns you want to sync to which destination columns. You can repeat the values for multiple columns.

| Parameter     | Description                                                              | Example |
| ------------- | ------------------------------------------------------------------------ | ------- |
| Source Column | **Mandatory.** The name of your column as it appears in the source.      | Name    |
| Target Column | **Mandatory.** The name of your column as it appears in the destination. | Name    |
{% endtab %}

{% tab title="API Specification" %}
You can use this section to help define your API Specifications. The options available to you are:

* Retry Configuration
* REST API Source
* Insert Specification
* Update Specification
* Delete Specification

You can learn more about these options in [Appendix A - API Specifications.](rest-api.md#appendix-a-api-specifications)
{% endtab %}

{% tab title="Post Sync Scripts" %}
Cinchy v5.5 introduced the ability to pass parameters from a REST response into post sync scripts during both real-time and batch data syncs, allowing you to do more with your REST API data.

[Read more about this feature here.](../building-data-syncs/advanced-settings/post-sync-scripts.md)
{% endtab %}

{% tab title="Filter" %}
You have the option to add a destination filter to your data sync. Please review the documentation here for more information on [destination filters.](../building-data-syncs/advanced-settings/filters.md#target-filters)
{% endtab %}
{% endtabs %}

<figure><img src="../../.gitbook/assets/image (272).png" alt=""><figcaption><p>Image 1: Define your Destination</p></figcaption></figure>

## Next steps

* Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
* If you are running a real-time sync, [set up your Listener Config](../supported-real-time-sources/) and enable it to begin your sync.
* If you are running a batch sync, click **Jobs > Start a Job** to begin your sync.

## Appendix A - API Specifications

<figure><img src="../../.gitbook/assets/image (394).png" alt=""><figcaption></figcaption></figure>

### Retry configuration

Retry Configuration automatically retries HTTP Requests on failure based on a defined set of conditions. A single retry configuration is defined for the REST API target, and applies to all requests configured in the Insert, Update, and Delete specifications. This capability provides a mechanism to recover from transient errors such as network disruptions or temporary service outages.

{% hint style="warning" %}
**Note: the maximum number of retries is capped at 10.**
{% endhint %}

To set up a retry configuration:

1. Under the REST API destination tab, select **API Specification > Retry Configuration**

2. Select your Delay Strategy.

* **Linear Backoff:** Defines a delay of approximately n seconds where n = current retry attempt.
* **Exponential Backoff:** A strategy where every new retry attempt is delayed exponentially by 2^n seconds, where n = current retry attempt.
  * _Example: you defined Max Attempts = 3. Your first retry is going to be in 2^1 = 2, second: 2^2 = 4, third: 2^3 = 8 sec._

3. Input your Max Attempts. The maximum number of retries allowed is 10.

<figure><img src="../../.gitbook/assets/image (441).png" alt=""><figcaption></figcaption></figure>

4. Define your Retry Conditions. You must define the conditions under which a retry should be attempted. For the Retry to trigger, **at least one** of the "Retry Conditions" has to evaluate to true.

{% hint style="info" %}
Retry conditions are only evaluated if the response code isn't 2xx Success.
{% endhint %}

Each Retry Condition contains **one or more "Attribute Match" sections**. This defines a Regex to evaluate against a section of the HTTP response. The following are the three areas of the HTTP response that can be inspected:

* Response Code
* Header
* Body

If there are multiple "Attribute Match" blocks within a Retry Condition, **all have to match for the retry condition to evaluate to true.**

{% hint style="warning" %}
The Regex value should be entered as a regular expression. The Regex engine is .NET and expressions can be tested by using [this online tool](http://regexstorm.net/tester). In the below example, the Regex is designed to match any HTTP 5xx Server Error Codes, using a Regex value of `5[0-9][0-]`.
\
**For Headers,** the format of the Header string which the Regex is applied against is {Header Name}={Header Value}. For example `Content-Type=application/json`.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (423).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (399).png" alt=""><figcaption></figcaption></figure>

### REST API source

This section has the same parameters as the usual REST API Source. For more information, see[the Rest API source page](../supported-data-sync-sources/rest-api.md)

### Request specifications
#### **Insert specification**

Select either:

* **Conditional Flow**
* **Request**
  * HTTP Method: GET, POST, PUT, PATCH, DELETE
  * Endpoint URL: Refers to where this request will be made to and inserted.

#### **Update specification**

Select either:

* **Conditional Flow**
* **Request**
  * HTTP Method: GET, POST, PUT, PATCH, DELETE
  * Endpoint URL: Refers to where this request will be made to and updated.

#### **Delete specification**

Select either:

* **Conditional Flow**
* **Request**
  * HTTP Method: GET, POST, PUT, PATCH, DELETE
  * Endpoint URL: Refers to where this request will be made to and deleted.

### Request subsections

#### Request Headers

For more information, see the page about [request headers](../building-data-syncs/advanced-settings/request-headers.md).

#### Files

Use this to upload a file to the target API.
#### Body

Use this section to add body content.

#### Variables to Extract

You can choose to specify variables to extract from your REST response.

| Parameter        | Description                                   | Example                                                                         |
| ---------------- | --------------------------------------------- | ------------------------------------------------------------------------------- |
| Name             | The name of the variable you wish to extract. | Value                                                                           |
| Extract vale from | The response area you want to extract from | Response Body, Response Header |
| Path in Response | The path to the above variable.               | $.headers.variable  |
| Header Vale Regex| Expression using .NET regex to capture a single value. See [.NET Regular Expressions](https://learn.microsoft.com/en-us/dotnet/standard/base-types/regular-expressions) for more info.| ([^\\/]+$) |

For more information, please see the [Variables](../building-data-syncs/advanced-settings/variables.md) page.