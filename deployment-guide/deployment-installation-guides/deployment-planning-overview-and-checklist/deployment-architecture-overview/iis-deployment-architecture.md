---
description: >-
  This page details the deployment architecture of Cinchy v5 when running on a
  VM.
---

# IIS deployment architecture

## Component diagram

The below diagram shows a high level overview of Cinchy's Infrastructure components when deploying on IIS.

{% hint style="info" %}
Some components and configurations are dependent on the platform usage. The table below provides a description of each component.
{% endhint %}

{% hint style="success" %}
Tip: Click on an image to enlarge it.
{% endhint %}

![](<../../../../.gitbook/assets/IIS Deployment Architecture Guide.png>)

## Component overview

| Component              | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Technology Stack       | Dependencies                                              |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|-----------------------------------------------------------|
| Cinchy Web Application | This is the primary application for Cinchy, providing both the UI for end users as well as the REST APIs that serve application integration needs. The back-end holds the engine that powers Cinchy's data / metadata management functionality.                                                                                                                                                                                                                                                                                                                                                                         | ASP.NET MVC 5          | .NET Framework 4.7.2+IIS 7.5+Windows Server 2012 or later |
| Cinchy IdP             | This is an OpenID Connect / OAuth 2.0 based Identity Provider that comes with Cinchy for authenticating users. Cinchy supports user group management directly on the platform, but can also connect into an existing IdP available in the organization if it can issue SAML tokens. Optionally, Active Directory groups may be integrated into the platform. When SSO is turned on, this component federates authentication to the customer's SAML enabled IdP. This centralized IdP issues tokens to all integrated applications including the Cinchy web app as well as any components accessing the REST based APIs. | .Net Core 2.1          | .NET Framework 4.7.2+IIS 7.5+Windows Server 2012 or later |
| Cinchy Database        | All data managed on Cinchy is stored in a MS SQL Server database. This is the persistence layer.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | MS SQL Server Database | Windows Server 2012 or laterMS SQL Server 2012 or later   |
| Cinchy CLI             | The CLI offers utilities to get data in and out of Cinchy. It has tools to sync data from a source into a table in Cinchy. it can operate on large datasets with built-in partitioning capability and performs a reconciliation to determine differences before applying changes. Another utility is the data export, which invokes a query against the Cinchy platform and dumps the results to a file for distribution to other systems requiring batch data feeds.                                                                                                                                                   | .NET Core 2.0          | .NET Core Runtime 2.0.7+ (on Windows or Linux)            |
| ADO.NET Driver         | For .NET applications Cinchy provides an ADO.NET driver that can be used to connect into the platform and perform CRUD operations on data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | .NET Standard 2.0      | See implementation support table here                     |
| JavaScript SDK      | Cinchy's JavaScript SDK for front-end developers looking to create an application that can integrate with the Cinchy platform to act as it's middle-tier and backend.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | JavascriptJQuery       |
| Angular SDK         | Cinchy's Angular SDK for front-end developers looking to create an application that can integrate with the Cinchy platform to act as it's middle-tier and backend.                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Angular 5              |


