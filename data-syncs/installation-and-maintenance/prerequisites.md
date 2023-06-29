# Prerequisites

## Overview

To install the components necessary to run a Cinchy Data Sync, you need to set up some environmental configurations. The following section contains the prerequisites and server size required for setting up both a batch data sync and a real-time data sync.

{% hint style="warning" %}
In a Kubernetes deployment of the platform, both Connections and the Worker/Listener are deployed automatically upon first install of Cinchy.
{% endhint %}

## Prerequisites**

* Windows Server 2012+
  * [.NET Core 3.1 Hosting Bundle ](https://dotnet.microsoft.com/download/dotnet-core/2.1)
* SQL Server 2012+
  * Service Broker enabled
* Cinchy Platform
* Server Sizing minimum requirements:
  * 4GB of RAM
  * 30GB of HDD space
  * 2 CPU Cores
