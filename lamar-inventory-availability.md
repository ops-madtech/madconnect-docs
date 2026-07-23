# Lamar Inventory Availability

### Lamar – Inventory Availability

MadConnect enables seamless integration with **Lamar's Extended Availability API**, allowing teams to pull display-level inventory availability and estimated costs directly into their own data environments. This source connector powers media planning.

***

**Connector Overview**

| Field                 | Description                                                                      |
| --------------------- | -------------------------------------------------------------------------------- |
| **Connector Type**    | Source                                                                           |
| **Data Type**         | Inventory Availability & Units                                                   |
| **Description**       | Retrieve display-level inventory availability and estimated cost from Lamar      |
| **Primary Use Cases** | Media planning, forecasting, and powering AI-assisted inventory recommendations. |
| **Supported Actions** | Read                                                                             |

***

**Platform Prerequisites**

Before configuring the connector in MadConnect, ensure you have:

**Authentication**

* Client ID
* Client Secret

These must be issued for a Lamar account with access to the Extended Availability API (\`/ext-availability\`). MadConnect stores them securely and manages token refresh.

**Access Requirements**

1. Ensure the Client ID/Secret ID you provide has access to the specific displays (by Geopath ID or Display ID) you want to check availability for
2. Network permissions allowing outbound HTTPS connections to the Lamar Inventory service

**Know Your IDs and Date Window Before Configuring**

We recommend confirming the Geopath IDs or Display IDs you want to check, and your intended date range, before setting up the connection:

* The date window (Start Date to End Date) can't be more than 730 days apart.
* End Date must be a future date.

***

**Setting Up the Connector in MadConnect**

* **Add Platform**
  * Go to **My Platforms → Add Platform**
  * Select **Lamar Inventory (Source)** and click **Configure**
* **Authenticate**
  * Enter **Client ID**, **Client Secret**, and **Scope**
  * Click **Save**
  * Once connected, status shows **Configured**
* **Create Connection**
  * Choose **Lamar Inventory** as the source
  * Select whether you're identifying displays by Geopath ID or Display ID, and enter the relevant IDs.
  * Choose your destination (S3, Snowflake, BigQuery, etc.)
* **Run or Schedule**
  * Run a **Manual Pull** for one-time data retrieval
  * Or configure a **Scheduled Sync** for recurring data refreshes (e.g., nightly)
* **Set Date Window**
  * Choose **Fixed Window** (a specific Start Date/End Date that stays the same on every run — useful for tracking availability of one specific campaign window over time)
  * or **Rolling Window** (a duration in days measured from Today, Tomorrow, or a custom date — useful for always seeing the next N days of availability).
  * Select whether you're identifying displays by Geopath ID or Display ID, and enter the relevant IDs.
  * Choose your destination (S3, Snowflake, BigQuery, etc.)
* **Review and Save Connection**
  * Review the connnection configuration and click on **Save**.

***

**Data Dictionary**

Availability Output

<table><thead><tr><th width="178">Field</th><th width="164">Data Type</th><th width="215">Description</th><th>Example</th></tr></thead><tbody><tr><td><code>`display_id`</code></td><td>string</td><td>Lamar's display identifier, returned when `unitIdType` is `display_ids`.</td><td><code>`072.002272`</code></td></tr><tr><td><code>`geopath_id`</code></td><td>int64</td><td>GeoPath spot identifier, returned when `unitIdType` is `geopath_ids`. Returned as a number, not a string.</td><td><code>`4060`</code></td></tr><tr><td><code>`display_type`</code></td><td>string</td><td>Lamar's own display/media format. Values are Lamar's real inventory categories (e.g. "Poster", "Jr Bulletin"), not a fixed enum.</td><td><code>`Poster`</code></td></tr><tr><td><code>`availability`</code></td><td>string (enum)</td><td>Availability status for the requested date window. Confirmed values: `AVAILABLE`, `PARTIALLY_AVAILABLE`, `UNAVAILABLE`.</td><td><code>`PARTIALLY_AVAILABLE`</code></td></tr><tr><td><code>`cost`</code></td><td>float64</td><td>Estimated cost for the display over the requested date window.</td><td><code>`996.43`</code></td></tr><tr><td>`<code>currency</code>`</td><td>string</td><td>Currency code for `cost`.</td><td><code>`USD`</code></td></tr><tr><td><code>`days_available`</code></td><td>array&#x3C;string> or null</td><td>Individual dates within the requested window the display is open. Only populated when `availability` is `PARTIALLY_AVAILABLE`; `null` for `AVAILABLE`/`UNAVAILABLE`.</td><td><code>`["2026-08-01", "2026-08-02", "2026-08-31"]`</code></td></tr></tbody></table>

***

For more information on the Lamar Extended Availability API, please review the [Lamar API documentation](https://anypoint.mulesoft.com/exchange/portals/lamaradvertising/7efeacc7-5d72-46dc-8249-c89f05c1bea1/sales-exp-api/minor/1.2/).
