---
description: >-
  This page outlines Step 3 of Deploying CinchyDXD: Installing the Data
  Experience
---

# Installing the Data Experience (CinchyDXD)

## Table of Contents

| Table of Contents                                                                                                       |
| ----------------------------------------------------------------------------------------------------------------------- |
| [#1.-introduction](installing-the-data-experience-cinchydxd.md#1.-introduction "mention")                               |
| [#2.-install-the-data-experience](installing-the-data-experience-cinchydxd.md#2.-install-the-data-experience "mention") |
| [#3.-validate-the-install](installing-the-data-experience-cinchydxd.md#3.-validate-the-install "mention")               |

## 1. Introduction

The install of a Data Experience is executed in a different environment than that of the export. Please ensure that before moving forward with the following instructions you have an environment to install the data experience into.The install of a data experience MUST be done in the same version. Your source and target environment version MUST be the same (e.g. Source Version = 4.11 | Target Version = 4.11)

Below are the details that will be required for the installation environment:

- Source: \<cinchy target url>
- UserID: \<target user id>
- Password: \<target password>

## 2. Install the Data Experience

Using PowerShell you will now install the Data Experience you have exported out of Cinchy.

1\. Open the **File Explorer** and navigate to your DX exported folder _(Image 1)._

![Image 1: Step 1](<../../../.gitbook/assets/image (340).png>)

2\. In the folder path URL for the exported data experience type in **PowerShell** to launch PowerShell for that path _(Image 2)._

![Image 2: Step 2](<../../../.gitbook/assets/image (440).png>)

3\. Hit **Enter** on your keyboard, the PowerShell window will appear _(Image 3)._

![Image 3: Step 3](<../../../.gitbook/assets/image (615).png>)

4\. In the PowerShell window, type in **cin** and hit **tab** on your keyboard _(Image 4)._

![Image 4: Step 4](<../../../.gitbook/assets/image (590).png>)

5\. In the PowerShell command line, type **install** _(Image 5)._

![Image 5: Step 5](<../../../.gitbook/assets/image (377).png>)

6\. Hit **Enter** on your keyboard _(Image 6)._

![Image 6: Step 6](<../../../.gitbook/assets/image (720).png>)

The PowerShell window will provide you with the required and optional components to install the DX.

7\. You must now set up your mandatory install parameters

{% hint style="info" %}
The parameters executed in PowerShell can exist on one line in PowerShell, however for legibility (below) the parameters have been put on separate lines. If you are putting your parameters on separate lines you will be required to have backticks quote \` for the parameters to execute
{% endhint %}

Sample:\
`` .\CinchyDXD.ps1 install` ``\
`` -s "<target Cinchy url>" ` ``\
`` -u "<target user id>" ` ``\
`` -p "<target password>" ` ``\
`` -c "C:\Cinchy CLI v4.0.2" ` ``\
`` -d "C:\CLI Output Logs" ` ``

{% hint style="info" %}
Be sure that the user(s) and group(s) required to install a DX are existing in your target environment. If they aren't, PowerShell will generate an error message when you attempt to install the DX.
{% endhint %}

8\. Enter the install parameters into the PowerShell window _(Image 7)._

![Image 7: Step 8](<../../../.gitbook/assets/image (251).png>)

9\. Hit **Enter** on your keyboard to run the install command. Once the Data Experience has been installed you will get a message in PowerShell that the install was completed _(Image 8)._

![Image 8: Step 9](<../../../.gitbook/assets/image (555).png>)

## 3. Validate the Install

1. Ensure that the **Models Table** is populated in the target environment with the model that was installed (_Image 9)._

![Image 9: Step 1](<../../../.gitbook/assets/image (489).png>)

2\. Ensure that the **Currency Exchange Rate** table exist in the target environment _(Image 10)._

![Image 10: step 2](<../../../.gitbook/assets/image (373).png>)

3\. Ensure that the **Currency Converter** query exist in the target environment _(Image 11)._

![Image 11: Step 3](<../../../.gitbook/assets/image (547).png>)

4\. Ensure that the **Data Experience Definitions** table is populated with the DX parameters that were set up in the source environment _(Image 12)._

![Image 12: Step 4](<../../../.gitbook/assets/image (710).png>)

5\. Ensure that the **Data Experience Releases** table in the target environment is populated _(Image 13)._

![Image 13: Step 5](<../../../.gitbook/assets/image (90).png>)
