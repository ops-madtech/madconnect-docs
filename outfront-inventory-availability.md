# Outfront Inventory Availability

## Outfront – Inventory Availability&#x20;

MadConnect’s **Outfront – Inventory Availability** connector enables teams to seamlessly pull up-to-date **inventory data** from Outfront’s planning systems into their own data environments. This source connector powers media planning, reporting, and optimization workflows.

***

#### Connector Overview

| Field                 | Description                                                                                                                 |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Connector Type**    | Source                                                                                                                      |
| **Data Type**         | Inventory Availability & Units                                                                                              |
| **Description**       | Retrieves Outfront inventory data including unit-level details, availability status, media formats, rates, and impressions. |
| **Primary Use Cases** | Media planning, forecasting, performance analysis, and powering AI-assisted inventory recommendations.                      |
| **Supported Actions** | Read                                                                                                                        |

***

#### Platform Prerequisites

Before configuring the connector in MadConnect, ensure you have:

1. **Authentication**
   * Tenant ID
   * Client ID
   * Client Secret
   * Scope (access permissions for Outfront data)

These credentials authenticate the connector to Outfront’s inventory data service. MadConnect stores them securely and manages token refresh.

**Access Requirements**

1. Active Outfront partner account with data-access entitlement
2. Network permissions allowing outbound HTTPS connections to the Outfront service

***

#### Filters & Parameters

The connector supports flexible filtering so users can narrow the returned data set before ingestion. Common filters include:

| Filter                    | Description                                               | Example                     |
| ------------------------- | --------------------------------------------------------- | --------------------------- |
| **Market Name**           | Geographic market or DMA to filter inventory              | `New York`, `Los Angeles`   |
| **Media Format**          | Format category such as Poster, Bulletin, Digital Display | `Digital Display`           |
| **Start Date / End Date** | Date range to restrict available inventory window         | `2025-04-01` – `2025-04-30` |

Multiple filters can be combined. Empty filters return all available data for authorized markets.

***

#### Setting Up the Connector in MadConnect

1. **Add Platform**
   * Go to **My Platforms → Add Platform**
   * Select **Outfront – Inventory Availability (Source)** and click **Configure**
2. **Authenticate**
   * Enter **Tenant ID**, **Client ID**, **Client Secret**, and **Scope**
   * Click **Connect**; MadConnect validates access automatically
   * Once connected, status shows **Configured**
3. **Create Connection**
   * Choose **Outfront – Inventory Availability** as the source
   * Optionally define filters (Market, Format, Dates, etc.)
   * Choose your destination (S3, Snowflake, BigQuery, etc.)
4. **Run or Schedule**
   * Run a **Manual Pull** for one-time data retrieval
   * Or configure a **Scheduled Sync** for recurring data refreshes (e.g., nightly)

***

#### Important Notes

1. Availability data reflects a **point-in-time snapshot**; schedule recurring pulls to maintain freshness.
2. Large pulls can be optimized by applying filters per market or format.
3. Some markets or formats may return partial data depending on Outfront account permissions.
4. Rate and impression values are subject to Outfront’s data refresh cadence.
