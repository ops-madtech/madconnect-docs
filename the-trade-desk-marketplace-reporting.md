---
description: >-
  MadConnect enables seamless integration with The Trade Desk's Platform API,
  allowing you to pull third-party data usage and spend directly from The Trade
  Desk Marketplace into your analytics, reportin
---

# The Trade Desk Marketplace - Reporting

See how much your data segments were used — impressions and gross data cost — broken out by partner, advertiser, campaign, ad group, segment, and brand for a chosen date window, and keep that data automatically synchronized so your team always has an up-to-date view of marketplace usage.

***

### Connector Overview

| Field             | Value                                                   |
| ----------------- | ------------------------------------------------------- |
| Connector Type    | Source                                                  |
| Data Type         | Reporting                                               |
| Platform Type     | Data Marketplace / Demand-Side Platform (DSP) Reporting |
| Supported Actions | Get                                                     |

***

To configure this connector, you must have an existing Marketplace agreement in place with The Trade Desk. Your Trade Desk Data Partnerships Manager will be able to assist you in gathering the various TTD identifiers outlined in this guide.

#### Authenticate The Trade Desk Account

The Trade Desk Marketplace Reporting Connector uses key-based (API token) authentication, not OAuth.

Required field:

* API Token

The token must be a Platform API token, generated in the OpenTTD Access Management app (set the Application field to Platform API). The login it belongs to must have Edit-level access to the Third-Party Data Provider entity — this connector only reads data, but The Trade Desk still checks for that permission. Your existing data provider account access normally covers this.

The token is shown only once when it is created and cannot be retrieved again — copy it somewhere safe before you close the dialog. If it is ever lost or needs replacing, generate a new one in The Trade Desk and re-enter it here.

#### Data Access Permissions

Make sure the API token's account is authorized for the Third-Party Data Provider ID you enter. If it is not, requests fail with "The entity was not found."

#### Know Your IDs and Date Window Before Configuring

We recommend confirming the following before setting up the connection:

* Your Third-Party Data Provider ID — assigned by your Trade Desk Data Partnerships Manager.
* Any Brand ID you want to report on individually (optional).
* The metrics and the dimension you want in the report. Metrics (impressions, dataCostInUSD) — pick one or more. Dimension — pick exactly one of: partnerBasicInfo, advertiserBasicInfo, campaignBasicInfo, adGroupBasicInfo, thirdPartyData (segment), thirdPartyDataBrand.
* Your intended date range.&#x20;

#### Rules:

* Start Date must be within the last 60 days. An older date fails the run with start\_date (\<date>) is outside The Trade Desk 60-day lookback window (earliest allowed: \<date>).
* Dates are used exactly as you enter them. The connection's timezone setting is not applied — a Start Date of 2026-07-12 is sent to The Trade Desk as 2026-07-12, with no UTC conversion or day shift.
* End Date is inclusive — its full day is included in the results.
* The Trade Desk's usage data itself rolls up at end of day UTC, so the most recent complete day is yesterday (no same-day data).
* Date ranges longer than 31 days are automatically split into multiple requests and combined for you.

### Configure Connector

1. Navigate to My Platforms — Go to "My Platforms" in the MadConnect UI.
2. Add a New Platform — Click "Add Platform."
3. Select The Trade Desk Marketplace Reporting Connector — Choose the "The Trade Desk – Marketplace Reporting" tile and click "Configure."
4. Go to Configuration — Open the "Configuration" tab.
5. Enter Your API Token — Paste the Platform API token.
6. Enter the Third-Party Data Provider ID — The provider's unique ID in The Trade Desk.
7. Choose Brand Scope — Enter a single Brand ID to report on one brand, or leave it blank to report on all brands under the Provider ID (each row still shows which brand it belongs to).
8. Select Metrics and Dimension — Choose one or more Metrics (impressions, dataCostInUSD) and exactly one Dimension (Partner, Advertiser, Campaign, Ad Group, Segment, or Brand) from the dropdowns. At least one metric is required, and the dimension is required — an empty selection fails the run with a "required, missing value" error. The output columns follow your choices.
9. Set the Date Window — Enter a Start Date and End Date. Keep the Start Date within the last 60 days; the connector handles splitting longer ranges automatically.
10. Set Transfer Type — Choose Manual Transfer (run once, immediately) or Scheduled Transfer (run automatically on a recurring schedule).
11. Verify Configuration — Ensure the platform status is marked as "Configured" under My Platforms.

