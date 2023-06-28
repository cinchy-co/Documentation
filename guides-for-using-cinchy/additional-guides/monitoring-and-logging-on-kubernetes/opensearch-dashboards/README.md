# Opensearch Dashboards

## Table of Contents

| Table of Contents                                                                                            |
| ------------------------------------------------------------------------------------------------------------ |
| [#1.-opensearch-dashboards-overview](./#1.-opensearch-dashboards-overview "mention")                         |
| [#2.-getting-started-with-opensearch-dashboards](./#2.-getting-started-with-opensearch-dashboards "mention") |
| [3. Updating your Opensearch Password](./#3.-updating-your-opensearch-password)                              |

## 1. Opensearch Dashboards Overview

When deploying Cinchy v5 on Kubernetes, we recommend using **Opensearch Dashboards** for your logging. Opensearch is a community-driven fork of Elasticsearch created by Amazon, and it captures and indexes all of your logs into a single, easily accessible **dashboard location.** These logs can be queried, searched, and filtered, and Correlation IDs mean that they can also be traced across various components. These logging components take advantage of persistent storage.

Review some of the Opensearch documentation here:

* [Introducing Opensearch](https://aws.amazon.com/blogs/opensource/introducing-opensearch/)
* [General Opensearch Documentation](https://opensearch.org/docs/latest/)
* [Using DQL (Dashboards Query Language)](https://opensearch.org/docs/latest/dashboards/dql/)
* [Troubleshooting and Common Errors](https://opensearch.org/docs/latest/troubleshoot/index/)
* [Alerts](https://opensearch.org/docs/1.0/monitoring-plugins/alerting/monitors)
  * [Anomaly Detection](https://opensearch.org/docs/2.0/monitoring-plugins/ad/index/)

## 2. Getting Started with Opensearch Dashboards

The below sections will guide you through setting up your first Index, Visualization, Dashboard, and Alert.

{% hint style="success" %}
Opensearch comes with sample data that you can use to get a feel of the various capabilities. You will find this on the main page upon logging in.
{% endhint %}

### 2.1 Defining your Log Level

1. Navigate to your **cinchy.kubernetes/environment\_kustomizations/instance\_template/worker/kustomization.yaml** file.
2. In the below code, copy the **Base64 encoded string** in the value parameter.

```yaml
patch: |-
    - op: replace
      path: /data/appsettings.json
      value: wcxJItEmCWQJQPZidpLUuV6Ll79ZUr8BimlMJysLwcxJItEmCWQJQPZidpLUuV6Ll79ZUr8BimlMJysL
```

3. [Decode the value](https://www.base64decode.org/) to retrieve your AppSettings.
4. Navigate to the below Serilog section of the code and update the **"Default"** parameter as needed to set your log level. The options are:
   1. | `Verbose`     | Verbose is the noisiest level, rarely (if ever) enabled for a production app.                                                                                                                            |
      | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
      | `Debug`       | <p>Debug is used for internal system events that are not necessarily observable from the outside, but useful when determining how something happened.<br><br>This is the default setting for Cinchy.</p> |
      | `Information` | Information events describe things happening in the system that correspond to its responsibilities and functions. Generally these are the observable actions the system can perform.                     |
      | `Warning`     | When service is degraded, endangered, or may be behaving outside of its expected parameters, Warning level events are used.                                                                              |
      | `Error`       | When functionality is unavailable or expectations broken, an Error event is used.                                                                                                                        |
      | `Fatal`       | The most critical level, Fatal events demand immediate attention.                                                                                                                                        |

```yaml
"Serilog": {
    "MinimumLevel": {
      "Default": "Debug",
```

5. Ensure that you commit your changes.
6. Navigate to **ArgoCD > Worker Application** and refresh.

### 2.1.1 Common Log Search Patterns

The following are some common search patterns when looking through your Opensearch Logs.

* **If an HTTP request to Cinchy Web/IDP fails,** check the page's requests and the relevant response headers to find the "x-correlation-id" header. That header value can be used to search and find all logs associated with the HTTP request.
* **When debugging batch syncs,** filter the "ExecutionId" field in the logs for your batch sync execution ID to narrow down your search.
* **When debugging real time syncs,** search for your data sync config name in the Event Listener or Workers logs to find all the associated logging information.

### 2.2 Setting up an Index

The first step to utilizing the power of Opensearch Dashboards is to set up an index to pull data from your sources. An Index Pattern identifies which indices you want to explore. An index pattern can point to a specific index, for example, your log data from yesterday, or all indices that contain your log data.

1. Login to Opensearch. You would have configured the access point during your [deployment installation](../../../../deployment-guide/deployment-installation-guides/); traditionally it will be found at **\<baseurl>/dashboard.**

{% hint style="info" %}
If this is your first time logging in, the username and password will be set to **admin/admin.**

We highly recommend you[ update the password as soon as possible.](https://opensearch.org/docs/latest/security-plugin/access-control/users-roles/#create-roles)
{% endhint %}

2\. Navigate to the **Stack Management** tab in the left navigation menu _(Image 1)._

![Image 1: Select Stack Management](<../../../../.gitbook/assets/image (624).png>)

3\. From the left navigation, click on **Index Patterns** _(Image 2)_.

![Image 2: Select Index Patterns](<../../../../.gitbook/assets/image (181).png>)

4\. Click on the **Create Index Pattern** button.

5\. To set up your index pattern, you must define the source. Opensearch will list the sources available to you on the screen below. Input your desired source(s) in the text box _(Image 3)._

{% hint style="info" %}
You can use the asterisk (\*) to match multiple sources.
{% endhint %}

![Image 3: Define your sources](<../../../../.gitbook/assets/image (559).png>)

6\. Configure your index pattern settings _(Image 4)._

* Time field: Select a primary time field to use with the global time filter
* Custom index pattern ID: By default, Opensearch gives a unique identifier to each index pattern. You can use this field to optional override the default ID with a custom one.

![Image 4: Configure your index pattern settings](<../../../../.gitbook/assets/image (391).png>)

7\. Once created, you can review your Index Patterns from the **Index Patterns** page _(Image 5)._

![Image 5: Review your Index Patterns](<../../../../.gitbook/assets/image (644).png>)

8\. Click on your Index Pattern to review your fields _(Image 6)._

![Image 6: Reviewing your Index Pattern fields](<../../../../.gitbook/assets/image (106).png>)

### 2.3 Create a Visualization

You can easily pull out any data from your index sources and view them in a variety of visualizations.

1. From the left navigation pane, click **Visualize** _(Image 7)._

![Image 7: Click Visualize](<../../../../.gitbook/assets/image (221).png>)

2\. If you have any Visualizations, they will appear on this page. To create a new one, click the **Create Visualization** button _(Image 8)._

![Image 8: Click Create New](<../../../../.gitbook/assets/image (392).png>)

3\. Select your visualization type from the populated list _(Image 9)._

![Image 9: Select your Visualization type](<../../../../.gitbook/assets/image (292).png>)

4\. Choose your source _(Image 10)_. If the source you want to pull data from isn't listed, you will need to [set it up as an index first.](./#2.1-setting-up-an-index)

![Image 10: Select your Source](<../../../../.gitbook/assets/image (212).png>)

5\. Configure the data parameters that appear in the right hand pane of the Create screen. These options will vary depending on what type of visualization you choose in step 3. The following example uses a pie chart visualization _(Image 11):_

* **Metrics**
  * **Aggregation:** Choose how you want your data aggregated. This example uses **Count.**
  * **Custom Label:** You can use this optional field for custom labelling.
* **Buckets**
  * **Aggregation:** Choose how you want your data aggregated. This example uses **Split Slices > Terms.**
  * **Field:** This drop down is populated based on the index source your chose. Select which field you want to use in your visualization. This example uses **machine.os.keyword.**
  * **Order By:** Define how you want your data to be ordered. This example uses **Metric: Count, in descending order of size 10.**
  * Choose wheteher to group other values in a seperate bucket. If you toggle this on, you will need to label the new bucket.
  * Choose whether to show missing values.
* **Advanced**
  * You can optionally choose a JSON input. These will be merged with the opensearch aggregation definition.
* Options
  * The variables in the options tab can be used to configure the UI of the visualization itself.

<figure><img src="../../../../.gitbook/assets/image (364).png" alt=""><figcaption><p>Image 11: Creating your Visualization</p></figcaption></figure>

6\. There are a few ways to further focus your visualization:

* [Use DQL](https://opensearch.org/docs/1.2/dashboards/dql/) to search your index data _(Image 12)._ You can also save any queries you write for easy access by clicking on the save icon.

<figure><img src="../../../../.gitbook/assets/image (330).png" alt=""><figcaption><p>Image 12: Use a query on your Visualization</p></figcaption></figure>

* Add a filter on any of your fields _(Image 13)._

<figure><img src="../../../../.gitbook/assets/image (57).png" alt=""><figcaption><p>Image 13: Add a filter on any of your fields</p></figcaption></figure>

* Update your date filter _(Image 14)._

<figure><img src="../../../../.gitbook/assets/image (289).png" alt=""><figcaption><p>Image 14: Update your date filter</p></figcaption></figure>

7\. Click save when finished with your visualization.

### 2.4 Create a Dashboard

Once you have created your visualizations, you can combine them together on one Dashboard for easy access.

{% hint style="info" %}
You can also create new visualizations from the Dashboard screen.
{% endhint %}

1. From the left navigation pane, click on **Dashboards** _(Image 15)._

![Image 15: Click Dashboards](<../../../../.gitbook/assets/image (368).png>)

2\. If you have any Dashboards, they will appear on this page. To create a new one, click the **Create Dashboard** button _(Image 16)._

![Image 16: Click Create Dashboard](<../../../../.gitbook/assets/image (84).png>)

3\. The **"Editing New Dashboard"** screen will appear. Click on **Add an Existing** object _(Image 17)._

![Image 17: Click Add An Existing](<../../../../.gitbook/assets/image (625).png>)

4\. Select any of the visualizations you created and it will automatically add to your Dashboard _(Image 18)._ Repeat this step for as many visualizations as you'd like to appear.

![Image 18: Add as many visualizations as you'd like](<../../../../.gitbook/assets/image (684).png>)

5\. Click **Save** to finish _(Image 19)._

![Image 19: Click Save.](<../../../../.gitbook/assets/image (25).png>)

## 3. Updating your Opensearch Password

{% hint style="info" %}
This capability was added in Cinchy v5.4.
{% endhint %}

Your Opensearch password can be updated in your **deployment.json** file (you may have renamed this during your original deployment).

1. Navigate to **"cluster\_component\_config" > "opensearch".**
2. There are two users that you can configure the passwords for: _Admin_ and _Kibana Server. Kibana Server_ is used for communication between the opensearch dashboard and the opensearch server. The default password for both is set to **"password"**;. To update this, you will need to use a machine with docker available.&#x20;
3. Update your Admin password:
   1. Your password must be hashed. You can do so by running the following command on a machine with docker available, inputting your new password where noted:

```
docker run -it opensearchproject/opensearch /usr/share/opensearch/plugins/opensearch-security/tools/hash.sh -p <<newpassword>>
```

&#x20;        2\. Navigate to **"opensearch\_admin\_user\_hashed\_password"** and input your hashed password.

&#x20;        3\. You must also provide your password in a **base64 encoded format**; input your cleartext password[ here](https://www.base64encode.org/) to receive your new encoded password.

&#x20;        4\. Navigate to **"opensearch\_admin\_user\_password\_base64"** and input your encoded password.

4\. Update your Kibana Server password:

&#x20;         1\. Your password must be hashed. You can do so by running the following command on a machine with docker available, inputting your new password where noted:

```
docker run -it opensearchproject/opensearch /usr/share/opensearch/plugins/opensearch-security/tools/hash.sh -p <<newpassword>>
```

&#x20;        2\. Navigate t**o "opensearch\_kibanaserver\_user\_hashed\_password"** and input your hashed password.

&#x20;        3\. You must also provide your new password in cleartext. Navigate to **"opensearch\_kibanaserver\_user\_password"** and input your cleartext password.

5\. Run the below command in the root directory of your **devops.automations repo** to update your configurations. If you have changed the name of your deployment.json file, make sure to update the command accordingly.

```
dotnet Cinchy.DevOps.Automations.dll "deployment.json"
```

4\.  Commit and push your changes.

5\. If your environment is not set-up to automatically apply upon configuration,navigate to the **ArgoCD** portal and refresh your component(s). If that does not work, **re-sync.**
