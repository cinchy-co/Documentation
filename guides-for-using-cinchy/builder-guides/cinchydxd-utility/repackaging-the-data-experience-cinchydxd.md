---
description: >-
  This page outlines Step 5 of Deploying CinchyDXD: Repackaging the Data
  Experience
---

# Repackaging the Data Experience (CinchyDXD)

## Introduction

After you have made any applicable changes to your DX it is time to re-export the package out of your source environment.&#x20;

## 1. Update the Data Experience Table

If you have added or removed any of the following while updating your DX, you will need to update the **Data Experience Definition** table:

* Name
* Tables
* Integrated Clients
* Data Sync Configurations
* Reference Data
* User Defined Functions
* Models
* Groups
* System Colours
* Saved Queries
* Applets
* Literal Groups
* Builders
* Builder Groups
* Sync GUID

## 2. Update Reference Data Table

If you have added or removed any of the following while updating your DX, you will need to update the **Data Experience Reference Data** table:

* Name
* Ordinal
* Filter
* New Records
* Changed Records
* Dropped Records
* Table
* Sync Key
* Expiration Timestamp Field
* Sync GUID

## 3. Re-Run CinchyDXD Export

Using PowerShell you will now export the Data Experience you have defined within Cinchy.

1. Launch PowerShell and navigate to your **CinchyDXD** folder

{% hint style="success" %}
You can launch PowerShell right from your file explorer window in the CinchyDXD file, saving you an extra step of navigating to the CinchyDXD folder manually in PowerShell.
{% endhint %}

2\. In the PowerShell window type in **cin** and hit tab on your keyboard\
3\. In the PowerShell command line next to .\CinchyDXD.ps1 type in **export**\
4\. Hit Enter on your keyboard

{% hint style="info" %}
If you do not remember the mandatory parameters, you can click the enter on your keyboard after typing in .\CinchyDXD.ps1 export, PowerShell will provide you with the required and optional components to export the data experience.
{% endhint %}

5\. You must now enter your mandatory export parameters.

{% hint style="info" %}
The parameters executed in PowerShell can exist on one line in PowerShell, however for legibility (below) the parameters have been put on separate lines.  If you are putting your parameters on separate lines you will be required to have backticks quote \`  for the parameters to execute
{% endhint %}

{% hint style="warning" %}
You will need to update your version number
{% endhint %}

\
Sample:\
.`` \CinchyDXD.ps1 export ` ``\
`` -s "<source Cinchy url>" ` ``\
`` -u "<source user id>" ` ``\
`` -p "<source password>" ` ``\
`` -c "C:\Cinchy CLI v4.0.2" ` ``\
`` -d "C:\CLI Output Logs" ` ``\
`` -g "8C4D08A1-C0ED-4FFC-A695-BBED068507E9" ` ``\
`` -v "2.0.0" ` ``\
`` -o "C:\CinchyDXD_Output" ` ``\


6\. Enter the export parameters into the PowerShell window _(Image 1)._

![Image 1: Step 6](<../../../.gitbook/assets/image (147).png>)

7\. Hit Enter on your keyboard to run the export command

PowerShell will begin to process the export. Once the export is complete, PowerShell will provide you with an export complete message _(Image 2)._

![Image 2: Step 7](<../../../.gitbook/assets/image (381).png>)

## 4. Validate the Export

1. Ensure that the **DXD Export Folder** is populated _(Image 3)._

![Image 3: Step 1](<../../../.gitbook/assets/image (55).png>)

2\. Ensure that the **Data Experience Release** table is populated in the source environment _(Image 4)._

![Image 4: Step 2](<../../../.gitbook/assets/image (217).png>)

3\. Ensure that the **Data Experience Release Artifacts** table is populated in the source environment _(Image 5)._

![Image 5: Step 3](<../../../.gitbook/assets/image (328).png>)
