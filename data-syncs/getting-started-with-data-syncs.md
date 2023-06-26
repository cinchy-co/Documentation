# Getting Started with Data Syncs

## 1. Overview

#### What are Data Syncs?&#x20;

The Cinchy platform is purpose-built to enable a dramatic reduction in integration costs and complexity for business outcomes requiring heavy integration. To enable this further, the platform leverages a set of configurable **pre-built data sync connectors** that are able to bi-directionally read and move your data between various sources and targets. The Data Sync Connections Experience is a core functionality of data collaboration and is available with every Cinchy platform install.

**Why are Data Syncs powerful?**

Instead of keeping track of your siloed systems and the labour and financial pain that they cause, you can utilize the **Data Sync Connections Experience** to link everything together via Cinchy. You no longer need to worry about having your data locked behind disparate platforms or softwares---after a simple configuration process to define your use case(s), you can start moving your data via batch or real time syncs. Additionally, you can drill down and filter exactly what data you want to sync.

The Cinchy platform comes with a robust list of source and destination connectors and is working to expand the list over time. These connectors can be easily configured and avoid the need to create complex ETLs, APIs, and reconciliations to move data from one system to the other.

All data that is connected to the network is protected through **Universal Access Controls** and is instantly available for use in future projects, without the need for integration. The end result is a dramatic reduction in the number of copies and the associated integration cost and risk.

**In a nutshell: building a data sync allows you to bring your data in and out of systems easily and without adding on to your integration tax.**

<figure><img src="../.gitbook/assets/image (349).png" alt=""><figcaption></figcaption></figure>

## 2. Building a Data Sync

The following sections outline the steps needed for building data syncs in Cinchy. Follow the attached links to learn more.

### 2.1. Installation and Prerequisites

Before you can harness the power of Cinchy's Data Sync capabilities, you will need to install the following:

* [Connections](broken-reference)
* [Event Worker/Listener](broken-reference)

{% hint style="warning" %}
In a Kubernetes deployment of the Cinchy Platform, the above installations are bundled in, there is no manual install required.
{% endhint %}

Once installed, ensure that you have access to the following Cinchy Tables on your platform:

* Data Sync Configurations
* Execution Log
* Execution Errors
* Listener Config

### 2.2. Define Your Data Sync Type

There are two types of data syncs available in Cinchy: Batch and Real-Time.

[**Batch syncs**](building-data-syncs/types-of-data-syncs.md#1.-batch-syncs) work by processing a group or a ‘batch’ of data all together rather than each piece of data individually. Batch sync can either be run as a one-time data load operation, or it can be scheduled to run periodically.

[**Real-time syncs**](building-data-syncs/types-of-data-syncs.md#2.-real-time-data-sync) push individual events in real-time to the target system through the Cinchy listener and worker process. Each piece of data is processed as soon as it is collected or edited, therefore, producing immediate results.

Creating a Batch data sync vs. real-time data sync are similar processes. The main difference is that real-time data sync has some additional steps in the setup and does not need to be manually executed, where a Batch data sync **does** need to be manually executed.

{% hint style="warning" %}
Not every Connector will support both types of sync. This will be noted in the Overview section of the Connector's page.
{% endhint %}

#### 2.2.1. Connections Options

You can choose whether to sync data one-way in, one way out, or bi-directionally into your Data Network.

For more information on the different types of connections in Cinchy, review the following pages:

* [Supported Data Sync Sources](supported-data-sync-sources/)
* [Supported Data Sync Destinations](supported-data-sync-destinations/)
* [Supported Sync Stream Sources (for real-time syncs)](supported-real-time-sync-stream-sources/)

### 2.3. Create a Data Sync Configuration

Whether you are setting up a real-time or a batch sync, you will need to create your data sync configuration. The data sync configuration defines the source, the mapping to the target, and synchronization behavior.

To set up a data sync, you can use the Connections UI or manually input a Data Sync XML into the Data Sync Configuration table. For more information, [review the documentation here.](broken-reference)

### 2.4. Set Up a Listener Config (Real-Time Syncs)

If you are setting up a real-time sync, you will need to first set up your listener configuration. This is done in the Listener Config table.

For instructions on subscribing to your event stream, [review the documentation here.](supported-real-time-sync-stream-sources/)

### 2.5 Test and Run

Once the data sync config is created and added to the table in Cinchy, you can test your data sync:

* For batch syncs: run your job in the Connections UI.
* For real-time syncs: You can test by making changes in the source system and seeing if the target updates.

## 3. Scheduling a Data Sync

You can set up a schedule for batch syncs using a task/job scheduling application.

## 4. Monitoring a Data Sync

To review documentation on how to monitor your data syncs for errors, click here.
