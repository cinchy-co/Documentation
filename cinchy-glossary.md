# Cinchy Glossary

## Overview

Becoming comfortable with the Cinchy terms and verbiage is an essential step to broaden your understanding of the platform and Data Collaboration as a whole. We have created this handy guide featuring to help you better understand the words and phrases you may see appear throughout this wiki space.

## B

#### Batch Sync

A Batch Sync is one of the two types of Data Syncs that you can perform using Cinchy. [Batch syncs work](data-syncs/building-data-syncs/types-of-data-syncs.md) by processing a group or a ‘batch’ of data all together rather than each piece of data individually. When the data sync is triggered  it will compare the contents of the source to the target. The Cinchy Worker will decide if data needs to be added, deleted or updated. Batch sync can either be run as a one-time data load operation, or it can be scheduled to run periodically using an external Enterprise Scheduler

## C

#### Cinchy Administrator

The [Cinchy Administrator](guides-for-using-cinchy/administrator-guide.md) is a user or user group with special [Entitlements ](cinchy-glossary.md#entitlements)within the Cinchy platform. An administrator can be either an [End-User](cinchy-glossary.md#cinchy-end-user) or a [Builder.](cinchy-glossary.md#cinchy-builder)

A Builder Admin can:

* Modify **all** table data (including system tables), **all** schema, and **all** data controls
  * This includes setting up and configuring users, assigning them to groups, and assigning which users have builder access
* View **all** tables (including system tables) and queries in the platform

A Non-Builder Admin can:

* View **all** tables (including system tables) and queries in the platform
* Modify data controls for tables

#### Cinchy Builder

[**“Cinchy Builders”**](guides-for-using-cinchy/builder-guides/) use the Cinchy platform to build an unlimited number of business capabilities and use-cases.

The “Cinchy Builder” has access to perform the following capabilities:

* Change Table Schema (use Cinchy’s “Design Table” functionality)
* Grant access (use Cinchy’s “Design Controls” functionality)
* Edit Cinchy Data in Cinchy System Tables
* Create, Save, and Share Cinchy Queries
* Perform ad hoc Cinchy Queries on the Cinchy data network
* Import/export packaged business capabilities (i.e. deployment packages)
* Build Cinchy Experiences
* Perform integration with Cinchy (e.g. Cinchy Command Line Interface \[CLI] operations)
* Create and Deliver an unlimited number of Customer Use Cases within Cinchy
* A builder can be part of the[ Administrators group](guides-for-using-cinchy/administrator-guide.md)

#### Cinchy CLI

The [Cinchy Command Line Interface (CLI) ](data-syncs/installation-and-maintenance/installing-the-cli-and-the-maintenance-cli.md)is a tool that offers utilities for syncing data in and out of Cinchy via [various commands.](data-syncs/cli-commands-list.md)

#### CinchyDXD

CinchyDXD is a downloadable utility used to move Data Experiences (DX) from one environment to another. This includes any and all objects and components that have been built for, or are required in, support of the Data Experience.

#### Cinchy ID

Cinchy ID is a unique identifier assigned automatically to all records within a normal table. The Cinchy ID is associated with the record permanently and is never reassigned even if the record is deleted.&#x20;

#### Cinchy Query Language (CQL)

[Cinchy Query Language (CQL)](cql/the-basics-of-cql/cql-functions-master-list.md) is the Data Collaboration version of common query languages such as SQL. With a robust set of functions and commands, you can use CQL to retrieve and manage/modify data and metadata from tables in your network. While data can reside across many tables, a query can isolate it to a single output, making the possibilities of CQL endlessly powerful.

**Cinchy Query Language can be used in many ways, including but not limited to:**

* ​Building queries through the query editor that can return, insert, delete, and otherwise manage your data.
* ​Creating, altering or dropping views for tables​
* ​Creating, altering or dropping indexes​

#### Cinchy Upgrade Utility

[The Cinchy Upgrade Utility](deployment-guide/upgrade-guides/upgrading-cinchy-versions/cinchy-upgrade-utility.md) is an easy to use tool that can help you deploy important changes and upgrades to your Cinchy environment when upgrading versions. Note that not every major or minor release version will require the Utility to be run, however this will be clearly noted in the release notes and upgrade guide.

#### Cinchy End-User

[The **“End-Users”**](guides-for-using-cinchy/user-guides/) of the Cinchy platform are those that apply the functionalities created by the **“Cinchy Builders”** to their business objectives. This can be employees, customers, partners, or systems. There are two types of end-user: direct and indirect.

* **Direct Users** log into Cinchy via the data browser
* **Indirect Users** (also commonly referred to as "external users") view/edit data via a third-party application/page that connects to Cinchy via API

Cinchy End-Users are able to:

* Create and save personal queries. Unlike traditional saved queries made by builders, _**personal**_ saved queries cannot be shared and are not auto-exposed as APIs.
* Use Tables, Saved Queries, and Experiences created by [“Builders"](guides-for-using-cinchy/builder-guides/#what-is-a-builder)
* Track version history for the full lifecycle of data
* Bookmark and manage data
* Access data through application experiences
* An end-user can be part of the [Administrators group](guides-for-using-cinchy/administrator-guide.md)

#### Connections Experience

The Connections Experience is an integral part of Cinchy's [Data Collaboration](cinchy-glossary.md#data-collaboration) offerings. Serving as the front-end UI for performing[ Data Syncs](cinchy-glossary.md#data-synchronizations), the Connections Experience can be accessed natively in your Cinchy browser in order to create, configure, and manage the data being synced in and out of the platform. The user friendly interface makes synchronizing your data across a range of apps and softwares easy.

## D

#### Dataware

Dataware is the emergence of a common group of technologies that solve data related problems across many business use cases. One of the most exciting categories in this group is [Data Collaboration.](cinchy-glossary.md#data-collaboration)

#### Data Browser

The [Data Browser](https://platform.docs.cinchy.com/guides-for-using-cinchy/user-guides/overview-of-the-data-browser) is the homepage of your Cinchy platform. From here you can view any [Tables](cinchy-glossary.md#table), [Queries](cinchy-glossary.md#queries-saved-queries), or [Experiences](cinchy-glossary.md#experiences) that you have access to. You can also use it to search and review any bookmarks that you have saved.

#### Data Collaboration

Data Collaboration entails the collection, exchange and use of data from different origins within an organization. This interaction creates data products with data owners acting as experts for their specific data domains. The interaction of these various data set domains also results in dramatically informed insights that are solely driven by the interactions of the data itself.

#### Data Destination(s)

When setting up a Data Sync, you must define which destination to push data to. Cinchy maintains [a robust list](data-syncs/supported-data-sync-destinations/) of Data Destination connectors and is always working on adding more into the [Connections Experience](cinchy-glossary.md#connections-experience).

#### Data Source(s)

When setting up a Data Sync, you must define which source to pull data from. Cinchy maintains [a robust list](data-syncs/supported-data-sync-sources/) of Data Source connectors and is always working on adding more into the [Connections Experience.](cinchy-glossary.md#connections-experience)

#### Data Synchronizations

Data Synchronizations ("Data Syncs") are a powerful and important aspect of the Cinchy platform. Using a Data Sync allows you to bidirectional push and pull data between various [Data Sources](cinchy-glossary.md#data-source-s) and [Data Destinations](cinchy-glossary.md#data-destination-s), including Salesforce, Snowflake, REST APIs, and more. Built with keeping the core tenants of Data Collaboration in mind, Data Syncs adhere to your set [Entitlements](cinchy-glossary.md#entitlements) and can be configured using the intuitive [Connections Experience](cinchy-glossary.md#connections-experience) to enable a myriad of use-cases for your business.

## E

#### Entitlements

Entitlements refers to the set of permissions that you are granted for any piece of data. Cinchy allows entitlements to be set at a very granular level, meaning you can give individual users or user groups access to things like:

* Viewing a data sync
* Running a job
* Viewing a specific table row or column
* Editing a specific table cell
* Etc.

Entitlements can persist across the platform when using features such as link columns or the [Network Map.](cinchy-glossary.md#network-map)

#### Experiences

Rather than traditional code-centric applications which creates data silos, you can build [metadata driven application experiences](https://platform.docs.cinchy.com/guides-for-using-cinchy/additional-guides/application-experiences) directly on the Cinchy platform. These look and feel like regular applications, but persists its data on the data network autonomously, rather than managing its own persistence. These experiences automatically adapt as your data evolves. An Org Chart is an example of a metadata driven Experience you can create using the power of [Data Collaboration](cinchy-glossary.md#data-collaboration) on Cinchy.

## L

#### Listener Configuration

When setting up real-time data syncs, you will need to [configure the Cinchy Listener](broken-reference) for your [event stream sources ](data-syncs/supported-real-time-sync-stream-sources/)so that it knows what data to pull/push through your sync. Each stream source has different variables and parameters that you can use to refine your sync. Note that the Listener only needs to be configured for [real-time syncs](cinchy-glossary.md#real-time-sync), not for [batch syncs.](cinchy-glossary.md#batch-sync)

## M

#### MDQE

MDQE, which stands for [**Metadata Quality Exceptions**](guides-for-using-cinchy/additional-guides/mdqe.md), can send out notifications based on a set of rules and the “exceptions” that break them. This powerful tool can be used to send notifications for exceptions such as:

* Healthchecks returning a critical status
* Upcoming Project Due Dates/Timelines
* Client Risk Ratings reaching a high threshold
* Tracking Ticket Urgency or Status markers
* Unfulfilled and Pending Tasks/Deliverables
* Etc.

In a nutshell, MDQE monitors for specific changes in data, and then pushes out notifications when that change occurs.

#### Meta Forms

[The Meta-Forms experience](meta-forms/introduction-to-meta-forms.md) is a combination of Angular code packaged as an App Experience and a Data Model packaged as a Data Experience that enables users to interact with Cinchy data in a User-Friendly manner.

It lives on top of the data collaboration platform, allowing builders to create forms with custom configurations and users to access this data outside of the tabular view that is native to Cinchy.

## N

#### Network Map

Cinchy comes out of the box with a system applet called ["Network Map"](guides-for-using-cinchy/additional-guides/application-experiences/network-map/), which is a visualization of your data on the platform and how everything interconnects; it's another way to view and navigate the data you have access to within Cinchy.

Each node represents a table you have access to within Cinchy, and each edge is one link between two tables. The size of the table is determined by the number of links referencing that table. The timeline on the bottom allows you to check out your data network at a point in the past and look at the evolution of your network.

It uses your [Entitlements](cinchy-glossary.md#entitlements) for viewable tables and linked columns.

## Q

#### Queries/Saved Queries

Queries are the fundamental calculations that allow data to be pushed, pulled, and manipulated across a range of Cinchy features. A query can be used to call data, change data, delete data, or whatever functionality your use case requires. Cinchy comes out of the box with a native Query Builder and works with our proprietary [Cinchy Query Language.](cinchy-glossary.md#cinchy-query-language-cql)

A Saved Query is merely a query that has been saved in order to be reused. These can be accessed via the Saved Queries table in Cinchy, and you have access to view or run them based on your [entitlements.](cinchy-glossary.md#entitlements)

## R

#### Real Time Sync

[A Real-Time Sync](data-syncs/building-data-syncs/types-of-data-syncs.md#2.-real-time-data-sync) is one of the two types of Data Syncs that you can perform using Cinchy. In real-time syncs, the Cinchy Listener picks up changes in the source **immediately as they occur.** These syncs don't need to be manually triggered or scheduled using an external scheduler. Setting up a real-time sync does require an extra step of defining a listener configuration in order to execute properly.
