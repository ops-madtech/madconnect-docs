# Outfront Inventory Availability

### Outfront – Inventory Availability

MadConnect’s **Outfront – Inventory Availability** connector enables teams to seamlessly pull up-to-date **inventory data** from Outfront’s planning systems into their own data environments. This source connector powers media planning.

***

**Connector Overview**

| Field                 | Description                                                                                                                 |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Connector Type**    | Source                                                                                                                      |
| **Data Type**         | Inventory Availability & Units                                                                                              |
| **Description**       | Retrieves Outfront inventory data including unit-level details, availability status, media formats, rates, and impressions. |
| **Primary Use Cases** | Media planning, forecasting, and powering AI-assisted inventory recommendations.                                            |
| **Supported Actions** | Read                                                                                                                        |

***

**Platform Prerequisites**

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

**Filters & Parameters**

**Step 1 Connection Configuration** This step allows you to define the filter criteria for the data

| Filter           | Description                                                                                                                                                                   | Example                    |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- |
| **Market Name**  | Geographic market or DMA to filter inventory                                                                                                                                  | `New York`, `Los Angeles`  |
| **Media Format** | Format category such as Poster, Bulletin, Digital Display                                                                                                                     | `Digital Display`          |
| **Unit ID Type** | The connector allows users to request data for specific ad units by either the Outfront ID (OMS), Geopath Spot ID or the Geopath Frame ID. Select the ID type to be utlilized | `Geopath - Frame`          |
| **Unit IDs**     | Once a Unit ID Type is selected, add your IDs as a comma delimited list                                                                                                       | `3042578,3042580,30842554` |
| **Digital**      | A flag to filter for Digital ad units. Yes = Digital, No = Static, Empty (null) = All                                                                                         | `Yes`                      |

**Step 3: Run Configuration** This step allows you to set the configuration for each data pull

| Configuration            | Description                                                                                       | Available Options                         |
| ------------------------ | ------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| **Load Type**            | Configure incremental loading to only process new or updated records on subsequent runs.          | `Full Load`                               |
| **Transfer Type**        | Configure whether the pull should happen manually or on a scheduled basis                         | `Manual Transfer` or `Scheduled Transfer` |
| **Window Configuration** | Set a fixed date range or a forward rolling date range. Avails is available for the next 350 days | `Fixed Window` or `Rolling Window`        |

* Multiple filters can be combined. Empty filters return all available data for authorized markets.
* Outfront data is sold on a Monday to Sunday schedule. Please select a Monday start date and Sunday end date
* Flight start lead time is 21 days for static units and 7 days for digital units
* Maximum availability period is a rolling 12 months

***

**Setting Up the Connector in MadConnect**

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

**Important Notes**

1. Availability data reflects a **point-in-time snapshot**; schedule recurring pulls to maintain freshness.
2. Large pulls can be optimized by applying filters per market or format.
3. Some markets or formats may return partial data depending on Outfront account permissions. Inquiry quantity limitation is 100 per market
4. Rate and impression values are subject to Outfront’s data refresh cadence.
5. These are some standard planning metrics and their calculations
   * **Weekly Net Rate** = net\_rate/4
   * **CPM** = Weekly Net Rate/(imp\_18p/1000)
   * **Cost** = Weekly Net Rate \* # of weeks

***

**Data Dictionary**

### `inventory_units_data.parquet`

