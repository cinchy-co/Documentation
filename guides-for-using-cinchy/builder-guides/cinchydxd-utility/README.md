---
description: This page guides you through the Cinchy Data Experience Deployment Utility.
---

# CinchyDXD Utility

## **1. Introduction to Cinchy DXD**

CinchyDXD is a downloadable utility used to move Data Experiences (DX) from one environment to another. This includes any and all objects and components that have been built for, or are required in, support of the Data Experience.

The following sections in this document will outline the basics of how to build, export**,** and install a DX’s.

Items of note moving forward in this document:

* **Source Environment** is the environment in which the DX is built.

{% hint style="danger" %}
All objects need to be created in one source environment (ex: DEV). From there, DXD will be used to push them into others (ex: SIT, UAT, Production).
{% endhint %}

* **Target Environment** is the environment in which the DX will be installed
* The example DX is a simple Currency Converter DX that consists of&#x20;
  * One (1) table
  * One (1) query
* This example does not include the following:
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

## 2. Overview of Steps

The general steps to deploying the CinchyDXD Utility are as follows:

* [building-the-data-experience-cinchydxd.md](building-the-data-experience-cinchydxd.md "mention")
* [packaging-the-data-experience-cinchydxd.md](packaging-the-data-experience-cinchydxd.md "mention")
* [installing-the-data-experience-cinchydxd.md](installing-the-data-experience-cinchydxd.md "mention")
* [updating-the-data-experience-cinchydxd.md](updating-the-data-experience-cinchydxd.md "mention")
* [repackaging-the-data-experience-cinchydxd.md](repackaging-the-data-experience-cinchydxd.md "mention")
* [reinstalling-the-data-experience-cinchydxd.md](reinstalling-the-data-experience-cinchydxd.md "mention")
