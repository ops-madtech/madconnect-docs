# Bing Ads - Reporting

MadConnect connects to the Microsoft Advertising (Bing Ads) Reporting API, letting you pull campaign performance data - impressions, clicks, spend, conversions, and more - directly from a client's Bing Ads account into your own data warehouse.

***

### Connector Overview

| **Field**         | **Description**                                                                  |
| ----------------- | -------------------------------------------------------------------------------- |
| Connector Type    | Source                                                                           |
| Data Type         | Reporting                                                                        |
| Description       | Retrieve performance report data from a Microsoft Advertising (Bing Ads) account |
| Primary Use Case  | Campaign performance reporting, blending Bing Ads data with other ad platforms   |
| Supported Actions | Read                                                                             |

***

### Prerequisites

#### Authentication

This connector uses OAuth. You'll sign in with a Microsoft account through MadConnect and approve access — no manual key entry needed for this part.

You'll also need a Developer Token. This is separate from OAuth and comes from Microsoft Advertising directly.

#### Access Requirements

* The signed-in Microsoft account must have access to the target Bing Ads account
* A Microsoft Advertising Developer Token
* Outbound HTTPS access to reporting.api.bingads.microsoft.com

#### Know These Before Configuring

* Your Customer ID and Account ID (found in Bing Ads under Account Settings)
* The report type you want (Account, Campaign, AdGroup, Keyword, or Audience performance)
* The columns you want in the report — these depend on the report type you pick. At least one measurement column is required (e.g. Impressions, Clicks, Spend, Conversions) — selecting only descriptive columns (e.g. CampaignName, CampaignStatus) without a metric will cause the report request to be rejected.

***

### Steps to Obtain a Developer Token

1. Sign in at [ads.microsoft.com](https://ads.microsoft.com)
2. Go to Tools → Developer settings
3. Copy your Developer Token
4. Keep this ready to enter into MadConnect

If you don't see a token, your account may need Microsoft's approval first — request one from the same page.

***

### Setting Up the Connector in MadConnect

**Add Platform**

* Go to My Platforms → Add Platform
* Select Bing Ads (Source) and click Configure

\
**Authenticate**

* Click Sign in with Microsoft and approve access
* Enter your Developer Token
* Click Save — status shows Configured



**Create Connection**

* Choose Bing Ads as the source
* Enter your Customer ID and Account ID
* Choose a Report Type (Account, Campaign, AdGroup, Keyword, or Audience)
* Select the Columns you want — options change based on the report type
* Choose Aggregation: Daily, Weekly, or Monthly
* Choose your Timezone — MadConnect converts this automatically, no need to know Bing's internal format
* Choose your destination (S3, Snowflake, BigQuery, etc.)<br>

**Set Date Range**

* Choose Fixed Window (enter explicit Start/End dates) or Rolling Window (Loop Back) (the platform recalculates the date range automatically at each scheduled run, based on your chosen loop-back period) — both are fully supported.<br>

**Run or Schedule**

* Run a Manual Pull for one-time data retrieval
* Or set up a Scheduled Sync for recurring pulls (e.g. daily)



**Review and Save Connection**

* Review the configuration and click Save

***

### Data Dictionary

Sample Report Output (Campaign report):

| **Column**   | **Example Value** | **Notes**                                           |
| ------------ | ----------------- | --------------------------------------------------- |
| TimePeriod   | 2026-08-05        | Always included automatically, even if not selected |
| CampaignName | Summer Sale       | Text                                                |
| Impressions  | 15420             | Whole number                                        |
| Clicks       | 342               | Whole number                                        |
| Spend        | 128.50            | Currency                                            |

Each column you select becomes a properly typed column in your destination.

#### Required Columns by Report Type

Bing enforces a minimum set of columns per report type — you must select these yourself; if omitted, Bing's API rejects the report request with an error (e.g. RequiredColumnsNotSelected).

| **Report Type** | **Required Columns**                                   | **Reference**                                                                                                                                      |
| --------------- | ------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| Account         | AccountId (or any other attribute column) + TimePeriod | [AccountPerformanceReportColumn](https://learn.microsoft.com/en-us/advertising/reporting-service/accountperformancereportcolumn?view=bingads-13)   |
| Campaign        | TimePeriod                                             | [CampaignPerformanceReportColumn](https://learn.microsoft.com/en-us/advertising/reporting-service/campaignperformancereportcolumn?view=bingads-13) |
| AdGroup         | TimePeriod                                             | [AdGroupPerformanceReportColumn](https://learn.microsoft.com/en-us/advertising/reporting-service/adgroupperformancereportcolumn?view=bingads-13)   |
| Keyword         | TimePeriod                                             | [KeywordPerformanceReportColumn](https://learn.microsoft.com/en-us/advertising/reporting-service/keywordperformancereportcolumn?view=bingads-13)   |
| Audience        | AudienceId + TimePeriod                                | [AudiencePerformanceReportColumn](https://learn.microsoft.com/en-us/advertising/reporting-service/audienceperformancereportcolumn?view=bingads-13) |

In addition, every report type requires at least one measurement column (e.g. Impressions, Clicks, Spend, Conversions) — selecting only descriptive columns without a metric will cause the report request to be rejected, regardless of report type.

***

### Additional Considerations

* Report generation takes time: Bing builds reports asynchronously — this can take a few minutes. MadConnect handles the wait automatically; no manual action needed.
* Data delay: very recent activity (e.g. the last few hours) may not show up in a report yet. Historical date ranges are reliable.
* Read-only: this connector only reads data out of Bing Ads. It never writes or changes anything in the client's account.
* Developer Token is separate from sign-in: even after signing in with Microsoft, the connection won't work without a valid Developer Token.
* No data in range: if no records match your selected date range or report type, the run completes successfully with an empty result.
* Restricted column combinations: for Account, Campaign, and AdGroup reports, certain attribute columns (e.g. DeviceType, DeviceOS, BidMatchType) can't be selected together with impression-share statistic columns (e.g. ImpressionSharePercent) — Bing rejects the request with RestrictedColumnCombinations if both are present. Pick one or the other.

***

For more information, see the official [Bing Ads Reporting API documentation](https://learn.microsoft.com/en-us/advertising/reporting-service/reporting-service-reference).

<br>
