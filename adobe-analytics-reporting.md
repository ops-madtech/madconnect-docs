# Adobe Analytics - Reporting

MadConnect enables seamless integration with Adobe Analytics Reporting, allowing you to pull dimension and metric data directly from an Adobe Analytics report suite into your analytics or reporting tools. Gain valuable insights into site and campaign performance with configurable, repeatable data pulls.

***

### Connector Overview

| **Field**      | **Description**                                                                   |
| -------------- | --------------------------------------------------------------------------------- |
| Connector Type | Source                                                                            |
| Data Type      | Reporting                                                                         |
| Description    | Retrieve dimension and metric reporting data from an Adobe Analytics report suite |

***

### Prerequisites

**Authenticate Adobe Analytics Account**

* OAuth Server-to-Server Authentication: Generate a Client ID and Client Secret for an OAuth Server-to-Server (client\_credentials) credential in the Adobe Developer Console. JWT/Service Account authentication is deprecated by Adobe and is not supported.

\
**Data Access Permissions**

* Ensure the technical account has Developer (or System Admin) rights in Adobe Admin Console to create the integration, and is assigned to an Analytics Product Profile that includes the target Report Suite (RSID) plus the required Dimensions and Metrics — along with the correct Global Company ID for your Adobe Analytics organization.

[https://developer.adobe.com/analytics-apis/docs/2.0/guides/oauth](https://developer.adobe.com/analytics-apis/docs/2.0/guides/oauth)

[https://developer.adobe.com/analytics-apis/docs/2.0/guides/](https://developer.adobe.com/analytics-apis/docs/2.0/guides/)



**Test Report in UI**

* We recommend first building the desired report in **Adobe Analytics Workspace** as a test before proceeding with configuring the reporting connection.

***

### Setting Up the Connector in MadConnect

**Add Connector**

* Go to **Connections** → **My Connectors** → **Add Connector**
* Search for and select **Adobe Analytics Reporting**, then click **Configure**



**Authenticate**

* On the connector page, go to the **Configuration** tab
* Enter the **Client Id** and **Secret Id** from your Adobe Developer Console OAuth Server-to-Server credential
* Click **Save**
* Once complete, the connector's status shows **Configured** on the My Connectors list

***

### Creating a Connection

From Connections → Create Connection, MadConnect walks you through a 4-step wizard: Select Source → Select Destination → Run Configuration → Review.

**Select Source**

* Choose **Adobe Analytics Reporting** as your data source
* Fill in the Source Configuration: **RSID** (the Report Suite ID), **Global Company ID**, **Dimensions** (e.g. Tracking Code, Activity Map Link), and **Metrics** (e.g. Campaign Click-throughs, Cart Additions)
* Click **Next**



**Select Destination**

* Choose your **destination** (e.g. Amazon S3, Azure, GCP, SFTP)
* Fill in the Destination Configuration — e.g. the Bucket URI (s3://bucket-name/path/)
* Click **Next**



**Run Configuration**

Define when and how the connection transfers data:

* Transfer Type — **Manual Transfer** (runs immediately, once) or **Scheduled Transfer** (runs on a regular schedule)
* If Scheduled, set the Schedule Configuration: **Initial Sync start time**, **Timezone**, and **Sync** **Frequency** (e.g. every 1 Day)
* Window Configuration — **Fixed Window** (specific Start Date/End Date) or **Rolling Window** (Loop Back) (a duration in days, plus Timezone)
* Click **Next** once configured.



**Review**

* Enter a descriptive **Connection Name** (e.g. Adobe\_reporting-s3\_destination)
* Review the Source Configuration, Destination Configuration, and Run Configuration summaries — use Edit on any section to make changes
* Click **Create Connection**

The connection now appears under **My Connections**, in the **In-Progress** tab, showing its Source, Destination, and Data Type. To activate the connection click on **Activate** button.

***

### Triggering the Connection

* Go to Connections → **My Connections**
* Locate your connection (search by name if needed)
* Click the **green (Initiate Transfer)** icon in the **Actions** column
* A confirmation modal appears asking: “Would you like to update the connection before initiating the transfer?” — click **Edit Connection** to make changes first, or **Initiate Transfer** to run it as-is
* A confirmation banner — “**Connection transfer initiated successfully**” — appears once the transfer starts

***

### Monitoring the Transfer

* Go to **Reports**
* Find your connection by name in the list
* Click the row's chevron (▾) to expand and view individual Batch ID runs, including Status (Success / Failed), Total and Failed row counts, and Started / Ended timestamps
* Use the **download** icon to export the batch output



**Batch Inspection**

Click into an individual batch to open the Batch Inspection view for a detailed breakdown of that run:

* A pipeline strip at the top shows the status of each stage — Source, Mapping, and Destination (e.g. Success or Failed)
* Delivered, Failed, and Retries counts summarize the outcome of the run
* Started, Ended, and Duration show the run's timing
* The Progress panel breaks down Input Files, Total Records, Failed, Accepted, and Retry Attempts
* If the run failed, open the Errors tab to see the specific error — including an error code (e.g. API\_001), category (e.g. AUTHENTICATION\_ERROR), and details such as the failing endpoint and the underlying API response

***

### Additional Considerations

**Authentication**

* **Server-to-Server only**: This connector uses OAuth Server-to-Server (client\_credentials grant), not a user-login OAuth flow. JWT/Service Account authentication is deprecated by Adobe (EOL June 30, 2025) and is not supported.
* **Token lifetime**: Access tokens last approximately 24 hours. MadConnect caches and reuses the token until it actually expires.
* **Scope requirements**: The credential's OAuth scope must include additional\_info.projectedProductContext (which grants Analytics product entitlement) — without it, API calls fail authorization even with a otherwise-valid token.

***

For more information, see the official : [Adobe Analytics Reporting Official API Documentation](https://developer.adobe.com/analytics-apis/docs/2.0/)

<br>

<br>
