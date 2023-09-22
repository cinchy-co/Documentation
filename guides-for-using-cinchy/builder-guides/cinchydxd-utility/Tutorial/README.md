---
description: This page guides you through the Cinchy Data Experience Deployment Utility.
---

# CinchyDXD utility

## Introduction to CinchyDXD

CinchyDXD is a downloadable utility to move Data Experiences (DX) from one environment to another. This includes all objects and components created or required to support the Data Experience.

The following sections in this document will outline the basics of how to build, export, and install a data experience (DX) using a simple Currency Converter DX. 

## Before you begin

Before you start, please note the following:

* The **Source Environment** is the environment where the DX is built.
* The **Target Environment** is the environment where the DX will be installed.
* The sample DX consists of:
  * One (1) table
  * One (1) query
* This example doesn't include the following:
  * Applets
  * Integrated clients
  * Data Sync Configurations
  * Reference Data
  * Models
  * Groups
  * System Colours
  * Formatting Groups
  * Literal Groups

{% hint style="danger" %}
All objects need to be created in one source environment (such as DEV). From there, DXD will be used to push them into others (such as SIT, UAT, Production).
{% endhint %}



Future iterations of this document will add to this example's complexity level.

## Steps

The general steps to deploying the CinchyDXD Utility are as follows:

* [Build the data experience](building-the-data-experience.md)
* [Package the data experience](packaging-the-data-experience.md)
* [Install the data experience](installing-the-data-experience.md)
* [Update the data experience](updating-the-data-experience.md)
* [Repackage the data experience](repackaging-the-data-experience.md)
* [Reinstall the data experience](reinstalling-the-data-experience.md)
