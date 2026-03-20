# Amazon Ads - Reporting

### Amazon Ads - Reporting&#x20;

The **Amazon Ads Reporting Connector** enables programmatic access to campaign performance and measurement data from Amazon Ads via their Reporting API (v3).

This connector allows users to extract reporting datasets directly from Amazon Ads and deliver them to a configured destination (e.g., Snowflake, S3, BigQuery) for downstream analytics, reporting, and modeling.

Supported report types include:

* **Audience Reports** – Performance by audience segment
* **Campaign Reports** – Campaign and line item performance
* **Geo Reports** – Performance by geographic region

### Connector Overview

| Field                 | Value                                                                                                       |
| --------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Connector Type**    | Source                                                                                                      |
| **Data Type**         | Reporting                                                                                                   |
| **Description**       | Enables retrieval of campaign performance and measurement data from Amazon Ads via the Reporting API (v3).  |
| **Supported Actions** | Get                                                                                                         |

***

### Prerequisites

* Active **Amazon Ads account**
* Access to **Amazon Ads API (Reporting v3)**
* Appropriate permissions to generate reports
* MadConnect workspace with a configured destination

***

### Authentication Requirements

You must authenticate your Amazon Ads account using OAuth.

#### Option 1: OAuth Authentication (Recommended)

OAuth provides the most secure and stable authentication.

**Steps**

1. Go to **My Platforms → Amazon Ads → Configure**
2. Click **Sign in with Amazon Ads**
3. Log in with your Amazon Ads credentials
4. Approve all requested permissions
5. Once successful, the platform will appear as **Configured** in My Platforms

**One-Time OAuth Link (Copy Icon)**\
A copy icon generates a single-use OAuth URL, which can be shared with a teammate who must complete authentication.

**Use cases:**

* Authentication must be completed by an account admin
* Shared advertiser account access

**Notes:**

* The link expires after one successful authentication
* A new link must be generated for additional attempts

***

#### Option 2: Token-Based Authentication (If Applicable)

Amazon Ads primarily uses OAuth. In limited cases, token-based authentication may be supported depending on account setup.

**Required fields:**

* Access Token
* Refresh Token
* Client ID / Client Secret

These must align with an Amazon Ads developer application with reporting permissions.

***

### Configuration Fields

| Field                          | Description                         |
| ------------------------------ | ----------------------------------- |
| **Advertiser Account ID**      | Amazon Ads advertiser profile ID    |
| **Report Type**                | Audience, Campaign, or Geo          |
| **Date Range**                 | Start and end date for report       |
| **Time Unit**                  | DAILY (standard for v3 reporting)   |
| **Lookback Window (optional)** | Rolling window for late conversions |
| **Destination**                | Snowflake, S3, BigQuery, etc.       |
| **Schedule**                   | Daily or custom frequency           |

***

### Supported Reports & Fields

Below reflects commonly used and supported fields from Amazon Ads Reporting v3.\
(Exact availability may vary slightly by ad product and account setup.)

***

#### Audience Report

**Dimensions**

| Field         | API Name       |
| ------------- | -------------- |
| Date          | `date`         |
| Campaign ID   | `campaignId`   |
| Ad Group ID   | `adGroupId`    |
| Audience ID   | `audienceId`   |
| Audience Name | `audienceName` |

**Metrics**

| Field             | API Name          |
| ----------------- | ----------------- |
| Impressions       | `impressions`     |
| Clicks            | `clicks`          |
| Spend             | `cost`            |
| Conversions       | `conversions`     |
| Purchases         | `purchases`       |
| Sales             | `sales`           |
| Units Sold        | `unitsSold`       |
| Detail Page Views | `detailPageViews` |

***

#### Campaign Report

**Dimensions**

| Field         | API Name       |
| ------------- | -------------- |
| Date          | `date`         |
| Campaign ID   | `campaignId`   |
| Campaign Name | `campaignName` |
| Ad Group ID   | `adGroupId`    |
| Ad Group Name | `adGroupName`  |
| Creative ID   | `creativeId`   |
| Creative Type | `creativeType` |

**Metrics**

| Field                       | API Name            |
| --------------------------- | ------------------- |
| Impressions                 | `impressions`       |
| Clicks                      | `clicks`            |
| Spend                       | `cost`              |
| Conversions                 | `conversions`       |
| Purchases                   | `purchases`         |
| Sales                       | `sales`             |
| Units Sold                  | `unitsSold`         |
| Video Views (if applicable) | `videoViews`        |
| Video Completion Rate       | `videoCompleteRate` |

***

#### Geo Report

**Dimensions**

| Field        | API Name      |
| ------------ | ------------- |
| Date         | `date`        |
| Country Code | `countryCode` |
| Region       | `region`      |
| Campaign ID  | `campaignId`  |
| Ad Group ID  | `adGroupId`   |

**Metrics**

| Field       | API Name      |
| ----------- | ------------- |
| Impressions | `impressions` |
| Clicks      | `clicks`      |
| Spend       | `cost`        |
| Conversions | `conversions` |
| Purchases   | `purchases`   |
| Sales       | `sales`       |

***

### Running a Transfer in MadConnect

1. Navigate to **Create Connection**
2. Select **Amazon Ads** as the **Source**
3. Select the desired **Report Type**
4. Input configuration fields (account, date range, etc.)
5. Select a **Destination**
6. Set schedule or run one-time
7. Launch the transfer

***

### Important Notes

* Reporting is **asynchronous** (job-based retrieval)
* Data is typically **T+1 availability**
* Use a **rolling lookback window** for accurate conversion tracking
* Metric availability varies by:
  * Ad product (DSP vs Sponsored Ads)
  * Campaign configuration
* Some fields may not be available across all report types
* Data may differ slightly from UI due to:
  * Attribution timing
  * Reporting delays

***

### Additional Resources

* Amazon Ads Reporting API (v3)\
  [https://advertising.amazon.com/API/docs/en-us/guides/reporting/v3](https://advertising.amazon.com/API/docs/en-us/guides/reporting/v3)
* Audience Reports\
  [https://advertising.amazon.com/API/docs/en-us/guides/reporting/v3/report-types/audience](https://advertising.amazon.com/API/docs/en-us/guides/reporting/v3/report-types/audience)
* Campaign Reports\
  [https://advertising.amazon.com/API/docs/en-us/guides/reporting/v3/report-types/campaign](https://advertising.amazon.com/API/docs/en-us/guides/reporting/v3/report-types/campaign)
* Geo Reports\
  [https://advertising.amazon.com/API/docs/en-us/guides/reporting/v3/report-types/geo](https://advertising.amazon.com/API/docs/en-us/guides/reporting/v3/report-types/geo)
