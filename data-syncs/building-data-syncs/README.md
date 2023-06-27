# Building Data Syncs

## Step-by-Step Guide

You can use this checklist in conjunction with the documentation below and elsewhere in this space to configure your data syncs in Cinchy.

* [ ] [Install any applicable components](../installation-and-maintenance/)
* [ ] Determine your [Data Sync Type](types-of-data-syncs.md) and [Design Pattern](common-design-patterns.md)
* [ ] Define and Configure your [Data Sync Source](../supported-data-sync-sources/)
* [ ] Define and Configure your [Data Sync Destination](../supported-data-sync-destinations/)
* [ ] Define and Configure your [Sync Actions](sync-actions.md)
* [ ] Define and Configure your [Event Stream source](../supported-real-time-sync-stream-sources/) (if running a real-time sync)

## 1. Overview

Once you have installed all of your necessary components and decided upon which type of data sync you'd like to use, the next step is to configure your data sync. This involves a series of steps that are outlined in the sections below.

There are two options when you want to create a data sync in Cinchy.&#x20;

* You can input all of your necessary information through the intuitive Connections UI. Once saved, all of this data is uploaded as an XML into the Data Sync configurations table.
* Or, you can bypass the UI and upload your XML config directly into the Data Sync configuration table yourself.

For real-time syncs only, you must also set up a listener configuration.

## 2. Create a Data Sync Configuration

Whether you are setting up a real-time or a batch sync, you will need to create your data sync configuration. The data sync configuration defines the source, the mapping to the target, and synchronization behavior.

To set up a data sync, you can use the Connections UI or manually input an XML into the Data Sync Configuration table in Cinchy.

### 2.1 Using the Connections UI

1. Navigate to the **Connections Experience** in Cinchy _(Image 1)._

<figure><img src="../../.gitbook/assets/image (413).png" alt=""><figcaption><p>Image 1: The Cinchy Connections Experience</p></figcaption></figure>

2. In the experience, there are six tabs that you must or can input data for in order to define your connection _(Image 2)._

<figure><img src="../../.gitbook/assets/image (389).png" alt=""><figcaption><p>Image 2: The Connections Tabs</p></figcaption></figure>

3. **The Info tab** is used to define some basic information about your data sync such as its name and permissions set _(Image 3)._ This is a role based access system where you can give specific groups read, write, execute, and/or all of the above with admin access. Inputting a name and an Admin Group are mandatory. You also have the optional ability to add in Variables; [please review the documentation here](advanced-settings/variables.md) for more details on Variables.

<figure><img src="../../.gitbook/assets/image (680).png" alt=""><figcaption><p>Image 3: The Info Tab</p></figcaption></figure>

4. **The Source tab** is used to define important information about the source of your data sync _(Image 4)_. This tab is mandatory. Cinchy supports many different source options including different file types and popular software systems. Each source will have different, and often unique, parameters that must be populated in the Source tab screen. You can review the full list of supported data sources, as well as their unique parameters and features, [here.](../supported-data-sync-sources/)

<figure><img src="../../.gitbook/assets/image (741).png" alt=""><figcaption><p>Image 4: The Source Tab</p></figcaption></figure>

[For event based real-time sync sources](../supported-real-time-sync-stream-sources/) (such as the Cinchy Event Broker/CDC, Kafka Topic, MongoDB Event, Polling Event, Salesforces Platform Event, or the Salesforce Push Topic) you will see an addition configuration tab to configure your Listener. Any configuration populated via the UI here will automatically reconcile back to the Listener Config table in your platform. You are able to set the:

* Topic JSON
* Connections Attribute(s)
* Auto Offset Reset

You can find more information about the Listener Config settings in the relevant [Sync Source](../supported-data-sync-sources/) or [Real-Time Sync Stream Source](../supported-real-time-sync-stream-sources/) page(s).

{% hint style="info" %}
Note: If there is more than one listener associated with your data sync, you will need to configure it via [the Listener Configuration table](../supported-real-time-sync-stream-sources/the-listener-configuration-table.md) instead.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (693).png" alt=""><figcaption></figcaption></figure>

5. **The Destination tab** is used to define important information about the target of your data sync _(Image 5)_. This tab is mandatory. Like with sources, Cinchy supports many different destination options. Each destination must be properly mapped to its source, and some may have unique parameters that must be populated in the Destination tab. You can review the full list of supported destinations, as well as their unique parameters and features, [here.](../supported-data-sync-destinations/)

<figure><img src="../../.gitbook/assets/image (493).png" alt=""><figcaption><p>Image 5: The Destination tab</p></figcaption></figure>

6. **The Sync Actions tab** defines what you want to happen to your data _(Image 6)_. This tab is mandatory. There are two options on this page: Full File Sync and Delta Sync. You can review the differences between them [here.](sync-actions.md)

<figure><img src="../../.gitbook/assets/image (713).png" alt=""><figcaption><p>Image 6: Sync Actions</p></figcaption></figure>

7. [**The Post Sync tab**](advanced-settings/post-sync-scripts.md) is an optional field that utilizes [Cinchy Query Language (CQL)](broken-reference) to perform actions on your resulting data _(Image 7)_. For example, you could set up a post sync script to push retrieved data values into a specific Cinchy table.

<figure><img src="../../.gitbook/assets/image (660).png" alt=""><figcaption><p>Image 7: Post Sync Scripts</p></figcaption></figure>

