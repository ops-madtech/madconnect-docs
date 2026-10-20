# DoubleVerify - Reporting Connector

MadConnect connects to the DoubleVerify Report Data API, letting you pull ad verification data — viewability and fraud/invalid traffic (IVT) measurement — directly from a client's DoubleVerify Pinnacle account into your own data warehouse.

## Connector Overview

| Field             | Description                                                                                      |
| ----------------- | ------------------------------------------------------------------------------------------------ |
| Connector Type    | Source                                                                                           |
| Data Type         | Reporting                                                                                        |
| Description       | Retrieve viewability and fraud/IVT verification report data from a DoubleVerify Pinnacle account |
| Primary Use Case  | Ad verification and measurement reporting, blending DoubleVerify data with other ad platforms    |
| Supported Actions | Read                                                                                             |

## Prerequisites

### Authentication

This connector uses a static **Access Token**. There's no sign-in flow and no OAuth — you generate the token yourself in the DoubleVerify Pinnacle UI and paste it into MadConnect.

The token is scoped to whichever **Reporting Programs** you select when creating it, so DoubleVerify resolves the correct account automatically — there's no separate Account ID or Advertiser ID to enter.

### Access Requirements

* Pinnacle access, with your account assigned to at least one DV Reporting Program
* An **Access Token** created in Pinnacle, scoped to the Reporting Program(s) you want to report on
* Outbound HTTPS access to `data-reporting.doubleverify.com`

### Know These Before Configuring

* The **Metrics** you want (at least one required)
* The **Dimensions** you want to break the data down by (optional)
* Your **Time Zone** — must be a valid IANA zone

## Steps to Obtain an Access Token

1. Sign in at [pinnacle.doubleverify.com](https://pinnacle.doubleverify.com)
2. Go to **Analytics** → **Data API**
3. Click **Create Token**, and select the **Reporting Programs** the token should cover
4. Click **Create**, then **Copy**

The token is shown once only — DoubleVerify cannot re-display it. If it's lost, create a new one and re-enter it in MadConnect.

## Setting Up the Connector in MadConnect

{% stepper %}
{% step %}
### Add Platform

* Go to **My Platforms** → **Add Platform**
* Select **DoubleVerify (Source)** and click **Configure**
{% endstep %}

{% step %}
### Authenticate

* Paste your **Access Token**
* Click **Save** — status shows **Configured**
{% endstep %}

{% step %}
### Create Connection

* Choose **DoubleVerify** as the source
* Select the **Metrics** you want (at least one required)
* Select the **Dimensions** you want (optional)
* Choose your **Time Zone**
* Choose your destination (S3, Snowflake, BigQuery, etc.)
{% endstep %}

{% step %}
### Set Date Range

Choose Fixed Window (enter explicit Start/End dates) or Rolling Window (Loop Back) — both are fully supported. DoubleVerify doesn't enforce a fixed lookback limit — the range is bounded dynamically by however much data DoubleVerify has processed: a start date beyond that available period is rejected, while an end date beyond it is not rejected — the report simply returns data up to the latest available point.
{% endstep %}

{% step %}
### Run or Schedule

* Run a **Manual Pull** for one-time data retrieval
* Or set up a **Scheduled Sync** for recurring pulls (e.g. daily)
{% endstep %}

{% step %}
### Review and Save Connection

* Review the configuration and click **Save**
{% endstep %}
{% endstepper %}

## Request Type

Every report request is submitted under a DoubleVerify **Request Type** (Standard, Proprietary Media Gardens, GroupM Billable, or DV Authentic Attention). This isn't a field you set — MadConnect currently submits every DoubleVerify connection under **Standard**, the request type used for plain tag-based viewability and fraud measurement. \[confirm: whether other Request Types will be exposed/supported in a future release]

## Data Dictionary

The exact Metrics and Dimensions available depend on your DoubleVerify account and are pulled from DoubleVerify per Request Type.

### Sample Report Output

| Mobile OS | Authentic Q1 Fully On-Screen Rate |
| --------- | --------------------------------- |
| iOS       | 0.87                              |
| Android   | 0.79                              |

Each Metric and Dimension you select becomes a column in the output CSV.

## Additional Considerations

* **Report generation takes time**: DoubleVerify builds reports asynchronously. MadConnect handles the wait automatically — no manual action needed.
* **Partial data**: if your selected date range includes a gap or missing data, DoubleVerify still returns the report but flags it as partial.
* **Time Zone**: must be a valid IANA zone recognized by DoubleVerify for the connector's Request Type — an unsupported zone will be rejected.
* **Read-only**: this connector only reads data out of DoubleVerify. It never writes or changes anything in the client's account.

For more information, see the official [DoubleVerify Report Data API documentation](https://developer.doubleverify.com/docs/client-integrations/extensions/report-data-api).
