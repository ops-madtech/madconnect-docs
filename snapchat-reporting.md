# Snapchat - Reporting

![](<.gitbook/assets/image (5).png>)

MadConnect enables seamless integration with **Snapchat Ads Reporting**, allowing you to pull Ad Account, Campaign, Ad Squad, or Ad performance and exposure data directly from Snapchat Ads into your analytics or reporting tools. Gain valuable insights to optimize your advertising strategies and improve campaign effectiveness with real-time data synchronization.

***

### Connector Overview

| **Field**      | **Description**                                                                                |
| -------------- | ---------------------------------------------------------------------------------------------- |
| Connector Type | Source                                                                                         |
| Data Type      | Reporting                                                                                      |
| Description    | Retrieve Ad Account, Campaign, Ad Squad, or Ad performance and exposure data from Snapchat Ads |

***

### Prerequisites

**Authenticate**&#x20;

* **OAuth** **Authentication**: Authenticate your **Snapchat Ads account** using **OAuth** to allow data access.



**Data Access Permissions**

* Ensure the authenticating account has **Organization membership** plus the **reports role type** on the **target Ad Account** (labeled "Data Manager" in Snap Business Manager) — this grants read access to reporting data without requiring edit permissions.&#x20;

[https://developers.snap.com/marketing-api/Ads-API/roles](https://developers.snap.com/marketing-api/Ads-API/roles)

[https://developers.snap.com/api/marketing-api/Ads-API/faq](https://developers.snap.com/api/marketing-api/Ads-API/faq)

[https://developers.snap.com/marketing-api/Ads-API/members](https://developers.snap.com/marketing-api/Ads-API/members)

***

### Setting Up the Connector in MadConnect

**Add Connector**

* &#x20;Go to **Connections** → **My Connectors** → **Add Connector**
* &#x20;Search for and select **Snapchat Reporting** connector, then click **Configure**

**Authenticate**

* On **Configuration** tab, Click **Connect to Snapchat Account** to authenticate via **OAuth** — once connected, the button updates to **Connected – Click to Reconnect**
* Alternatively, check **Allow me to enter the tokens manually** to enter credentials directly: **Client** **ID**, **Client** **Secret**, **Access** **Token**, **Refresh** **Token** and click **Save**
* Once complete, the connector's status shows **Configured** on the **My Connectors** list

***

### Creating a Connection

From **Connections** - **Create Connection**, MadConnect walks you through a 4-step wizard: Select Source - Select Destination - Run Configuration - Review.

**Select Source**

* Choose **Snapchat Reporting** as your data source
* Fill in the Source Configuration: **Entity Type** (e.g. Ad Account), **Entity ID** (the Snapchat entity's unique identifier), **Granularity** (e.g. Day), and **Metrics** (e.g. Impressions, Swipes, View Time Millis, Quartile 1–3, View Completion, Spend, Video Views)
* &#x20;Click **Next**



**Select Destination**

* Choose your destination (e.g. Amazon S3, GCP, Azure or Snowflake, bigQuery )
* &#x20;Fill in the Destination Configuration — e.g. the Bucket URI (s3://bucket-name/path/)
* Click **Next**



**Run Configuration**

Define when and how the connection transfers data:

* &#x20;Transfer Type — **Manual Transfer** (runs immediately, once) or **Scheduled Transfer** (runs on a regular schedule)
* If Scheduled, set the Schedule Configuration: **Initial Sync start time**, **Timezone**, and **Sync** **Frequency** (e.g. every 1 Day)
* &#x20;Window Configuration — **Fixed Window** (specific Start Date/End Date) or **Rolling Window** (Loop Back) (a duration in days, e.g. 15, plus Timezone)
* Click **Next** once configured.



**Review**

* Enter a descriptive **Connection Name** (e.g. snapchat\_reporting-s3\_destination)
* Review the Source Configuration, Destination Configuration, and Run Configuration summaries — use Edit on any section to make changes
* Click **Create Connection**

The connection now appears under **My Connections**, in the **In-Progress** tab, showing its Source, Destination, and Data Type. To activate the connection click on **Activate** button.

***

### Triggering the Connection

* Go to **Connections** → **My Connections**
* Locate your connection (search by name if needed)
* Click the **green  ( Initiate Transfer )** icon in the Actions column
* A confirmation modal appears asking: “Would you like to update the connection before initiating the transfer?” — click **Edit Connection** to make changes first, or **Initiate Transfer** to run it as-is
* &#x20;A confirmation banner — “**Connection transfer initiated successfully**” — appears once the transfer starts

***

### Monitoring the Transfer

* Go to **Reports**
* Find your connection by name in the list
* Click the row's chevron to expand and view individual Batch ID runs, including Status (Success / Failed), Total and Failed row counts, and Started / Ended timestamps
* &#x20;Use the **download** icon to export the batch output

***

For more information, see the official documentation :

* [Snapchat Ads Reporting API documentation - Campaign report](https://developers.snap.com/marketing-api/Ads-API/campaigns)&#x20;
* [Snapchat Ads Reporting API documentation - ad-squad report](https://developers.snap.com/marketing-api/Ads-API/ad-squads)
* [Snapchat Ads Reporting API documentation - ads report](https://developers.snap.com/marketing-api/Ads-API/ads)

<br>

