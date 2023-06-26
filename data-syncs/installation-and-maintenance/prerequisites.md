# Prerequisites

## 1. Overview

In order to install the components necessary to run a Cinchy Data Sync, there are some environmental configurations that will need to be set up. The following section contains the prerequisites and server size required for setting up both a batch data sync and a real-time data sync.

{% hint style="warning" %}
In a Kubernetes deployment of the platform, both Connections and the Worker/Listener are deployed automatically upon first install of Cinchy.&#x20;
{% endhint %}

## **2. Prerequisites**

* Windows Server 2012+
  * .NET Core 3.1 Hosting Bundle (click [here ](https://dotnet.microsoft.com/download/dotnet-core/2.1)to download)
* SQL Server 2012+
  * Service Broker enabled
* Cinchy Platform
* Server Sizing minimum requirements:
  * 4GB of RAM
  * 30GB of HDD space
  * 2 CPU Cores
