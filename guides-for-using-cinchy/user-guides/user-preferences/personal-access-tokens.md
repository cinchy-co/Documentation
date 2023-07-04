# Personal Access Tokens

## Overview

You now have the option to use personal access tokens (PATs) in Cinchy, which are alternatives to using passwords for authentication. Similar to [Cinchy Bearer Tokens](https://platform.docs.cinchy.com/api-guide/api-overview/api-authentication#post-bearer-token-request), you can use a Cinchy PAT to call the Cinchy API as your current user, meaning your associated access controls will be honoured as well. Cinchy PATs, however, have an expiration date of up to 1 year. A single user can have up to 5 PATs active at one time.\
\
See [Authentication](../../../api-guide/api-overview/api-authentication.md) for details on using a PAT in lieu of a Bearer token.

## 2. Creating a PAT

1. From the Cinchy homepage, navigate to your **User Settings > Tokens.** Any tokens that you make will appear here. You will also be able to see any expired tokens.

<figure><img src="../../../.gitbook/assets/image (715).png" alt=""><figcaption></figcaption></figure>

2. Click **“Generate New Token”.**

{% hint style="warning" %}
**Note:** You can have up to 5 active (non-expired) tokens at a time. Once you reach that threshold, the “Generate New Token” button will not work.
{% endhint %}

3\. Input the following information about your PAT and click **Generate:**

1. **Token Name**
2. **Description**
3. **Expiration**

<figure><img src="../../../.gitbook/assets/image (290).png" alt=""><figcaption></figcaption></figure>

4\. Once generated, make sure to copy down the PAT somewhere secure. You will not be able to view the PAT again once you navigate away from this screen.

<figure><img src="../../../.gitbook/assets/image (240).png" alt=""><figcaption></figcaption></figure>

## 3. Deleting a PAT

1. From the Cinchy homepage, navigate to your **User Settings > Tokens.** Any tokens that you make will appear here.
2. Click the **“Delete”** button next to the applicable PAT.

## 4. Using a PAT in an API

Cinchy PATs can be used in much the same way that Bearer tokens are used for in [API authentication](../../../api-guide/api-overview/api-authentication.md). I.E. in the **Authorization** header with the value: **"Bearer \<token>".**

You may also wish to review the information on [using PATs in Excel or PowerBI.](../../builder-guides/integration-guides.md)