8.  **The Jobs tab** will appear when you are configuring a **batch data sync** _(Image 8)_. This page allows you to start and track your batch jobs, and will show important info on any job successes or failures. By default, the job will run as whichever user is logged in (as long as you have authority to run the Job). You have the option to run it as another, non-SSO account if:

    * You have the credentials
    * The account has access to run the Job

    You can configure this by clicking on **Advanced > Run Job as a Different User**

**The Jobs tab** also gives you the option to review your sync Output or download the logs of succesful or failed syncs.

<figure><img src="../../.gitbook/assets/image (698).png" alt=""><figcaption><p>Image 8: The Jobs tab</p></figcaption></figure>

10. **The Execution tab** will appear when you are configuring a **real-time sync** _(Image 9)_. This provides useful information for tracking any errors associated with your real-time sync. Since you don't need to click "Start a Job" in the UI for real-time syncs, a sync is considered active when your Listener Config is set up and turned to "enabled", which you can do through this tab.

<figure><img src="../../.gitbook/assets/image (678).png" alt=""><figcaption><p>Image 9: Execution Errors</p></figcaption></figure>



### 2.2. Using a Config XML

In lieu of using the Connections UI, you can also set up a data sync by uploading a correctly formatted XML into the Data Sync Configs table within Cinchy.

We recommend only doing so once you have a good grasp on how data sync work. Note that not all sources/targets follow the same XML pattern, but you can review a basic version that uses a Delimited File source into a Cinchy Table [here.](batch-data-sync-example.md)

To set up a data sync using a config XML:

1. In the Cinchy platform, navigate to the Data Sync Config table _(Image 10)._

<figure><img src="../../.gitbook/assets/image (177).png" alt=""><figcaption><p>Image 10: Data Sync Configurations table</p></figcaption></figure>

2. In a new row, paste your **Data Sync XML** into the **Config XML** column.&#x20;
3. Define your group permissions in the applicable columns.
4. Once you have completed your Data Sync XML, navigate to the Data Sync Configurations table in Cinchy _(Image 11)._

{% hint style="info" %}
The Name and Config Version columns will be auto populated as they values are coming from the Config XML.
{% endhint %}

{% hint style="success" %}
Tip: Click on the below image to enlarge it.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (89).png" alt=""><figcaption><p>Image 11: Config XML</p></figcaption></figure>

{% hint style="info" %}
Be sure when you are pasting into the Config XML column that you double click into the column before pasting, otherwise each line of the XML will appear as an individual record in the Data Sync Configurations table.
{% endhint %}

5. To execute your Data Sync you will use the CLI. If you do not have this downloaded, [refer to the documentation here.](../installation-and-maintenance/installing-the-cli-and-the-maintenance-cli.md)
6. In this example we will be using the following Data Sync Commands, however, for the full list of commands click [here.](../cli-commands-list.md)

<table><thead><tr><th width="199.33333333333331">Parameter</th><th width="241">Description</th><th>Example</th></tr></thead><tbody><tr><td>-s (server)</td><td><mark style="color:orange;"><strong>Required</strong>.</mark> The full path to the Cinchy server without the protocol (e.g. cinchy.co/Cinchy).</td><td>"pilot.cinchy.co/Training/Cinchy/"</td></tr><tr><td>-u (userid)</td><td><mark style="color:orange;"><strong>Required</strong>.</mark> The user id to login to Cinchy that has execution access to the data sync.</td><td>"admin"</td></tr><tr><td>-p (password)</td><td><mark style="color:orange;"><strong>Required</strong>.</mark> The password of the above User ID parameter. This must be encrypted. For a walkthrough on how to use the CLI to encrypt the password, refer to the Appendix section. </td><td>"DESuEGqfffsamx55yl256hjuPYxa4ncc+5+bLkoVIFpgs0Lq6hkcU="</td></tr><tr><td>-f (feed)</td><td><mark style="color:orange;"><strong>Required</strong>.</mark> The name of the Data Sync Configuration as defined in Cinchy</td><td>"Data Sync Name"</td></tr></tbody></table>

5. Launch Powershell and navigate to the Cinchy CLI directory.
6. Enter and execute the following into Powershell:

```
.\Cinchy.CLI.exe syncdata -s "pilot.cinchy.co/Training/Cinchy/" -u "admin" -p "DESuEGqmx55yl2PYxa4ncc+5+bLkoVIFpgs0Lq6hkcU=" -f "Data Sync Name"
```

### 2.3. Set Up a Listener Config (Real-Time Syncs)

Setting up a Listener Configuration is a required step when doing a real-time data sync. You will configure your Event Stream Source with your data sync information. You can review an more on the [Listener Config here.](broken-reference)

1. Navigate to the **Listener Config table** in Cinchy _(Image 12)._

<figure><img src="../../.gitbook/assets/image (503).png" alt=""><figcaption><p>Image 12: Listener Config table</p></figcaption></figure>

2. In a new row, add in your listener config configuration data. [Review the documentation here](../supported-real-time-sync-stream-sources/) for more information.
3. Ensure that it is set to Enabled in order for your real-time data sync to run successfully.

## 3. Examples

The following subsections provide basic examples of both batch and real-time data syncs. These simple use cases can be used as a jumping off reference point for learning the ropes of Cinchy daata syncs.

* [Batch Data Sync Example](batch-data-sync-example.md)
* [Real-Time Data Sync Example](real-time-sync-example.md)