| Field                    | Data Type         | Description                                                              | Example                                                                                         |
| ------------------------ | ----------------- | ------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------- |
| `unique_id`              | string            | Market-prefixed unit identifier used across OMS joins.                   | `AT E9210251`                                                                                   |
| `geopath_spot_id`        | int64             | GeoPath spot identifier for the face.                                    | `14920757`                                                                                      |
| `geopath_frame_id`       | int64             | GeoPath frame identifier (often the same as spot for single-face units). | `14920757`                                                                                      |
| `location_description`   | string            | Street-level placement description.                                      | `Hwy 5 400 ft N/O Douglas Blvd E/S`                                                             |
| `media_format`           | string            | Outfront media classification (e.g., Bulletins, Junior Posters).         | `Junior Posters`                                                                                |
| `latitude`               | float64           | Latitude in decimal degrees (WGS84).                                     | `33.725994`                                                                                     |
| `longitude`              | float64           | Longitude in decimal degrees (WGS84).                                    | `-84.762035`                                                                                    |
| `is_digital`             | string (`Y`/`N`)  | Flag indicating digital playback capability.                             | `N`                                                                                             |
| `unit`                   | string            | Pure unit code without market prefix.                                    | `E9210251`                                                                                      |
| `market`                 | string            | Outfront market name.                                                    | `Atlanta`                                                                                       |
| `size`                   | string            | Physical face size.                                                      | `5'x11'`                                                                                        |
| `illumination`           | string (`Y`/`N`)  | Whether the face is illuminated.                                         | `N`                                                                                             |
| `illumination_hours`     | string            | Stated lighting schedule.                                                | `12 HRS`                                                                                        |
| `facing`                 | string            | Cardinal/ordinal facing.                                                 | `S`                                                                                             |
| `primary_read`           | string            | Traffic read direction; values Left, Right, Parallel, Center.            | `Parallel`                                                                                      |
| `no_faces`               | int64             | Number of faces (string digit; current data only '1').                   | `1`                                                                                             |
| `net_install_cost`       | int64             | Install cost stored as numeric string (e.g., '0','525').                 | `0`                                                                                             |
| `net_copy_change_cost`   | float64           | Net copy change cost per cycle (USD).                                    | `29.75`                                                                                         |
| `net_production_cost`    | float64           | Net production cost (USD).                                               | `46.75`                                                                                         |
| `production_forced`      | string (`Y`/`N`)  | Indicates whether production is mandatory.                               | `N`                                                                                             |
| `net_rate`               | int64             | Base net rate per `rate_cycle` (USD).                                    | `310`                                                                                           |
| `rate_cycle`             | string            | Billing cycle label (e.g., 4-Week).                                      | `4-Week`                                                                                        |
| `digital_spot_length`    | int64             | Spot length for digital inventory (seconds).                             | `0`                                                                                             |
| `number_of_spots`        | int64             | Slot count in a digital loop; zero for static.                           | `0`                                                                                             |
| `image_url`              | string (URL)      | Hosted location photo for presentations.                                 | `https://mapweb.outfront.com/handlers/image.ashx?t=photo&m=AT&p=E9210251`                       |
| `spec_sheet_url`         | string (URL)      | Production/spec sheet link.                                              | `https://mapweb.outfront.com/production.ashx?m=AT&s=E9210251&pdf=yes`                           |
| `imp_18p`                | int64             | Weekly impressions 18+ (Geopath).                                        | `60155`                                                                                         |
| `marketing_info`         | string            | Narrative blurb for sales context.                                       | `This highly visible bulletin is located just west of Ross Circle on Stone Mountain Highway...` |
| `production_spec`        | string            | Production specification code.                                           | `JP104G-P`                                                                                      |
| `package_name`           | string            | Internal package grouping identifier (blank if standalone).              | `WEHOPK2`                                                                                       |
| `restrictions`           | string            | Sales restrictions or category carveouts.                                | \`Alcohol-P.O.                                                                                  |
| `enabled_for_automation` | string (`Y`/`N`)  | Can be bought on an impression basis                                     | `N`                                                                                             |
| `active_date`            | string (ISO date) | Unit go-live date in YYYY-MM-DD.                                         | `2009-01-01`                                                                                    |
| `inactive_date`          | string (ISO date) | Unit sunset date; empty string when active.                              | \`\`                                                                                            |

### `inventory_avails_data.parquet`

| Field       | Data Type                      | Description                                                                                                                                                | Example                                                       |
| ----------- | ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| `unique_id` | string                         | Matches the unit master `unique_id` for joins.                                                                                                             | `AT E9210251`                                                 |
| `dates`     | struct\<YYYY-MM-DD: string, …> | Wide struct with one string column per calendar day (e.g., `2026-03-02: string`) storing the daily availability state (`"Available"` / `"Not Available"`). | `{ "2026-03-02": "Available", "2026-03-03": "Available", … }` |

Use the shared `unique_id` to join this avail calendar back to `inventory_units_data.parquet` when producing media plans or utilization dashboards.
