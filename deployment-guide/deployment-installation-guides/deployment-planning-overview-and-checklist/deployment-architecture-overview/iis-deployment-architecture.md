---
description: >-
  This page details the deployment architecture of Cinchy v5 when running on a
  VM.
---

# IIS Deployment Architecture

## Table of Contents

| Table of Contents                                                                        |
| ---------------------------------------------------------------------------------------- |
| [#1.-component-diagram](iis-deployment-architecture.md#1.-component-diagram "mention")   |
| [#2.-component-overview](iis-deployment-architecture.md#2.-component-overview "mention") |

## 1. Component Diagram

The below diagram shows a high level overview of Cinchy's Infrastructure components when deploying on IIS.

{% hint style="info" %}
Certain components and configurations are optional and dependent upon the usage pattern of the platform, these will be called out in the table below the diagram which provides a description of each component.
{% endhint %}

{% hint style="success" %}
Tip: Click on an image to enlarge it.
{% endhint %}

![](<../../../../.gitbook/assets/IIS Deployment Architecture Guide.png>)

## **2. Component Overview**

<table data-header-hidden><thead><tr><th>Component</th><th>Description</th><th width="162">Technology Stack</th><th>Dependencies</th></tr></thead><tbody><tr><td><ol><li>Cinchy Web Application</li></ol></td><td>This is the primary application for Cinchy, providing both the UI for end users as well as the REST APIs that serve application integration needs. The back-end holds the engine that powers Cinchy's data / metadata management functionality.</td><td>ASP.NET MVC 5</td><td><ul><li>.NET Framework 4.7.2+</li><li>IIS 7.5+</li><li>Windows Server 2012 or later</li></ul></td></tr><tr><td>2. Cinchy IdP</td><td><p>This is an OpenID Connect / OAuth 2.0 based Identity Provider that comes with Cinchy for authenticating users.</p><p>Cinchy supports user &#x26; group management directly on the platform, but can also connect into an existing IdP available in the organization if it can issue SAML tokens. Optionally, Active Directory groups may be integrated into the platform.</p><p>When SSO is turned on, this component is responsible for federating authentication to the customer's SAML enabled IdP. This centralized IdP issues tokens to all integrated applications including the Cinchy web app as well as any components accessing the REST based APIs.</p></td><td>.Net Core 2.1</td><td><ul><li>.NET Framework 4.7.2+</li><li>IIS 7.5+</li><li>Windows Server 2012 or later</li></ul></td></tr><tr><td>3. Cinchy Database</td><td>All data managed on Cinchy is stored in a MS SQL Server database. This is the persistence layer</td><td>MS SQL Server Database</td><td><ul><li>Windows Server 2012 or later</li><li>MS SQL Server 2012 or later</li></ul></td></tr><tr><td>4. Cinchy CLI</td><td><p>This is Cinchy's Command Line Interface that offers utilities to get data in and out of Cinchy.</p><p>One of these utilities is a tool to sync data from a source into a table in Cinchy. This is able to operate on large datasets by leveraging an in-built partitioning capability and performs a reconciliation to determine differences before applying changes.</p><p>Another commonly used utility is the data export, which allows customers to invoke a query against the Cinchy platform and dump the results to a file for distribution to other systems requiring batch data feeds.</p></td><td>.NET Core 2.0</td><td><ul><li>.NET Core Runtime 2.0.7+ (on Windows or Linux)</li></ul></td></tr><tr><td>5. ADO.NET Driver</td><td>For .NET applications Cinchy provides an ADO.NET driver that can be used to connect into the platform and perform CRUD operations on data.</td><td>.NET Standard 2.0</td><td><ul><li>See implementation support table <a href="https://docs.microsoft.com/en-us/dotnet/standard/net-standard">here</a></li></ul></td></tr><tr><td>6. Javascript SDK</td><td>Cinchy's Javascript SDK for front-end developers looking to create an application that can integrate with the Cinchy platform to act as it's middle-tier and backend.</td><td>Javascript<br>JQuery</td><td></td></tr><tr><td>7. Angular SDK</td><td>Cinchy's Angular SDK for front-end developers looking to create an application that can integrate with the Cinchy platform to act as it's middle-tier and backend.</td><td>Angular 5</td><td></td></tr></tbody></table>

