# SOAP 1.2 Web Service

## 1. Overview

SOAP (Simple Object Access Protocol) is an XML-based protocol for accessing web services over HTTP.

SOAP allows applications running on different operating systems to communicate using different technologies and programming languages. You can use SOAP APIs to create, retrieve, update or delete records, such as passwords, accounts, leads, and custom objects, from a server.

{% hint style="success" %}
The SOAP 1.2 Web Service source supports batch syncs.
{% endhint %}

## 2. Info Tab

You can review the parameters that can be found in the info tab below _(Image 1)._

#### Values

| Parameter   | Description                                                                                                                                                                                      | Example   |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------- |
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                   | SOAP Sync |
| Variables   | **Optional.** Review our documentation on [Variables here ](../building-data-syncs/advanced-settings/variables.md)for more information about this field.                                         |           |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. **Inputting at least an Admin Group is mandatory.** |           |

<figure><img src="../../.gitbook/assets/image (166).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## 3. Source Tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>SOAP 1.2 Web Service</td></tr><tr><td>authType</td><td><strong>Mandatory.</strong> Select the type of authentication you wish to us in this sync.<br><strong>- None</strong><br><strong>- WSSE:</strong> This will allow you to use a Username and Password to authenticate via a WS-Security SOAP envelope header.<br><strong>- Basic:</strong> This will allow you to use a Username and Password to authenticate via a basic auth header.</td><td>Basic</td></tr><tr><td>Use Password Digest</td><td>The password digest is a cryptographic hash of the password and timestamp. <strong>This parameter should only be used in conjunction with a WSSE authType, and when the Password Type for your auth is "PasswordDigest".</strong> If neither of those applies, leave this value unchecked.</td><td></td></tr><tr><td>Request Timeout</td><td><strong>Mandatory.</strong> You can use this field to set a timeout, in milliseconds, for your request. There is no maximum value. The minimum should be greater than 0. The default value is 100 milliseconds</td><td>2000</td></tr><tr><td>Endpoint</td><td><strong>Mandatory.</strong> This field should contain your SOAP 1.2 Web Service API endpoint.</td><td><a href="https://www.dataaccess.com/webservicesserver/NumberConversion.wso">dataaccess</a></td></tr><tr><td>Has Mtom Response</td><td><strong>This value is required to be true if:</strong> the SOAP API response contains an attachment outside of the response message. <a href="https://images.app.goo.gl/E82L6mYrJxCxXwhKA">See here for an example.</a></td><td></td></tr><tr><td>Record Xpath</td><td><strong>Mandatory.</strong> The Xpath to select all records we want to extract from the SOAP response. The path should point to the XML element wrapping the column data.<br><br>XPath stands for XML Path Language. It uses a non-XML syntax to provide a flexible way of addressing (pointing to) different parts of an XML document. This value should start with ‘//’ and be followed by the tag name of the data. <br><br>You can refer http://xpather.com/ to find out the Xpath.</td><td></td></tr><tr><td>Envelope Namespace</td><td><p>The namespace prefix to use for the SOAP request elements.<br><br>For example, setting the value to "foo" would result in the soap request being prefixed with the "foo" namespace. </p><p></p><p></p><pre><code>&#x3C;foo:Envelope xmlns:foo="...">
	&#x3C;foo:Body>
		[Request XML]
	&#x3C;/foo:Body>
&#x3C;/foo:Envelope>
</code></pre></td><td>"foo"</td></tr><tr><td>Namespaces - Name</td><td><p>The name of your SOAP namespace tags in your request and response. This value appear as "soap" in the snippet below.</p><p></p><p>These should be the values immediately after "xmlns:"<br></p><pre><code>&#x3C;?xml version="1.0" encoding="utf-8"?>
&#x3C;soapenv:Envelope
	xmlns:<a data-footnote-ref href="#user-content-fn-1">soap</a>="http://schemas.xmlsoap.org/soap/envelope/">
	&#x3C;soap:Body>
		&#x3C;m:NumberToWordsResponse
			xmlns:m="http://www.dataaccess.com/webservicesserver/">
			&#x3C;m:NumberToWordsResult>four million four hundred and seventy three thousand two hundred and thirty nine &#x3C;/m:NumberToWordsResult>
		&#x3C;/m:NumberToWordsResponse>
	&#x3C;/soap:Body>
&#x3C;/soap:Envelope>
</code></pre></td><td>"soap"</td></tr><tr><td>Namespaces - Value</td><td><p>The URL describing this namespace in the response. In the below snippet this value is "<a href="http://www.dataaccess.com/webservicesserver/">http://www.dataaccess.com/webservicesserver/</a>"<br></p><pre><code>&#x3C;?xml version="1.0" encoding="utf-8"?>
&#x3C;soapenv:Envelope
	xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
	&#x3C;soap:Body>
		&#x3C;m:NumberToWordsResponse
			xmlns:m=<a data-footnote-ref href="#user-content-fn-2">"http://www.dataaccess.com/webservicesserver/"</a>>
			&#x3C;m:NumberToWordsResult>four million four hundred and seventy three thousand two hundred and thirty nine &#x3C;/m:NumberToWordsResult>
		&#x3C;/m:NumberToWordsResponse>
	&#x3C;/soap:Body>
&#x3C;/soapenv:Envelope>

</code></pre></td><td>"<a href="http://www.dataaccess.com/webservicesserver/">http://www.dataaccess.com/webservicesserver/</a>"</td></tr></tbody></table>


{% endtab %}

{% tab title="Schema" %}
**The** [**Schema**](../building-data-syncs/columns-and-mappings/#2.-schema-columns) **section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

| Parameter   | Description                                                                                                   | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------- | ------- |
| Name        | **Mandatory.** The name of your column as it appears in the source.                                           | Name    |
| Alias       | **Optional.** You may choose to use an alias on your column so that it has a different name in the data sync. |         |
| Data Type   | **Mandatory.** The data type of the column values.                                                            | Text    |
| Description | **Optional.** You may choose to add a description to your column.                                             |         |



There are other options available for the Schema section if you click on **Show Advanced.**

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Example |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Mandatory       | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Mandatory is checked</strong> on a column, then all rows are synced with the execution log status of failed, and the source error of <strong>"Mandatory Rule Violation"</strong></li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul> |         |
| Validate Data   | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul>                                                                                                                                                                                                                   |         |
| Trim Whitespace | **Optional if data type = text.**  If your data type was chosen as "text", you can choose whether to **trim the whitespace**._                                                                                                                                                                                                                                                                                                   |         |
| Max Length      | **Optional if data type = text.** You can input a numerical value in this field that represents the maximum length of the data that can be synced in your column. If the value is exceeded, the row will be rejected (you can find this error in the Execution Log).                                                                                                                                                                                                                  |         |

You can choose to add in a **Transformation > String Replacement** by inputting the following:

| Parameter   | Description                                                                                                                           | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Pattern     | **Mandatory if using a Transformation.** The pattern for your string replacement, i.e. the string that will be searched and replaced. |         |
| Replacement | What you want to replace your pattern with.                                                                                           |         |

{% hint style="info" %}
Note that you can have more than one String Replacement
{% endhint %}
{% endtab %}
{% endtabs %}



<figure><img src="../../.gitbook/assets/image (339).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

## 4. Next Steps

* Configure your [Destination](../supported-data-sync-destinations/)
* Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Click **Jobs > Start a Job** to begin your sync.

[^1]: Namespace tag

[^2]: Namespace Value
