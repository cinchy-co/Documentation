---
description: >-
  This page guides you through the overview, installation, and use of Cinchy's
  MDQE function.
---

# MDQE

## Overview

MDQE, which stands for **Metadata Quality Exceptions**, can send out notifications based on a set of rules and the “exceptions” that break them. This powerful tool can be used to send notifications for exceptions such as:

* Healthchecks returning a critical status
* Upcoming Project Due Dates/Timelines
* Client Risk Ratings reaching a high threshold
* Tracking Ticket Urgency or Status markers
* Unfulfilled and Pending Tasks/Deliverables
* Etc.

In a nutshell, MDQE monitors for specific changes in data, and then pushes out notifications when that change occurs.

## 2. Prerequisites

* You will need to have the [Cinchy CLI installed.](https://cli.docs.cinchy.com/installation-guide)
* Request the installation package from the [Cinchy Support team.](../../getting-help.md)

## 3. Installing MDQE

To install MDQE in your Cinchy environment, follow the below steps:

1. **Download** the MDQE Installation package.
2. **Unzip** the file.
3. Open an instance of Powershell as an Administrator and navigate to the path where you extracted the **MDQE package in step 2 > Metadata Quality Exceptions V.x > Metadata Quality Exceptions.**
4. **Run the following command** to install all MDQE components in your environment, using the table below as a parameter guide.

```
.\CinchyDXD.ps1 install `
-s "url.com/Cinchy" `
-sso "url.com/CinchySSO" `
-u "CinchyDQE" `
-p "cinchy" `
-c "C:\Cinchy CLI\Cinchy CLI v4.12.0.564" `
-d "C:\Cinchy CLI\Cinchy CLI Error Logs" `
-y
```

| Parameter | Description                                                                               |
| --------- | ----------------------------------------------------------------------------------------- |
| **-s**    | The base URL of your Cinchy instance, without the protocol.                               |
| **-sso**  | The base URL of your Cinchy SSO, without the protocol.                                    |
| **-u**    | Username. We recommend creating a new, specific user for this install. Example: CinchyDQE |
| **-p**    | The password for the user designated above.                                               |
| **-c**    | This refers to the path where you have your CLI installed.                                |
| **-d**    | This refers to a temporary path for storing error logs.                                   |
| **-h**    | This flag must be added for environments set up with **https.**                           |

5\. Within the MDQE file package, navigate to the **Powershell - DQE Orchestration** folder.

6\. **Extract** the contents.

7\. Navigate to the **\_config.json file** and update the parameter values using the below as a guide. Make sure to **save** when finished.

| Parameter                | Description                                                                                                                                          |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CinchyServerProtocol** | Defaulted to HTTPS                                                                                                                                   |
| **CinchyServer**         | <p>The base URL of your Cinchy instance. <br><br><em>Example: Cinchy.net</em></p>                                                                    |
| **CinchyServerSSO**      | <p>The base URL of your Cinchy SSO. <br><br><em>Example: Cinchy.net/SSO</em></p>                                                                     |
| **APIClientSecret**      | You can find this value in the **Integrated Clients table > mdqe row > Guid column** in your Cinchy instance.                                        |
| **CinchyCLIPath**        | <p>The path to your CLI. <br><br><em>Example: C:\Cinchy CLI\Cinchy CLI v4.12.0.564</em></p>                                                          |
| **CinchyCLITempPath**    | <p>The path for storing error logs. <br><br><em>Example: C:\Cinchy CLI\Cinchy CLI Error</em> </p>                                                    |
| **MailServer**           | <p>The server that will be sending out your email notifications. <br><br><em>Example: smtp.office365.com.</em></p>                                   |
| **MailPort**             | <p>The port number for your chosen email server. <br><br><em>Example: 25.</em></p>                                                                   |
| **MailFrom**             | <p>The email account that notifications will come from. <br><br><em>Example: MDQEnotifications@outlook.com</em></p>                                  |
| **Mail Subject**         | <p>A subject line for outbound emails. <br><br><em>Example: Data Quality Exception found.</em></p>                                                   |
| **MailUser**             | <p>The username for the email address above. This may be the same as the address itself. <br><br><em>Example: MDQEnotifications@outlook.com</em></p> |
| **MailPswd**             | The password for the email account above.                                                                                                            |

## 4. Creating MDQE Rules

1. In the environment where you installed MDQE, search for and open the **\[Cinchy MDQE].\[Rules] table.**
2. Using the “Create Rule” view and the following data, create your Rule:

| Column                 | Description                                                                                                                                                                                                                                                                                                                            |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Name**               | <p>The name of your rule. This must be unique across the rules.<br></p><p><em>Example: Project Timeline Start Date Exception</em></p>                                                                                                                                                                                                  |
| **Table**              | <p>Table: The table on which the exception scenario needs to be evaluated<br><br><em>Example: Projects</em></p>                                                                                                                                                                                                                        |
| **Table Columns**      | <p>The columns in the table that should be highlighted in the case of an exception<br><br><em>Example: Start Date</em></p>                                                                                                                                                                                                             |
| **Signature**          | <p>The CQL for your exception condition.<br><br><em>Example: [Start Date] is null</em></p>                                                                                                                                                                                                                                             |
| **Description**        | <p>A description of the rule.<br><br><em>Example: This exception will trigger if the start date of a project is left blank.</em></p>                                                                                                                                                                                                   |
| **User Assignment**    | <p>This is the owner of the exception. You will use this when you want to assign the rule to a Cinchy user.<br><br><em>Example: John Smith</em></p>                                                                                                                                                                                    |
| **Severity**           | <p>Choose from the drop down list.<br><br><em>Example: Low</em><br><br><strong>Note : In case you would like to define your own severity, use [Cinchy MDQE].[Severity] table. You would need admin privileges to view this table</strong></p>                                                                                          |
| **Send Notifications** | <p>Choose from the drop down list. Use “Never” if you do not want email notifications sent out.<br><br><em>Example: Daily</em><br><br><strong>Note : In case you would like to define your own Notification frequency, use [Cinchy MDQE].[Notification schedule] table.You would need admin privileges to view this table</strong></p> |

{% hint style="info" %}
Use the “Invalid Rules” view to correct Rules with have syntax errors
{% endhint %}

## 5. Viewing All Exceptions

All exceptions can be viewed in the \[Cinchy MDQE].\[Data Quality Exceptions] table

* The **Default** view only displays exceptions assigned to the currently logged in user.
* The **All Data** view displays all exceptions. This is only visible with admin privileges.

## 6. Debugging

Ways to debug your rules:

* If your Powershell scripts aren't running: Run the script files in the Powershell - DQE Orchestration folder **using an IDE** to make sure that all the configurations are correct.
* Check to see if your bugged Rule is part of the **“Invalid Rules”** view.
* If you have admin privileges, check to see if an equivalent SQL statement has been created in **the \[Rules CQL] table.**
* Check if there is a row for the Rule’s Signature value in the **\[Cinchy].\[Formatting Rules] table.**

## 7. Scheduling MDQE Jobs

You can use the Windows Task Scheduler to run MDQE jobs at regular intervals.

1. Navigate to your **MDQE installation package > Windows Task Scheduler Jobs folder.**
2. Import the files into the Windows Task Scheduler, updating the parameters accordingly.
