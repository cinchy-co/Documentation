# Setting up Alerts

## 1. Monitoring and Alerting

Opensearch comes with the ability to set up alerts based on any number of monitors. You can then push these alerts via email, should you desire.

{% hint style="success" %}
Prior to setting up a monitor or alert, ensure that you have added your data source as an index pattern.
{% endhint %}

Definitions:

| Monitor     | A job that runs on a defined schedule and queries OpenSearch indices. The results of these queries are then used as input for one or more _triggers_. |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Trigger     | Conditions that, if met, generate _alerts_.                                                                                                           |
| Alert       | An event associated with a trigger. When an alert is created, the trigger performs _actions_, which can include sending a notification.               |
| Action      | The information that you want the monitor to send out after being triggered. Actions have a _destination_, a message subject, and a message body.     |
| Destination | A reusable location for an action. Supported locations are Amazon Chime, Email, Slack, or custom webhook.                                             |

### 1.1 Create your Destination

Your destination will be where you want your alerts to be pushed to. Opensearch supports various options, however this guide will focus on email.

1. From the left navigation pane, click **Alerting** _(Image 1)._

<figure><img src="../../../../.gitbook/assets/image (158).png" alt=""><figcaption><p>Image 1: Click on Alerting</p></figcaption></figure>

2\. Click on the **Destinations Tab > Add Destination**

3\. Add a **name** to label your destination and select **Email** as type _(Image 2)_

<figure><img src="../../../../.gitbook/assets/image (461).png" alt=""><figcaption><p>Image 2: Update your destination</p></figcaption></figure>

4\. You will need to assign a Sender. This is the email address that the alert will send from when you specify this specific destination. To add a new Sender, click **Manage Senders** _(Image 3)._

<figure><img src="../../../../.gitbook/assets/image (196).png" alt=""><figcaption><p>Image 3: Manage your Senders</p></figcaption></figure>

5\. Click **Add Sender**

6\. Add in the following information _(Image 4):_

* **Sender Name**
* **Email Address**
* **Host** (this is the host address for the email provider)
* **Port** (this is the Port of the email provider)
* **Encryption**

