---
hidden: true
---

# TikTok Marketing - Reporting

MadConnect enables seamless integration with **TikTok Ads Reporting,** allowing you to pull performance and exposure data — at the Advertiser, Ad Account, Campaign, Ad Group, and Ad level — directly from TikTok Ads into your analytics or reporting tools. Gain valuable insights to optimize your advertising strategies and improve campaign effectiveness with real-time data synchronization. .

***

### Connector Overview

| **Field**      | **Description**                                                 |
| -------------- | --------------------------------------------------------------- |
| Connector Type | Source                                                          |
| Data Type      | Reporting                                                       |
| Description    | Retrieve campaign performance and exposure data from TikTok Ads |

***

### Prerequisites

**Authenticate TikTok Ads Account**

* OAuth Authentication: Authenticate your **TikTok Ads account** using **OAuth** to allow data access.



**Data Access Permissions**

* Ensure the authenticating account has **Operator** or **Admin access** on the target Ad Account in TikTok Business Center — Analyst-level access does not support OAuth API authorization. \
  [https://ads.tiktok.com/help/article/about-business-center-roles-and-permissions?lang=en](https://ads.tiktok.com/help/article/about-business-center-roles-and-permissions?lang=en)[https://ads.tiktok.com/help/article/about-assets-and-asset-level-permissions](https://ads.tiktok.com/help/article/about-assets-and-asset-level-permissions)



**Test Report in UI**

* We recommend first building the desired report in the TikTok Ads UI as a test before proceeding with configuring the reporting connection.

***

### Setting Up the Connector in MadConnect

**Add Connector**

* &#x20;Go to **Connections** → **My Connectors** → **Add Connector**
* Search for and select **TikTok Ads - Reporting** connector, then click **Configure**



**Authenticate**

* Click **Connect to TikTok Account** to authenticate via **OAuth** — once connected, the button updates to Connected – **Click to Reconnect**
* &#x20;Alternatively, check **Allow me to enter the tokens manually** to enter the Access Token directly and Click **Save**
* Once complete, the connector's status shows **Configured** on the **My Connectors** list

***

### Creating a Connection

From **Connections** → **Create Connection**, MadConnect walks you through a 4-step wizard: Select Source → Select Destination → Run Configuration → Review.

**Select Source**

* Choose **TikTok Ads - Reporting** as your data source
* Fill in the Source Configuration: Advertiser Id, Report Type (BASIC, AUDIENCE, PLAYABLE MATERIAL, or CATALOG), Granularity (e.g. AUCTION AD, AUCTION ADGROUP, AUCTION CAMPAIGN, AUCTION ADVERTISER), Dimensions, and Metrics (e.g. Impressions, Clicks, Spend, Conversion, CTR, CPC, CPM, Conversion Rate V2, Reach, Frequency, Video Play Actions, Video Watched 2s/6s, Video Views P25/P50/P75/P100, Engagements)
* Click Next



**Select Destination**

* Choose your destination (e.g. Amazon S3, GCP, Azure etc)
* &#x20;Fill in the Destination Configuration — e.g. the Bucket URI (s3://bucket-name/path/)
* &#x20;Click **Next**



**Run Configuration**

Define when and how the connection transfers data:

* Transfer Type — **Manual Transfe**r (runs immediately, once) or **Scheduled Transfer** (runs on a regular schedule)
* If Scheduled, set the Schedule Configuration: **Initial Sync start time**, **Timezone**, and **Sync** **Frequency** (e.g. every 1 Day)
* Window Configuration — **Fixed Window** (specific Start Date/End Date) or **Rolling Window** (Loop Back) (a duration in days, plus Timezone)
* Click **Next** once configured.



**Review**

* Enter a descriptive **Connection Name** (e.g. tiktok\_s3\_transfer)
* Review the Source Configuration, Destination Configuration, and Run Configuration summaries — use Edit on any section to make changes
* Click **Create Connection**

The connection now appears under **My Connections**, in the **In-Progress** tab, showing its Source, Destination, and Data Type. To activate the connection click on **Activate** button.

***

### Triggering the Connection

* Go to **Connections** → **My Connections**
* Locate your connection (search by name if needed)
* Click the **green**  ( **Initiate Transfer** ) icon in the Actions column
* &#x20;A confirmation modal appears asking: “Would you like to update the connection before initiating the transfer?” — click **Edit Connection** to make changes first, or **Initiate Transfer** to run it as-is
* &#x20;A confirmation banner — “**Connection transfer initiated successfully**” — appears once the transfer starts

***

### Monitoring the Transfer

* Go to **Reports**
* Find your connection by name in the list
* Click the row's chevron (▾) to expand and view individual Batch ID runs, including Status (Success / Failed), Total and Failed row counts, and Started / Ended timestamps
* Use the **download** icon to export the batch output



**Batch Inspection**

Click into an individual batch to open the Batch Inspection view for a detailed breakdown of that run:

* &#x20;A pipeline strip at the top shows the status of each stage — Source, Mapping, and Destination (e.g. Success or Failed)
* &#x20;Delivered, Failed, and Retries counts summarize the outcome of the run
* Started, Ended, and Duration show the run's timing
* The Progress panel breaks down Input Files, Total Records, Failed, Accepted, and Retry Attempts
* &#x20;If the run failed, open the Errors tab to see the specific error — including an error code, category, and details such as the failing endpoint and the underlying API response

***

## Additional Considerations

* Report Type determines available fields: Dimensions and Granularity options in Source Configuration change based on the selected Report Type (BASIC, AUDIENCE, PLAYABLE MATERIAL, CATALOG) — pick Report Type first before configuring Dimensions and Granularity.
* Metrics selection affects report shape: available metrics span performance (Impressions, Clicks, Spend, CTR, CPC, CPM), conversion (Conversion, Conversion Rate V2), reach/frequency, and video engagement (Video Play Actions, Video Watched 2s/6s, Video Views P25–P100, Engagements) — select only what's needed, since wider metric sets increase report size and pull time.

***

For more information, see the official :[Tiktok Ads Reporting Official API Documentation](https://business-api.tiktok.com/portal/docs)

<br>
