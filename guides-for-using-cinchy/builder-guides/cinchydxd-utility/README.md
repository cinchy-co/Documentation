---
description: This page guides you through the Cinchy Data Experience Deployment Utility.
---

# CinchyDXD utility

## Introduction to CinchyDXD

CinchyDXD is a downloadable utility used to move Data Experiences (DX) from one environment to another. This includes any and all objects and components that have been built for, or are required in, support of the Data Experience.

The following sections in this document will outline the basics of how to build, export**,** and install a DX’s.

Items of note moving forward in this document:

* **Source Environment** is the environment in which the DX is built.

{% hint style="danger" %}
All objects need to be created in one source environment (ex: DEV). From there, DXD will be used to push them into others (ex: SIT, UAT, Production).
{% endhint %}

* **Target Environment** is the environment in which the DX will be installed
* The example DX is a simple Currency Converter DX that consists of
  * One (1) table
  * One (1) query
* This example doesn't include the following:
  * NO applets
  * NO integrated clients
  * NO Data Sync Configurations
  * NO Reference Data
  * NO Models
  * NO Groups
  * NO System Colours
  * NO Formatting Groups
  * NO Literal Groups

Future iterations of this document will add to this example's complexity level.

## Steps

The general steps to deploying the CinchyDXD Utility are as follows:

* [building-the-data-experience.md](building-the-data-experience.md "mention")
* [packaging-the-data-experience.md](packaging-the-data-experience.md "mention")
* [installing-the-data-experience.md](installing-the-data-experience.md "mention")
* [updating-the-data-experience.md](updating-the-data-experience.md "mention")
* [repackaging-the-data-experience.md](repackaging-the-data-experience.md "mention")
* [reinstalling-the-data-experience.md](reinstalling-the-data-experience.md "mention")