{% hint style="danger" %}
Ensure that you[ authenticate the Sender](#12-authenticate-your-sender), else your alert will not work.
{% endhint %}

<figure><img src="../../../../.gitbook/assets/image (249).png" alt=""><figcaption><p>Image 4: Configure your Sender</p></figcaption></figure>

7\. You will need to assign your **Recipients.** This is the email address(es) that will receive the alert when you specify this specific destination. To add a new Recipient, you can either type their email(s) into the box, or click **Manage Senders to create an email group** _(Image 5)._

<figure><img src="../../../../.gitbook/assets/image (538).png" alt=""><figcaption><p>Image 5: Configure your Recipients</p></figcaption></figure>

8\. Click **Update** to finalize your Destination.

### 1.2 Authenticate your Sender

You will need to authenticate your sender to allow emails to come through. Please contact Cinchy Customer Support to help you with this step.

* **Via email:** support@cinchy.com
* **Via phone:** 1-888-792-6051
* **Through the support portal**: [Support Portal](http://support.cinchy.com/)

### 1.3 Create your Monitor

Your monitor is a job that runs on a defined schedule and queries OpenSearch indices. The results of these queries are then used as input for one or more _**triggers**_**.**

1. From the **Alerting** dashboard, select **Monitors > Create Monitor** _(Image 6)._

<figure><img src="../../../../.gitbook/assets/image (612).png" alt=""><figcaption><p>Image 6: Create your Monitor</p></figcaption></figure>

2\. Under **Monitor Details**, add in the following information _(Image 7)._

* **Monitor Name**
* **Monitor Type** (This example uses **Per Bucket**)
  * Whereas query-level monitors run your specified query and then check whether the query’s results triggers any alerts, bucket-level monitors let you select fields to create buckets and categorize your results into those buckets.&#x20;
  * The alerting plugin runs each bucket’s unique results against a script you define later, so you have finer control over which results should trigger alerts. Each of those buckets can trigger an alert, but query-level monitors can only trigger one alert at a time.
* **Monitor Defining Method:** the way you want to define your query and triggers. (This example uses **Visual Editor**)
  * Visual definition works well for monitors that you can define as “some value is above or below some threshold for some amount of time.”
  * Query definition gives you flexibility in terms of what you query for (using [the OpenSearch query DSL](https://opensearch.org/docs/1.3/opensearch/query-dsl/full-text)) and how you evaluate the results of that query (Painless scripting).
* **Schedule:** Choose a frequency and time zone for your monitor.

<figure><img src="../../../../.gitbook/assets/image (555).png" alt=""><figcaption><p>Image 7: Define your Monitor details</p></figcaption></figure>

3\. Under **Data Source** add in the following information _(Image 8):_

* **Index:** Define the index you want to use as a source for this monitor
* **Time Field:** Select the time field that will be used for the x-axis of your monitor

<figure><img src="../../../../.gitbook/assets/image (176).png" alt=""><figcaption><p>Image 8: Configure your Data Source</p></figcaption></figure>

4\. The **Query** section will appear differently depending on the **Monitor Defining Method** selected in step 2 _(Image 9)_. This example is using the visual editor.

To define a monitor visually, select an aggregation (for example, `count()` or `average()`), a data filter if you want to monitor a subset of your source index, and a group-by field if you want to include an aggregation field in your query. At least one group-by field is required if you’re defining a bucket-level monitor. Visual definition works well for most monitors.

<figure><img src="../../../../.gitbook/assets/image (603).png" alt=""><figcaption><p>Image 9: Define your Query</p></figcaption></figure>

### 1.4 Add a Trigger

A trigger is a condition that, if met, will generate an alert.

1. To add a trigger, click the **Add a Trigger** button _(Image 10)._

<figure><img src="../../../../.gitbook/assets/image (605).png" alt=""><figcaption><p>Image 10: Add a Trigger</p></figcaption></figure>

2\. Under **New Trigger**, define your trigger name and severity level (with 1 being the highest) _(Image 11)._

<figure><img src="../../../../.gitbook/assets/image (255).png" alt=""><figcaption><p>Image 11: Define your Trigger.</p></figcaption></figure>

3\. Under **Trigger Conditions**, you will specify the thresholds for the query metrics you set up previously _(Image 12)._ In the below example, our trigger will be met if our **COUNT threshold goes ABOVE 10000.**

You can also use **keyword filters** to drill down into a more specific subset of data from your data source.

<figure><img src="../../../../.gitbook/assets/image (164).png" alt=""><figcaption><p>Image 12: Trigger Conditions</p></figcaption></figure>

4\. In the **Action** section you will define what happens if the trigger condition is met _(Image 13)._ Enter the following information to set up your **Action:**

* **Action Name**
* [**Destination** ](#11-create-your-destination)
* **Message Subject:** In the case of an email alert, this will be the email subject line.
* **Message:** In the case of an email alert, this will be the email body.
* **Perform Action:** If you’re using a bucket-level monitor, decide whether the action is performed per execution or per alert.
* **Throttling:** Enable action throttling if you wish. Use action throttling to limit the number of notifications you receive within a given span of time.

<figure><img src="../../../../.gitbook/assets/image (394).png" alt=""><figcaption><p>Image 13: Define your Actions</p></figcaption></figure>

5\. Click **Send Test Message**, if you want to test that the alert functions correctly.

6\. Click **Save.**

## Example Alerts

* [Monitor Cluster Metrics](https://opensearch.org/docs/1.3/monitoring-plugins/alerting/monitors/#create-cluster-metrics-monitor)
* [Endpoint Monitoring with Blackbox Exporter](https://github.com/prometheus/blackbox\_exporter)

### 1. Alerting on Stream Errors

In this example, we are pushing an alert based on errors. We will monitor our Connections stream for any instance of 'error', and push out an alert when our trigger threshold is hit.

1. First we create our [Monitor](#13-create-your-monitor) by defining the following _(Image 14):_

* **Index:** In this example we are looking specifically at Connections.
* **Time Field**
* **Time Range:** Define how far back you want to monitor
* **Data Filter:** We want to monitor specifically whenever the **Stream** field of our index is **stderr** (standard error).

<figure><img src="../../../../.gitbook/assets/image (246).png" alt=""><figcaption><p>Image 14: Define your Data Source and Query</p></figcaption></figure>

This is how our example monitor will appear; it shows when in the last 15 days our Connections app had errors in the log _(Image 15)._

<figure><img src="../../../../.gitbook/assets/image (537).png" alt=""><figcaption><p>Image 15: Example monitor</p></figcaption></figure>

2\. Once our monitor is created, we need to define a [trigger condition](#14-add-a-trigger). When this condition is met, the alert will be pushed out to our defined [Recipient(s).](#11-create-your-destination) In this example we want to be alerted when there is more than one stderr in our Connections stream _(Image 16)._ Input the following:

* **Trigger Name**
* **Severity Level**
* **Trigger Condition:** In this example, we use **IS ABOVE** and the threshold of **1.**

The trigger threshold will be visible on your monitoring graph as a red line.

<figure><img src="../../../../.gitbook/assets/image (290).png" alt=""><figcaption><p>Image 16: Example Trigger</p></figcaption></figure>

### 2. Alerting on Kubernetes Restarts

In this example, we are pushing an alert based on the kubectl.kubernetes.io/restartedAt annotation, which updates whenever your pod restarts. We will monitor this annotation across our entire product-mssql instance, and push out an alert when our trigger threshold is hit.

1. First we create our [Monitor](#13-create-your-monitor) by defining the following _(Image 17):_

* **Index:** In this example we are looking at our entire product-mssql instance.
* **Time Field**
* **Query:** This example is using the total count of the kubectl.kubernetes.io/restartedAt annotattion
* **Time Range:** Define how far back you want to monitor. This example goes back 30 days.

<figure><img src="../../../../.gitbook/assets/image (460).png" alt=""><figcaption><p>Image 17: Define your Query and Data Source</p></figcaption></figure>

This is how our example monitor will appear; it shows when in the last 30 days our instance had restarts _(Image 18)._

<figure><img src="../../../../.gitbook/assets/image (459).png" alt=""><figcaption><p>Image 18: Example Monitor</p></figcaption></figure>

2\. Once our monitor is created, we need to define a [trigger condition](#14-add-a-trigger). When this condition is met, the alert will be pushed out to our defined [Recipient(s).](#11-create-your-destination) In this example we want to be alerted when there is more than 100 restarts across our instance _(Image 19)._ Input the following:

* **Trigger Name**
* **Severity Level**
* **Trigger Condition:** In this example, we use **IS ABOVE** and the threshold of **100.**

The trigger threshold will be visible on your monitoring graph as a red line.

<figure><img src="../../../../.gitbook/assets/image (184).png" alt=""><figcaption><p>Image 19: Trigger Conditions</p></figcaption></figure>

### 3. Alerting on Status Codes

In this example, we are pushing an alert based on status codes. We will monitor our entire instance for 400 status codes and push out an alert when our trigger threshold is hit.

1. First we create our [Monitor](#13-create-your-monitor) by defining the following _(Image 20):_

* **Index:** In this example we are looking across out entire product-mssql-1 instance.
* **Time Field**
* **Time Range:** Define how far back you want to monitor. In this example we are looking at the past day.
* **Data Filter:** We want to monitor specifically whenever the **Status Code** is **400 (bad request).**

<figure><img src="../../../../.gitbook/assets/image (156).png" alt=""><figcaption><p>Image 20: Define your Query and Data Source</p></figcaption></figure>

This is how our example monitor will appear (note that there are no instances of a 400 status code in this graph) _(Image 21)._

<figure><img src="../../../../.gitbook/assets/image (39).png" alt=""><figcaption><p>Image 21: Example Monitor</p></figcaption></figure>

2\. Once our monitor is created, we need to define a [trigger condition](#11-create-your-destination). When this condition is met, the alert will be pushed out to our defined [Recipient(s).](#3.1-create-your-destination) In this example we want to be alerted when there is at least one 400 status code across out instance _(Image 22)._ Input the following:

* **Trigger Name**
* **Severity Level**
* **Trigger Condition:** In this example, we use **IS ABOVE** and the threshold of **0.**

The trigger threshold will be visible on your monitoring graph as a red line.

<figure><img src="../../../../.gitbook/assets/image (365).png" alt=""><figcaption><p>Image 22: Trigger Conditions</p></figcaption></figure>
