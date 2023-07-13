# Supported data sync sources

### Cinchy Event Broker

{% content-ref url="cinchy-event-broker-cdc/" %}
[cinchy-event-broker-cdc](cinchy-event-broker-cdc/)
{% endcontent-ref %}

### Cinchy Table

{% content-ref url="cinchy-table/" %}
[cinchy-table](cinchy-table/)
{% endcontent-ref %}

### Cincy Query

{% content-ref url="cinchy-query/" %}
[cinchy-query](cinchy-query/)
{% endcontent-ref %}

### Binary file

A binary file is a computer file that is **not a text file**, and whose content is in a binary format consisting of a series of sequential bytes, each of which is eight bits in length.

You can use binary files from a **Local upload, Amazon S3, or Azure Blob Storage** in your data syncs.

Some benefits of using binary files include:

* **Better efficiency via compression**
* **Better Security** through the ability to create custom encoding standards.
* **Unmatched Speed,** since the data is stored in a raw format, and is not encoded using any character encoding standards, it is faster to read and store.

{% content-ref url="file-based-sources/binary-file.md" %}
[binary-file.md](file-based-sources/binary-file.md)
{% endcontent-ref %}

### Copper

[Copper](https://www.copper.com/) is a **Customer Relationship Management (CRM)** software. Copper is a tool focused on automation and simplicity, most known for its Google Workspace integration.

{% content-ref url="copper.md" %}
[copper.md](copper.md)
{% endcontent-ref %}

### Delimited file

{% content-ref url="file-based-sources/delimited-file.md" %}
[delimited-file.md](file-based-sources/delimited-file.md)
{% endcontent-ref %}

### Dynamics 365

[Microsoft Dynamics 365 ](https://dynamics.microsoft.com/en-us/)functions as an interconnected CRM, ERP, and productivity suite that integrates processes, data, and business logic.

{% content-ref url="dynamics.md" %}
[dynamics.md](dynamics.md)
{% endcontent-ref %}

### Dynamics 2015

Dynamics 2015 is a legacy CRM predecessor to Microsoft Dynamics 365. Mainstream end of life support finished in [January 2020](https://learn.microsoft.com/en-us/lifecycle/products/dynamics-crm-2015), with extended end of life support finishing in January 2025.

{% content-ref url="dynamics-2015.md" %}
[dynamics-2015.md](dynamics-2015.md)
{% endcontent-ref %}

### DynamoDB

[Amazon DynamoDB](https://aws.amazon.com/dynamodb/?trk=d1003b1b-ffc2-4fbd-9ce6-e70c668663bc\&sc\_channel=ps\&s\_kwcid=AL!4422!3!536393505298!e!!g!!dynamodb\&ef\_id=Cj0KCQjwteOaBhDuARIsADBqRehoQ4LyBjuhkAYGKfx15DT4NXjMrNVjbVFUYbYb\_5uQOrcctpV9A-8aAihsEALw\_wcB:G:s\&s\_kwcid=AL!4422!3!536393505298!e!!g!!dynamodb) is a managed NoSQL database service that's offered by Amazon as part of the AWS portfolio.

{% content-ref url="dynamodb.md" %}
[dynamodb.md](dynamodb.md)
{% endcontent-ref %}

### Excel

{% content-ref url="file-based-sources/excel.md" %}
[excel.md](file-based-sources/excel.md)
{% endcontent-ref %}

### Fixed-width file

A **fixed-width file** is a file that has a specific format which allows for the saving of information in an organized fashion. The data is arranged in rows and columns, with one entry per row. Each column has a fixed-width, specified in characters, which determines the maximum amount of data it can contain. No delimiters are used to separate the fields in the file.

Advantages of using a fixed-width file include:

* It is a very **compact** representation of your data
* It is fast to parse because every field is in the same place in every line

{% content-ref url="file-based-sources/fixed-width-file.md" %}
[fixed-width-file.md](file-based-sources/fixed-width-file.md)
{% endcontent-ref %}

### Kafka Topic

[Apache Kafka ](https://kafka.apache.org/intro)is an end-to-end **event streaming platform** that:

* **Publishes** (writes) and **subscribes to** (reads) streams of events from sources like databases, cloud services, and software applications.
* **Stores** these events durably and reliably for as long as you want.
* **Processes and reacts** to the event streams in real-time and retrospectively.

Those events are organized and durably stored in **topics.** These topics are then partitioned over a number of buckets located on different Kafka brokers.

Event streaming thus ensures a continuous flow and interpretation of data so that the right information is at the right place, at the right time [for your key use cases.](https://kafka.apache.org/powered-by)

{% content-ref url="kafka-topic/" %}
[kafka-topic](kafka-topic/)
{% endcontent-ref %}

### LDAP

[LDAP](https://www.techtarget.com/searchmobilecomputing/definition/LDAP) **(Lightweight Directory Access Protocol)** is a mature, flexible, and well supported standards-based mechanism software protocol for enabling anyone to locate data whether on the public internet or on a corporate intranet.

Common uses of LDAP include when:

* A single piece of data needs to be found and accessed regularly;
* Your organization has a lot of smaller data entries;
* Your organization wants all smaller pieces of data in one centralized location, and there doesn't need to be an extreme amount of organization between the data.

{% content-ref url="ldap.md" %}
[ldap.md](ldap.md)
{% endcontent-ref %}

### MongoDB collection

[MongoDB](https://www.mongodb.com/what-is-mongodb/features) is a scalable, flexible NoSQL document database platform known for its horizontal scaling and load balancing capabilities, which has given application developers an unprecedented level of flexibility and scalability.

{% content-ref url="mongodb-collection/" %}
[mongodb-collection](mongodb-collection/)
{% endcontent-ref %}

### MongoDB collection (CDC triggered)

[MongoDB](https://www.mongodb.com/what-is-mongodb/features) is a scalable, flexible NoSQL document database platform known for its horizontal scaling and load balancing capabilities, which has given application developers an unprecedented level of flexibility and scalability. Data changes in Cinchy (CDC) can be used to trigger a data sync from a MongoDB data source to a specified target. The attributes of the CDC Event are available to use as parameters within the  Data Source Definition to narrow the scope of the request. For example, a lookup.

{% content-ref url="mongodb-collection-cinchy-event-triggered.md" %}
[mongodb-collection-cinchy-event-triggered.md](mongodb-collection-cinchy-event-triggered.md)
{% endcontent-ref %}

### MS SQL Server (query and table)

{% content-ref url="ms-sql-server-query-and-table.md" %}
[ms-sql-server-query-and-table.md](ms-sql-server-query-and-table.md)
{% endcontent-ref %}

### ODBC query

Open Database Connectivity ([ODBC](https://learn.microsoft.com/en-us/sql/odbc/reference/what-is-odbc?view=sql-server-ver16)) is a standard API for accessing database management systems (DBMS).

ODBC is the database portion of the Microsoft Windows Open Services Architecture (WOSA), which is an interface that allows Windows-based desktop applications to connect to multiple computing environments without rewriting the application for each platform.

{% content-ref url="dynamodb.md" %}
[dynamodb.md](dynamodb.md)
{% endcontent-ref %}

### Oracle (query and table)

[Oracle Database](https://docs.oracle.com/cd/B13789\_01/server.101/b10743/intro.htm) is a relational database management system, commonly used for running online transaction processing, data warehousing and mixed database workloads. The system is built around a relational database framework in which data objects may be directly accessed by users (or an application front end) through structured query language (SQL).

{% content-ref url="oracle-query-and-table.md" %}
[oracle-query-and-table.md](oracle-query-and-table.md)
{% endcontent-ref %}

### Parquet

[Apache Parquet](https://parquet.apache.org/) is an open source data file format built to handle flat columnar storage data formats. Parquet operates well with complex data in large volumes and is known for its both performant data compression and its ability to handle a wide variety of encoding types.

{% content-ref url="file-based-sources/parquet.md" %}
[parquet.md](file-based-sources/parquet.md)
{% endcontent-ref %}

### Polling event

{% content-ref url="polling-event/" %}
[polling-event](polling-event/)
{% endcontent-ref %}

### REST API

A REST API is an application programming interface that conforms to the constraints of REST (representational state transfer) architectural style and allows for interaction with RESTful web services.

REST APIs work by fielding **requests for a resource** and **returning all relevant information** about the resource, translated into a format that clients can easily interpret (this format is determined by the API receiving requests). Clients can also **modify** items on the server and even **add new items** to the server through a REST API.

{% content-ref url="rest-api.md" %}
[rest-api.md](rest-api.md)
{% endcontent-ref %}

### Salesforce object (bulk API)

[Salesforce](https://www.salesforce.com/ca/products/what-is-salesforce/) is a cloud-based CRM software designed for service, marketing, and sales.

Salesforce objects are database tables that permit you to store data that's specific to an organization. Salesforce objects are of two types:

* Standard Objects: Standard objects are the kind of objects that are provided by salesforce.com such as users, contracts, reports, dashboards, etc.
* Custom Objects: Custom objects are those objects that are created by users. They supply information that's unique and essential to their organization. They are the heart of any application and provide a structure for sharing data.

{% content-ref url="salesforce-object-bulk-api.md" %}
[salesforce-object-bulk-api.md](salesforce-object-bulk-api.md)
{% endcontent-ref %}

### Salesforce platform event

[Salesforce](https://www.salesforce.com/ca/products/what-is-salesforce/) is a cloud-based CRM software designed for service, marketing, and sales.

Salesforce Platform Events are secure and scalable messages that contain data. Publishers push out event messages that subscribers receive in real time.

{% content-ref url="salesforce-platform-event.md" %}
[salesforce-platform-event.md](salesforce-platform-event.md)
{% endcontent-ref %}

### Salesforce push topic

[Salesforce](https://www.salesforce.com/ca/products/what-is-salesforce/) is a cloud-based CRM software designed for service, marketing, and sales.

Push Topic events provide a secure and scalable way to receive notifications for changes to Salesforce data that match a SOQL query you define.

You can use PushTopic events to:

* Receive notifications of Salesforce record changes, including create, update, delete, and undelete operations.
* Capture changes for the fields and records that match a SOQL query.
* Receive change notifications for only the records a user has access to based on sharing rules.
* Limit the stream of events to only those events that match a subscription filter.

{% content-ref url="salesforce-push-topic.md" %}
[salesforce-push-topic.md](salesforce-push-topic.md)
{% endcontent-ref %}

### Snowflake

[Snowflake ](https://www.snowflake.com/en/)is a fully managed SaaS that provides a single platform for data warehousing, data lakes, data engineering, data science, data application development, and secure sharing and consumption of real-time/shared data.

Snowflake enables data storage, processing, and analytic solutions.

{% content-ref url="snowflake/" %}
[snowflake](snowflake/)
{% endcontent-ref %}

### SOAP 1.2 web service

SOAP (Simple Object Access Protocol) is an XML-based protocol for accessing web services over HTTP.

SOAP can communicate between different operating systems using different technologies and programming languages. You can use SOAP APIs to create, retrieve, update or delete records, such as passwords, accounts, leads, and custom objects, from a server.

{% content-ref url="soap-1.2-web-service.md" %}
[soap-1.2-web-service.md](soap-1.2-web-service.md)
{% endcontent-ref %}

### SAP SuccessFactors

[SAP SuccessFactors ](https://www.sap.com/products/hcm.html)solutions are cloud-based HCM software applications that support core HR and payroll, talent management, HR analytics and workforce planning, and employee experience management.

{% content-ref url="sap-successfactors.md" %}
[sap-successfactors.md](sap-successfactors.md)
{% endcontent-ref %}