***

### Running a Transfer in MadConnect

1. Navigate to **Create Connection**.
2. Select **The Trade Desk Marketplace - Reporting** as the Source.
3. Enter all required Source Configuration fields.
4. Select a Destination connector (e.g., Snowflake, S3, BigQuery).
5. Configure transfer settings — one-time manual run or scheduled transfer.
6. Click **Create Connection**, then **Activate**.
7. Click the **Initiate Transfer** button to run the first transfer.

Monitor execution status, record counts, and any error details from the **Reports** section of your workspace.

***

### Important Notes

* Cost is an estimate. dataCostInUSD is a directional, intra-month figure that excludes the marketplace participation fee (MPF). Use your monthly billing report for revenue reconciliation.
* Rolling up by brand or advertiser. Every row carries a brand ID and an advertiser ID, so you can total impressions and cost per brand or per advertiser directly in your reporting tool — no extra setup needed.
* One brand per filter. The Trade Desk's brand filter accepts a single Brand ID. To report on several brands separately, set up the connection once per Brand ID. To see them all together, leave Brand ID blank.
* Dates are literal. Whatever Start / End dates you pick are passed to The Trade Desk as-is. Your connection timezone does not move them — pick the calendar dates you actually want reported.
* Metrics and dimensions are your choice. Metrics is multi-select (impressions, dataCostInUSD); dimension is single-select — The Trade Desk's API breaks a report out by one dimension at a time. To see the data in more than one dimension, set up one connection per dimension.

***

### Data Dictionary

#### Usage & spend output

Each metric you select becomes a numeric or currency column, and the one dimension you select becomes its id and name columns. Dimensions you did not select are not in the output. The table below documents every possible column.

| Field                             | Data Type | Description                                                                                                                                                         | Example                                          |
| --------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| impressions                       | int64     | Total ad impressions served for this row.                                                                                                                           | 4102983                                          |
| dataCostInUSD                     | float64   | Gross data cost in USD for this row. This is a directional estimate — it excludes The Trade Desk's marketplace participation fee (MPF) and is not a billing actual. | 1284.55                                          |
| partnerBasicInfo\_name            | string    | Name of the partner (the buying entity working with the advertiser).                                                                                                | Northlake Media                                  |
| partnerBasicInfo\_id              | string    | The Trade Desk ID for the partner.                                                                                                                                  | abc12de                                          |
| advertiserBasicInfo\_name         | string    | Advertiser name.                                                                                                                                                    | Vantage Motors                                   |
| advertiserBasicInfo\_id           | string    | The Trade Desk ID for the advertiser. Use this to total usage "by advertiser."                                                                                      | f34gh56                                          |
| campaignBasicInfo\_name           | string    | Campaign name.                                                                                                                                                      | Q3 CTV Prospecting                               |
| campaignBasicInfo\_id             | string    | The Trade Desk ID for the campaign.                                                                                                                                 | j78kl90                                          |
| adGroupBasicInfo\_name            | string    | Ad group name.                                                                                                                                                      | \`CTV                                            |
| adGroupBasicInfo\_id              | string    | The Trade Desk ID for the ad group.                                                                                                                                 | m12no34                                          |
| thirdPartyData\_id                | string    | The Trade Desk's ID for the data segment that was used.                                                                                                             | 9182736                                          |
| thirdPartyData\_targetingData\_id | string    | ID of the targeting-data group the segment belongs to.                                                                                                              | 55310284                                         |
| thirdPartyData\_fullPath          | string    | Full taxonomy path of the segment.                                                                                                                                  | AlwaysOn Data > Auto > In-Market > Full-Size SUV |
| thirdPartyData\_providerElementId | string    | The ID you (the data provider) assigned to this segment in your own taxonomy.                                                                                       | aod-auto-suv-fullsize                            |
| thirdPartyDataBrand\_name         | string    | Brand the usage is attributed to.                                                                                                                                   | AlwaysOn Auto                                    |
| thirdPartyDataBrand\_id           | string    | Brand ID. Use this to total usage "by brand."                                                                                                                       | brand-4471                                       |

For more information on the underlying API, please review The Trade Desk's [Third-party data usage and spend](https://open.thetradedesk.com/provider/docsApp/GuidesProvider/retail/doc/DataThirdPartyUsage) documentation.
