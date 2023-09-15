# MongoDB Collection

## Overview

[MongoDB](https://www.mongodb.com/what-is-mongodb/features) is a scalable, flexible NoSQL document database platform known for its horizontal scaling and load balancing capabilities, which has given application developers an unprecedented level of flexibility and scalability.

{% hint style="info" %}
Before you set up your data sync destination, [make sure to configure your Source.](../supported-data-sync-sources/)
{% endhint %}

{% hint style="success" %}
The MongoDB Collection destination supports batch and real-time syncs.
{% endhint %}

## Destination tab

The following table outlines the mandatory and optional parameters you will find on the Destination tab _(Image 1)._

{% tabs %}
{% tab title="Destination Details" %}
The following parameters will help to define your data sync destination and how it functions.

| Parameter         | Description                                                                                                                               | Example                                                                           |
|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| Destination       | Mandatory. Select your destination from the drop down menu.                                                                               | MongoDB Collection                                                                |
| Connection String | Mandatory. The encrypted connection string for your MongoDB Collection. The Connections UI will automatically encrypt this value for you. | You can review MongoDB's Connection String guide and parameter descriptions here. |
| Database          | Mandatory. The name of your MongoDB database.                                                                                             | Cinchy                                                                            |
| Collection        | Mandatory. The name of your MongoDB collection.                                                                                           | Employees                                                                         |
| Use SSL           | This checkbox can be used to define the use of x.509 certificate authentication for your sync. If checked, you will need to input the following values taken from your cert:- SSL Key PEM- SSL Certificate PEM- SSL CLA PEM |
{% endtab %}

{% tab title="Column Mapping" %}
**The** [**Column Mapping**](../building-data-syncs/columns-and-mappings/#3.-column-mappings) **section** is where you define which source columns you want to sync to which destination columns. You can repeat the values for multiple columns.

| Parameter     | Description                                                              | Example |
|---------------|--------------------------------------------------------------------------|---------|
| Source Column | **Mandatory.** The name of your column as it appears in the source.      | Name    |
| Target Column | **Mandatory.** The name of your column as it appears in the destination. | Name    |
{% endtab %}
{% endtabs %}

<figure><img src="../../.gitbook/assets/image (653).png" alt=""><figcaption><p>Image 2: Define your Destination</p></figcaption></figure>

## Retry Configuration

Cinchy v5.6 introduced the Retry Configuration for MongoDB targets. This will automatically retry HTTP Requests on failure based on timeout or connection errors. If the final retry attempt fails it gets logged into the Execution Errors table

This capability provides a mechanism to recover from transient errors such as network disruptions or temporary service outages.

{% hint style="warning" %}
The maximum number of retries is capped at 10.
{% endhint %}

To set up a retry configuration:

1. Under the MongoDB destination tab, select **Retry Configuration**

<figure><img src="../../.gitbook/assets/image (753).png" alt=""><figcaption></figcaption></figure>

2. Select your Delay Strategy.

* **Linear Backoff:** Defines a delay of approximately n seconds where n = current retry attempt.
* **Exponential Backoff:** A strategy where every new retry attempt is delayed exponentially by 2^n seconds, where n = current retry attempt.
  * _Example: you defined Max Attempts = 3. Your first retry is going to be in 2^1 = 2, second: 2^2 = 4, third: 2^3 = 8 sec._

3\. Input your Max Attempts. The maximum number of retries allowed is 10.

## Next steps

* Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
* If you are running a real-time sync, [set up your Listener Config](../supported-real-time-sources/) and enable it to begin your sync.
* If you are running a batch sync, click **Jobs > Start a Job** to begin your sync.
