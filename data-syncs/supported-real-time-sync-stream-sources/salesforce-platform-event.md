# Salesforce Platform Event

## 1. Overview

[A Salesforce Platform Event ](https://developer.salesforce.com/docs/atlas.en-us.242.0.object\_reference.meta/object\_reference/sforce\_api\_objects\_pushtopic.htm)is a supported Sync Source that you can use in your Cinchy data syncs. The below documentation will refer to the parameters necessary to set up your Platform Event as part of your sync configuration.

## 2. The Listener Config Table

To set up an Stream Source, you must navigate to the Listener Config table and insert a new row for your data sync _(Image 1)_. Most of the columns within the Listener Config table persist across all Stream Sources, however exceptions will be noted. You can find all of these parameters and their relevant descriptions in the tables below.

<figure><img src="../../.gitbook/assets/image (566).png" alt=""><figcaption><p>Image 1: The Listener Config table</p></figcaption></figure>

{% tabs %}
{% tab title="General Parameters" %}
The following column parameters can be found in the Listener Config table:

<table><thead><tr><th width="201">Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Name</td><td><strong>Mandatory.</strong> Provide a name for your Listener Config.</td><td>SF Platform Event Real-Time Sync</td></tr><tr><td><strong>Event Connector Type</strong></td><td><strong>Mandatory.</strong> Select your Connector type from the drop down menu.</td><td>Salesforce Platform Event</td></tr><tr><td><strong>Topic</strong></td><td><strong>Mandatory.</strong> This field is expecting a JSON formatted value specific to the connector type you are configuring.</td><td>See the <strong>Topic</strong> tab.</td></tr><tr><td><strong>Connection Attributes</strong></td><td><strong>Mandatory.</strong> This field is expecting a JSON formatted value specific to the connector type you are configuring.</td><td>See the <strong>Connection Attributes</strong> tab.</td></tr><tr><td><strong>Status</strong></td><td><strong>Mandatory.</strong> This value refers to whether your listener config/real-time sync is turned on or off. Make sure you keep this set to Disabled until you are confident you have the rest of your data sync properly configured first.</td><td>Disabled</td></tr><tr><td><strong>Data Sync Config</strong></td><td><strong>Mandatory.</strong> This drop down will list all of the data syncs on your platform. Select the one that you want to use for your real-time sync.</td><td>Salesforce Data Sync</td></tr><tr><td><strong>Subscription Expires On</strong></td><td><strong>This value is only relevant for Salesforce Stream Sources.</strong> This field is a timestamp that is auto-populated when it has successfully subscribed to a topic. </td><td></td></tr><tr><td><strong>Message</strong></td><td><strong>Leave this value blank when setting up your configuration.</strong> This field will auto-populate during the running of your sync with any relevant messages. For instance "Cinchy listener is running", or "Listener is disabled". </td><td></td></tr><tr><td><strong>Auto Offset Reset</strong></td><td><p><strong>Earliest, Latest or None.</strong> <br><br>In the case where the listener is started and either there is no last message ID, or when the last message ID is invalid (due to it being deleted or it's just a new listener), it will use this column as a fallback to determine where to start reading events from.<br></p><p><strong>Earliest</strong> will start reading from the beginning on the queue (when the CDC was enabled on the table). This might be a suggested configuration if your use case is recoverable or re-runnable and if you need to reprocess all events to ensure accuracy.<br><br><strong>Latest</strong> will fetch the last value after whatever was last processed. This is the typical configuration.<br><br><strong>None</strong> will not read start reading any events.<br><br>You are able to switch between Auto Offset Reset types after your initial configuration through the below steps:<br>1. Navigate to the <strong>Listener Config table.</strong><br>2. Re-configure the <strong>Auto Offset Reset value.</strong><br>3. Set the <strong>"Status"</strong> column of the Listener Config to <strong>"Disabled".</strong><br>4. Navigate to the <strong>Event Listener State table.</strong><br>5. Find the column that pertains to your data sync's Listener Config and <strong>delete it.</strong><br>6. Navigate back to the <strong>Listener Config table.</strong><br>7. Set the <strong>"Status"</strong> column of the Listener Config to <strong>"Enabled"</strong> in order for your new Auto Offset Reset configuration to take effect.</p></td><td>Latest</td></tr></tbody></table>
{% endtab %}

{% tab title="Topic" %}
The below table can be used to help create your Topic JSON needed to set up a real-time sync.

<table><thead><tr><th width="264.66666666666663">Parameter</th><th width="272">Description</th><th>Example</th></tr></thead><tbody><tr><td>Name</td><td><strong>Mandatory.</strong> The name of the Platform Event, as it appears in Salesforce, that you want to subscribe to.</td><td>Notification__e</td></tr></tbody></table>

**Example Topic JSON**

```
{  "Name": "Notification__e"
```
{% endtab %}

{% tab title="Connection Attributes" %}
The below table can be used to help create your Connection Attributes JSON needed to set up a real-time sync.

| Parameter       | Description                                                                                                                                  | Example                                                                                                  |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| ApiVersion      | **Mandatory.** Your Salesforce API Version. Note that this needs to be an exact match; for instance "47.0" cannot be written as simply "47". | 47.0                                                                                                     |
| GrantType       | This value should be set to "password".                                                                                                      | password                                                                                                 |
| ClientId        | The encrypted Salesforce Client ID. You can encrypt this value using the Cinchy CLI.                                                         | Bn8UmtiLydmYQV6//qCL5dqfNUMhqchdk959hu0XXgauGMYAmYoyWN8FD+voGuMwGyJa7onrc60q1Hu6QFsQXHVA==               |
| ClientSecret    | The encrypted Salesforce Client Secret. You can encrypt this value using the Cinchy CLI.                                                     | DyU1hqde3cWwkPOwK97T6rzwqv6t3bgQeCGq/fUx+tKI=                                                            |
| Username        | The encrypted Salesforce username. You can encrypt this value using the Cinchy CLI.                                                          | dXNlcm5hbWVAZW1haWwuY29t                                                                                 |
| Password        | The encrypted Salesforce password You can encrypt this value using the Cinchy CLI.                                                           | cGFzc3dvcmRwYXNzd29yZA==                                                                                 |
| InstanceAuthUrl | The authorization URL for Salesforce instance.                                                                                               | [https://login.salesforce.com/services/oauth2/token](https://login.salesforce.com/services/oauth2/token) |

```
{
 "ApiVersion": 47.0,
 "GrantType": "password",
 "ClientId": "Bn8UmtiLydmYQV6//qCL5dqfNUMhqchdk959hu0XXgauGMYAmYoyWN8FD+voGuMwGyJa7onrc60q1Hu6QFsQXHVA==",
 "ClientSecret": "DyU1hqde3cWwkPOwK97T6rzwqv6t3bgQeCGq/fUx+tKI=",
 "UserName": "dXNlcm5hbWVAZW1haWwuY29t",
 "Password": "cGFzc3dvcmRwYXNzd29yZA=="
 "InstanceAuthUrl": "https://login.salesforce.com/services/oauth2/token"
}
```
{% endtab %}
{% endtabs %}
