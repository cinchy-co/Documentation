# Get started with data syncs

## Overview

### Introduction to data syncs

The Cinchy platform reduces integration costs and complexity. Through configurable data sync connectors, it enables bi-directional data movement between different sources and targets, enhancing data collaboration and minimizing integration overhead with the included Data Sync Connections Experience.

## Features

Data syncs remove the hurdles of managing siloed systems. With the Data Sync Connections Experience, link systems via Cinchy using tailored configurations for real-time or batch data transfers. Choose the specific data subsets for syncing to meet your requirements.

Cinchy offers an evolving list of source and destination connectors, erasing the need for complex ETL pipelines and APIs.

With Universal Access Controls, Cinchy safeguards every connected data piece, ensuring instant access for future endeavors and reducing integration costs, risks, and data duplication.

<figure><img src="../.gitbook/assets/image (163).png" alt=""><figcaption></figcaption></figure>

## Quick start guide

### 1. **Installation and prerequisites**

#### Kubernetes

A Kubernetes deployment of the Cinchy Platform includes the Connections and Event Worker/Listener.

#### IIS 

For an IIS deployment, install:

* [Connections](./installation-and-maintenance/installing-connections)
* [Event Worker/Listener](./installation-and-maintenance/installing-the-worker-listener.md)

**Post-installation:** Ensure you have access to these Cinchy Tables:
* Data Sync Configurations
* Execution Log
* Execution Errors
* Listener Config

### 2. **Define your data sync type**

Cinchy offers two types of data syncs: 

* [**Batch syncs**](building-data-syncs/types-of-data-syncs.md#1.-batch-syncs): Process a batch of data together. Suitable for one-time data loads or periodic runs.
* [**Real-time syncs**](building-data-syncs/types-of-data-syncs.md#2.-real-time-data-sync): Push individual data events in real-time through the Cinchy listener and worker process.

{% hint style="warning" %}
Remember, not every Connector supports both types of syncs. Visit the specific data source and destination pages for details.
{% endhint %}

### 3. **Choose connection options**

Decide on your data sync direction:
* One-way in
* One-way out
* Bi-directional

Explore various connection types:
* [Supported data sync sources](supported-data-sync-sources/)
* [Supported data sync destinations](supported-data-sync-destinations/)
* [Supported sync stream sources (for real-time syncs)](supported-real-time-sync-stream-sources/)

### 4. **Create a data sync configuration**

For both real-time and batch syncs, you'll need to set up your configuration:

* Define the source
* Map to the target
* Determine synchronization behavior

Use the Connections UI or manually input a Data Sync XML into the Data Sync Configuration table. More details are on the [Building data syncs page.](../data-syncs/building-data-syncs/README)

### 5. **Set up Listener Config (For Real-time Syncs)**

For real-time syncs, configure your listener in the **Listener Config** table. [See the supported real-time sync stream sources page](supported-real-time-sync-stream-sources/) for subscribing to your event stream.

### 6. **Test and Run**

After setting up the configuration:
* **Batch syncs:** Execute your job through the Connections UI.
* **Real-time syncs:** Test by altering the source system and checking if the target updates.

### 7. **Schedule Your Data Sync**

Batch syncs can be scheduled using a task/job scheduling application.

