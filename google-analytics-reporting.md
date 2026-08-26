# Google Analytics - Reporting

MadConnect enables seamless integration with the Google Analytics 4 (GA4) Data API, allowing you to pull website and app reporting data — like users, sessions, page views, and revenue — directly from a client's GA4 property into your own data warehouse. Gain valuable insights to power dashboards and reporting alongside your other data sources.

***

### Connector Overview

| **Field**         | **Description**                                                                   |
| ----------------- | --------------------------------------------------------------------------------- |
| Connector Type    | Source                                                                            |
| Data Type         | Reporting                                                                         |
| Description       | Retrieve report data (dimensions and metrics) from a Google Analytics 4 property  |
| Primary Use Case  | Website/app performance reporting, blending GA4 data with other analytics sources |
| Supported Actions | Read                                                                              |

***

### Prerequisites

Before configuring the connector in MadConnect, ensure you have:

#### Authentication

* A Google Service Account JSON key

This key is created by the client in their own Google Cloud project, then provided to MadConnect to use for logging in to the GA4 Data API on their behalf. No client login or consent screen is involved at run time — it's a server-to-server connection.

#### Access Requirements

* The client must create the service account and add its email address as a Viewer on their GA4 property before any data can be pulled. Without this step, calls will fail.
* Network permissions allowing outbound HTTPS connections to analyticsdata.googleapis.com

#### Know Your Property ID and Report Fields Before Configuring

We recommend confirming the following before setting up the connection:

* The numeric GA4 Property ID you want to pull data from (found in GA4 Admin → Property Settings). This is the number — not the "G-XXXXXXX" Measurement ID, and not a "UA-" ID.
* The dimensions (e.g. date, country, traffic source) and metrics (e.g. active users, sessions, revenue) you want in the report. (Max 9 dimensions and 10 metrics per report)

***

### Steps to Obtain Credentials

The client (or their Google Cloud/Workspace admin) performs the following one-time setup in Google Cloud Console, then shares the resulting JSON key with MadConnect.

**1. Create a Google Cloud Project**

* Go to [console.cloud.google.com](https://console.cloud.google.com)
* Click the project dropdown → New Project
* Name it, then Create

**2. Enable the Google Analytics Data API**

* In the search bar, search "Google Analytics Data API"
* Click it, then click Enablee

**3. Create a Service Account**

* Go to IAM & Admin → Service Accounts
* Click + Create Service Account
* Give it a name, click Create and Continue
* Skip the optional role/access screens, click Done

**4. Generate and Download the JSON Key**

* Click into the new service account → Keys tab
* Click Add Key → Create new key
* Choose JSON, click Create
* A .json file downloads automatically — this is the key to share with MadConnect and enter into the connector configuration
* Copy the service account's email address (shown at the top of the service account page, looks like name@project-id.iam.gserviceaccount.com) — you'll need it for the next step

**5. Grant the Service Account Access in GA4**

* Go to [analytics.google.com](https://analytics.google.com), select the client's GA4 property
* Admin (bottom-left gear icon) → Property Access Management
* Click + → Add users
* Paste in the service account's email address
* Set the role to Viewer
* Click Add

Once these steps are complete, the client shares the downloaded JSON key file with MadConnect to enter into the connector configuration.

***

### Setting Up the Connector in MadConnect

**Add Platform**

1. Go to My Platforms → Add Platform
2. Select Google Analytics 4 (Source) and click Configure



**Authenticate**

1. Paste the full contents of the Service Account JSON key file into the provided field
2. Click Save
3. Once connected, status shows Configured



**Create Connection**

1. Choose Google Analytics 4 as the source
2. Enter the numeric Property ID
3. Select your Dimensions (e.g. date, sessionSource) and Metrics (e.g. activeUsers, newUsers, totalRevenue) from the dropdowns
4. Choose your destination (S3, Snowflake, BigQuery, etc.)<br>

**Set Date Window**

* Choose Fixed Window (a specific Start Date/End Date that stays the same on every run — useful for pulling one specific historical period)
* or Rolling Window (a duration in days measured from Today or Yesterday — useful for always pulling the most recent N days)



**Run or Schedule**

* Run a Manual Pull for one-time data retrieval
* Or configure a Scheduled Sync for recurring data refreshes (e.g. daily)



**Review and Save Connection**

* Review the connection configuration and click Save

***

### Data Dictionary

Sample Report Output

| **Dimension/Metric** | **Example Value** | **Notes**                                                            |
| -------------------- | ----------------- | -------------------------------------------------------------------- |
| date                 | 2026-08-05        | Returned by GA4 as 20260805; MadConnect converts it to a proper date |
| sessionSource        | google            | Where the traffic came from                                          |
| activeUsers          | 1542              | Whole number                                                         |
| newUsers             | 890               | Whole number                                                         |
| totalRevenue         | 3421.75           | Currency, in the property's default currency (e.g. USD)              |

Each dimension you select becomes a text column, and each metric becomes a properly typed numeric or currency column in your destination.

***

### Additional Considerations

* Data limits: Each GA4 property has a daily and hourly limit on how much can be pulled (200,000 units per day for standard properties, higher for Google Analytics 360 properties). Normal day-to-day syncs use only a small fraction of this.
* Large reports: If a report has more rows than fit in a single pull, MadConnect automatically retrieves the rest in additional pages — no manual action needed.
* Read-only: This connector only reads data out of GA4. It never writes or changes anything inside the client's Google Analytics account.
* Field limits: A single report can include up to 9 dimensions and 10 metrics. If you need more, split them across two separate connections.
* Field compatibility: Not all dimensions and metrics can be combined in the same report, GA4 restricts certain pairings. If you select an incompatible combination, the error message will tell you exactly which field(s) to remove. You can check compatibility in advance using GA4's own tool ([More information](https://ga-dev-tools.web.app/ga4/dimensions-metrics-explorer/))

***



For more information on the Google Analytics Data API, please review the official [Google Analytics Data API documentation](https://developers.google.com/analytics/devguides/reporting/data/v1).

<br>
