---
description: >-
  This page describes the Cinchy Webhook ingestion, including a video
  walkthrough and step-by-step guide
---

# Webhook ingestion

## Introduction

**Compatibility:** Webhook ingestion was introduced in Cinchy platform v4.21. Note that previous Cinchy versions won't include a Webhooks system table, and won't support this feature.

**Context:** A webhook uses a trigger event to initiate a data transfer that can then be ingested by another application. Many applications support webhooks to provide data updates in real time with minimal configuration required. Cinchy users can subscribe to and ingest webhooks via configuring a unique API endpoint. When the external application addresses this endpoint, a pre-identified saved query can be run, under an authorized user account, to ingest the data and insert or update it into Cinchy. The following video walks you through a specific configuration, with a general step-by-step guide below:

## Walkthrough video <a href="#walkthrough-video" id="walkthrough-video"></a>

{% embed url="https://vimeo.com/644493386/9398d94616" %}

## Configure a webhook <a href="#step-by-step-instructions-for-configuring-a-webhook" id="step-by-step-instructions-for-configuring-a-webhook"></a>

1. Identify the table in Cinchy that you want the information to be pushed to. _(Image 1)_

![Image 1: Identifying your Table (Step 1)](<../../.gitbook/assets/image (375).png>)

{% hint style="info" %}
See the [Creating tables page ](../../guides-for-using-cinchy/builder-guides/creating-tables/) for instructions on creating a table within Cinchy.
{% endhint %}

1. Create your query in Cinchy. This query should take data from the webhook event, and push it into the Cinchy table that you identified in _Step 1. (Image 2)_

- Your query will be running under a specific user account that you will assign in the next step. Ensure that whichever user you choose for this purpose has the correct permissions to execute the query, and to insert / update data in the target table.

![Image 2: Creating your Query (Step 2)](<../../.gitbook/assets/image (617).png>)

{% hint style="info" %}
See the [Saved Queries page](../../guides-for-using-cinchy/builder-guides/saved-queries.md) for instructions on creating a query within Cinchy.
{% endhint %}

{% hint style="warning" %}
You will need administrator access to your Cinchy platform to perform _Step 3._
{% endhint %}

3. Navigate to the webhooks table in Cinchy, and populate the following columns _(Image 3):_

1. **Key:** This can be any string (we recommend that you treat this like a password and make it random and unguessable).
2. **Run As:** Insert the user who will be running this query here. This should be the same user to whom you gave permissions in _Step 2a._
3. **Saved Query:** Select the saved query that you created in _Step 2._
4. **Forward Payload as Parameter:** This column will depend on the manner in which you would like to ingest the webhook payload.
   - If you are configuring individual parameters in the webhook payload (for example: @name, @url, etc.), you may leave this column blank.
   - If you aren't configuring individual parameters, as an alternative you can ingest the entire payload under one parameter and specify it in this field. In the below image, we've defined it as `JSON`. This means that the full payload (which happens to be a JSON file in this case) will be parameterized as @JSON and then inserted into a table column named JSON.

![Image 3: Configuring your Table (Step 3)](<../../.gitbook/assets/image (604).png>)

4. In your source application, navigate to the webhook settings and configure the following:

- **URL:** This will be your **Base URL + /API/callback?key= + the key value that you assigned in **_**Step 3a.**_
- **Example:** _{baseURL}/API/callback?key=rGh5tfgHK8989J5_
