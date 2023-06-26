# Post Sync Scripts

## 1. Overview

Post sync scripts are written in CQL and can be added to the end of a sync to allow you to do more with your data, such as inserted retrieved values into a Cinchy table.

<figure><img src="../../../.gitbook/assets/image (285).png" alt=""><figcaption></figcaption></figure>

## 2. Example Walkthrough

In this example, we will be doing a batch data sync using the Cinchy Table **\[Product].\[Names Test].** In this scenario we will be using the following simple API as our destination:  [https://cinchy.net/API/Product/LatestCinchyReleaseDate](https://cinchy.net/API/Product/LatestCinchyReleaseDate)

When we run our batch job, it will check for updates in our source table. These updates will trigger our REST API to fetch our defined value, which we can use in a post-sync script. In this case, our script will insert this value into a second table, **\[Product].\[Response Table].**

The following steps will walk you through how to utilize this functionality.

1. Set up your data sync to a REST API as normal. You can review our **Source** _(Image 1),_ **Destination** _(Image 2)_, and **Sync Behaviour** _(Image 3)_ below.

<figure><img src="https://lh5.googleusercontent.com/_eb2qTk-AHKJKOyTDs_x2INkb411fJsUr4SnJXZn-Lw-g-MmKYvDVS-8Ipbe365ylWIEKGfzIsryr4A50j7VNgyqdWDER4oc0b0plB3NXsTW4IpyWIjlvwZBJs24Y9zqeCK7hkQXWrTj2d5ZMmd5nuAnEulIjtQ-sE4cWSsWLZO0NcissuusyQs4On9FJQ" alt=""><figcaption><p>Image 1: Post Sync Scripts</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (110).png" alt=""><figcaption><p>Image 2: Post Sync Scripts</p></figcaption></figure>

<figure><img src="https://lh6.googleusercontent.com/Wbc5RRQRbhcuhULXlKvqBUrSiRw2HcDIimdA6-0WgC5_6LzW94wqRDOMLFDVCMNXOv7FlL8wGdvhmCe0-Ky6Xoz-S_5OlOLavKr421mVU6m6IOj852-ceSdR5Alth6IbeJZUhhaFAttoQTb191Gj9MeeZi0CNrsHJMMABqfHBz1Cbraiez2-A8DX6yTpsQ" alt=""><figcaption><p>Image 3: Post Sync Scripts</p></figcaption></figure>

2\. Under the **Post Sync tab**, we want to input the CQL that, in our case, will take the defined variable from our REST API and insert it into the **\[Product].\[Response Test]** table _(Image 4)._

<figure><img src="https://lh4.googleusercontent.com/tgyxXID-3g8RcMVWygxSPMVMScmjqmdJGu0hra3q5SSuRugND3fEFcI4TIs63oQHmjyXBZl8uXGkChgXDJOaJYRRiWGS19Gej0K_bZtviLBO-OafojrzWmwl8AA8TOQ8WbEkdvmfqB12Z8MEG_vGDeBHts3QvWzr3KbU5z5X8mAArpumWvjWRkOsSyiKWg" alt=""><figcaption><p>Image 4: Post Sync Scripts</p></figcaption></figure>

3\. Add in your Permissions and click **Save.**

4\.  Once we have finished configuring our sync, we can run the job. It will check for new data in our Name column on our **\[Product].\[Names]** table _(Image 5)_. When found, it will trigger the REST API to GET our “value” variable. Our post sync script will then take that value and INSERT it into the **\[Product].\[Response Test]** table _(Image 6)._

<figure><img src="https://lh4.googleusercontent.com/IBkC9bHB-O1iuPKmBiHIzim_SV_xgPkp79PH_R9LS6yUkLTbFEchp5jMLlu0-KzPu7-Cbd4SrcvR31XPJCLG8LHpvxgoIdu23IWArqX2EvQ3g7FnoF6feEp_V06X-WSAoZ08JXvG4ud0mZleVqiwwoCL5Xqp0fCSXAGGzBL-z3WcLDkOTubhx129kOP5Ww" alt=""><figcaption><p>Image 5: Post Sync Scripts</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (184).png" alt=""><figcaption><p>Imaage 6: Post Sync Scripts</p></figcaption></figure>
