# SOAP 1.2 web service

## Overview

SOAP (Simple Object Access Protocol) is an XML-based protocol for accessing web services over HTTP.

SOAP allows applications running on different operating systems to communicate using different technologies and programming languages. You can use SOAP APIs to create, retrieve, update or delete records, such as passwords, accounts, leads, and custom objects, from a server.

{% hint style="info" %}
Before you set up your data sync destination, [make sure to configure your Source.](../supported-data-sync-sources/)
{% endhint %}

{% hint style="success" %}
The SOAP 1.2 Web Service destination supports batch and real-time syncs.
{% endhint %}

## Destination tab

The following table outlines the mandatory and optional parameters you will find on the Destination tab _(Image 1)._

{% tabs %}
{% tab title="Destination Details" %}
The following parameters will help to define your data sync destination and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Destination</td><td><strong>Mandatory.</strong> Select your destination from the drop down menu.</td><td>SOAP 1.2 Web Service</td></tr></tbody></table>
{% endtab %}

{% tab title="Column Mapping" %}
**The** [**Column Mapping** ](../building-data-syncs/columns-and-mappings/#3.-column-mappings)**section** is where you define which source columns you want to sync to which destination columns. You can repeat the values for multiple columns.\
\
When specifying the Target Column in the Column Mappings section, **all names are case-sensitive.**

| Parameter     | Description                                                              | Example |
| ------------- | ------------------------------------------------------------------------ | ------- |
| Source Column | **Mandatory.** The name of your column as it appears in the source.      | Name    |
| Target Column | **Mandatory.** The name of your column as it appears in the destination. | Name    |
{% endtab %}

{% tab title="API Specification" %}
The API Specification section will default with a mandatory Insert Specification field, however you are also able to add fields for Request Headers, SOAP Body, and Variables to Extract.\
\
**Insert Specification**\
When specifying the Target Column in the Column Mappings section, **all names are case-sensitive.**
<!-- markdown-link-check-disable -->
| Parameter                                                                                              | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Example                                                                                                                                |
| ------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| Endpoint URL                                                                                           | **Mandatory.** The URL of the [SOAP 1.2 Web Service API endpoint](https://www.ibm.com/docs/en/wsr-and-r/8.5.6?topic=mswsd-retrieving-addresses-from-soap-11-soap-12-endpoints)                                                                                                                                                                                                                                                                                                                      | [https://www.dataaccess.com/webservicesserver/NumberConversion.wso](https://www.dataaccess.com/webservicesserver/NumberConversion.wso) |
| Has [MTOM Response](https://www.ibm.com/docs/en/integration-bus/10.0?topic=services-what-is-soap-mtom) | This is required to be true if the SOAP API response contains an attachment outside of the SOAP response message. [See this diagram for more information.](https://images.app.goo.gl/E82L6mYrJxCxXwhKA)                                                                                                                                                                                                                                                                                             |                                                                                                                                        |
| Envelope Namespace                                                                                     | <p>The namespace prefix to use for the SOAP request elements.<br><br>This value will default to <code>soapenv</code> as associated with the following schema: <a href="https://schemas.xmlsoap.org/soap/envelope/">https://schemas.xmlsoap.org/soap/envelope/</a><br><br>You can append the default value, if you wish. For example, setting the value to "foo" would result in the soap request being prefixed with the "foo" namespace.</p><pre><code>foo:Envelope xmlns:foo="...">
</code></pre> |                                                                                                                                        |

```
foo:Body>
	[Request XML]
/foo:Body>
```

/foo:Envelope | soapenv | | Namespace - Name |

The name of your SOAP namespace tags in your request and response.\
\
By default, the Connections UI will populate this field with `soapenv`, but you can delete this value or add additional values, as needed.\
\
This value appears as "soap" in the snippet below.

These should be the values immediately after "xmlns:"\


<pre><code>?xml version="1.0" encoding="utf-8"?>
soapenv:Envelope
xmlns:<a data-footnote-ref href="#user-content-fn-1">soap</a>="http://schemas.xmlsoap.org/soap/envelope/">
soap:Body>
m:NumberToWordsResponse
xmlns:m="http://www.dataaccess.com/webservicesserver/">
m:NumberToWordsResult>four million four hundred and seventy three thousand two hundred and thirty nine /m:NumberToWordsResult>
/m:NumberToWordsResponse>
/soap:Body>
/soap:Envelope>
</code></pre>

\| soap | | Namespaces - Value |

The URL describing this namespace in the response.\
\
By default, the Connections UI will populate this field with [http://schemas.xmlsoap.org/soap/envelope/](http://schemas.xmlsoap.org/soap/envelope/), however you can delete this value or add additional values, as needed.\
\
In the below snippet this value is "[http://www.dataaccess.com/webservicesserver/](http://www.dataaccess.com/webservicesserver/)"\


```
?xml version="1.0" encoding="utf-8"?>
soapenv:Envelope
xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
soap:Body>
m:NumberToWordsResponse
xmlns:m="http://www.dataaccess.com/webservicesserver/">
m:NumberToWordsResult>four million four hundred and seventy three thousand two hundred and thirty nine /m:NumberToWordsResult>
/m:NumberToWordsResponse>
/soap:Body>
/soapenv:Envelope>
```

\| "[http://www.dataaccess.com/webservicesserver/](http://www.dataaccess.com/webservicesserver/)" |

**Request Header**

You can add in Request Headers by [reviewing the documentation here.](../building-data-syncs/advanced-settings/request-headers.md)

**SOAP Body**

| Parameter | Description                                                                                                                                                                                             | Example                                                                                      |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| XML       | <p>The SOAP body is a sub-element of the SOAP envelope, which contains information intended for the ultimate recipient of the message.<br><br>This field is expecting you to specify the SOAP Body.</p> | <pre><code>NumberToWords xmlns="http://www.dataaccess.com/webservicesserver/">
</code></pre> |

```
ubiNum>500/ubiNum>
```

/NumberToWords> |

**Variables to Extract**

You may choose to specify variables to extract from your SOAP response.

| Parameter        | Description                                   | Example                                                                         |
| ---------------- | --------------------------------------------- | ------------------------------------------------------------------------------- |
| Name             | The name of the variable you wish to extract. | Value                                                                           |
| Path in Response | The path to the above variable.               | soapenv:Envelope/soapenv:Body/m:NumberToWordsResponse/m:NumberToWordsResult\[1] |

**SOAP 1.2 Source**

This section should be used if you have a set of data from a SOAP API that you need to reconcile against; **therefore it should always be used when doing Full-File syncs.** You can follow [the values outlined on this page](../supported-data-sync-sources/soap-1.2-web-service.md) to set up this section.
{% endtab %}

{% tab title="Filter" %}
You have the option to add a destination filter to your data sync. Please review the documentation here for more information on [destination filters.](../building-data-syncs/advanced-settings/filters.md#target-filters)
{% endtab %}
{% endtabs %}

<figure><img src="../../.gitbook/assets/image (108).png" alt=""><figcaption><p>Image 2: Define your Destination</p></figcaption></figure>
<!-- markdown-link-check-enable -->
## Next steps

* Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md) Note that if you are doing a Full-File sync, the **API Specification > SOAP 1.2 Source section should be filled in.**
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
* If you are running a real-time sync, [set up your Listener Config](../supported-real-time-sources/) and enable it to begin your sync.
* If you are running a batch sync, click **Jobs > Start a Job** to begin your sync.

1. Namespace Value

[^1]: Namespace tag
