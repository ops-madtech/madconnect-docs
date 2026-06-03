# TheTradeDesk - My Reports

## The Trade Desk – Reporting

MadConnect's The Trade Desk Reporting connector enables programmatic access to campaign performance data from The Trade Desk (TTD) via the My Reports API. This connector allows users to extract report data directly from TTD and deliver it to a configured destination — such as Snowflake, BigQuery, or Amazon S3 — for downstream analytics, reporting, and modeling.

***

### Connector Overview

| Field             | Value                      |
| ----------------- | -------------------------- |
| Connector Type    | Source                     |
| Data Type         | Reporting                  |
| Platform Type     | Demand-Side Platform (DSP) |
| Supported Actions | Get                        |

***

### How This Connector Works

The Trade Desk Reporting connector pulls data from TTD's My Reports API. Reports must be pre-configured in the TTD platform before they can be retrieved through MadConnect. MadConnect retrieves the output of those existing reports on a scheduled or on-demand basis.

There are two supported report paths:

* **Existing Template** — Reference a custom report you have already built and saved in the TTD My Reports UI. You will provide the Template ID and Report Name to pull it via the API.
* **Predefined Reports** — TTD provides a set of standard report templates available to all accounts (e.g., Basic Performance Stats). These can be referenced without additional setup in the TTD UI.

> **Important:** The Trade Desk API does not support creating reports on the fly. The report must exist in your TTD My Reports account before configuring this connection in MadConnect.

***

### Prerequisites

Before configuring this connector, ensure the following are in place:

1. **Active Trade Desk account** with access to the My Reports reporting system.
2. **API credentials** — TTD maintains separate credentials for UI access and API access. UI login credentials cannot be used with this connector. Contact your Trade Desk account representative to request dedicated API credentials and third-party API access to My Reports.
3. **Report pre-built in TTD UI** — The report you want to pull must already exist in your TTD My Reports account. We recommend validating the report output in the TTD UI before connecting it to MadConnect.
4. **MadConnect workspace** with a configured destination connector.

***

### Authentication Requirements

The Trade Desk Reporting uses **Client ID and Client Secret** authentication.

#### Steps

1. Navigate to **My Platforms → The Trade Desk (Reporting) → Configure**.
2. Open the **Configuration** tab.
3. Enter your **Client ID** and **Client Secret** in the provided fields.
4. Click **Save**.
5. Once successful, the connector will appear as **Configured** in My Platforms.

| Field         | Required | Description                                                             |
| ------------- | -------- | ----------------------------------------------------------------------- |
| Client ID     | Yes      | TTD API client identifier. Provided by your TTD account representative. |
| Client Secret | Yes      | TTD API client secret. Provided by your TTD account representative.     |

> **Note:** API credentials are distinct from your TTD UI login. If you are unsure whether you have API credentials, contact your Trade Desk account representative before proceeding.

***

### Source Configuration

When creating a connection with The Trade Desk Reporting as the source, you will be prompted to provide the following fields:

| Field           | Required                | Description                                                                                                                                   |
| --------------- | ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Report Template | Yes                     | Select **Existing Template** to reference a custom report you have pre-built in TTD, or select a **Predefined** TTD standard report template. |
| Partner ID      | Yes                     | Your TTD Partner ID. Scopes data retrieval to your organization.                                                                              |
| Template ID     | Yes (Existing Template) | The unique identifier of your report template as it appears in the TTD My Reports UI.                                                         |
| Report Name     | Yes                     | The name assigned to the report. Used to identify this report execution within MadConnect.                                                    |
| Report Format   | Yes                     | Output format for the report data. Options: **CSV**, **TSV**, or **EXCEL**. CSV is recommended for downstream processing.                     |
| Report Type     | Yes                     | The entity level at which data is reported. Options: **advertiser**, **campaign**, or **adGroup**.                                            |

***

### Running a Transfer in MadConnect

1. Navigate to **Create Connection**.
2. Select **The Trade Desk (Reporting)** as the Source.
3. Enter all required Source Configuration fields.
4. Select a Destination connector (e.g., Snowflake, S3, BigQuery).
5. Configure transfer settings — one-time manual run or scheduled transfer.
6. Click **Create Connection**, then **Activate**.
7. Click the **Initiate Transfer** button to run the first transfer.

Monitor execution status, record counts, and any error details from the **Reports** section of your workspace.

***

### Important Notes

* **Data latency is T+1.** Report data reflects the previous day's activity and is not available same-day.
* **Reports must be pre-built in TTD.** MadConnect cannot create or modify report configurations in the TTD platform. All report setup must be completed in the TTD My Reports UI before connecting.
* **Schema mapping is not required.** For reporting connections, field mapping is handled automatically. Data is delivered to the destination in the schema defined by the report template.
* We recommend building and validating your desired report in the TTD UI before configuring the MadConnect connection, to confirm the dimensions and metrics are correct.

***

### Additional Resources

* [The Trade Desk My Reports Documentation](https://partner.thetradedesk.com/v3/portal/reds/doc/UserDefinedReports)
* [The Trade Desk Report Templates](https://partner.thetradedesk.com/v3/portal/reds/doc/ReportTemplates)
* [The Trade Desk Predefined Reports](https://partner.thetradedesk.com/v3/portal/reds/doc/PredefinedReports)
