# Roku Ads Reporting

MadConnect connects to the Roku Ads API, letting you pull campaign performance data — impressions, spend, video completion, household reach, attributed actions, and more — directly from a client's Roku Ads account into your own data warehouse.

## Connector Overview

| Field             | Description                                                                    |
| ----------------- | ------------------------------------------------------------------------------ |
| Connector Type    | Source                                                                         |
| Data Type         | Reporting                                                                      |
| Description       | Retrieve asynchronous performance report data from a Roku Ads account          |
| Primary Use Case  | Campaign performance reporting, blending Roku CTV data with other ad platforms |
| Supported Actions | Read                                                                           |

## Prerequisites

### Authentication

This connector uses OAuth. You'll sign in with a Roku Ads Manager account through MadConnect and approve access — no manual key entry needed for this part.

Unlike some platforms, Roku doesn't require a separate long-lived developer token — the OAuth grant is all you need.

**Manual entry option:** if you'd rather not use the sign-in flow, you can enter tokens directly — Client Id, Client Secret, Access Token, Refresh Token.

**Token expiry:** Roku access tokens expire every **24 hours**. MadConnect refreshes them automatically using the refresh token — no manual action needed, and no impact to scheduled runs.

### Access Requirements

* The signed-in Roku account must have at least the **Viewer** role (minimum needed to run reports and view account info) on the target ad account.
* **Every ad account must be individually granted access** to MadConnect's developer app — this is not automatic, even if multiple accounts sit under the same Roku organization. This is done in Roku Ads Manager under **Organization → Partners → Edit roles and ad accounts**, by adding the account with the Viewer role. If an account isn't added there, report requests against it will fail with a permission error even though sign-in succeeded.
* Outbound HTTPS access to `api.ads.roku.com`.

### Know These Before Configuring

* Your **Account UID** — found in Roku Ads Manager under **Ad accounts**, in the **ID** column.
* Whether you're reporting on one account or several — one connection can cover multiple accounts (see [Multiple Accounts](roku-ads-reporting.md#multiple-accounts)).
* The **Metrics** you want (at least one is required).
* The **Dimensions** you want to break the data down by (optional).
* If you plan to report on attributed actions (purchases, sign-ups, etc.), read the [Activity-Dependent Metrics](roku-ads-reporting.md#activity-dependent-metrics) note below before picking metrics.

## Setting Up the Connector in MadConnect

{% stepper %}
{% step %}
### Add Platform

* Go to My Platforms → Add Platform
* Select Roku Ads (Source) and click Configure
{% endstep %}

{% step %}
### Authenticate

* Click Connect to Roku Account and approve access, or check "Allow me to enter the tokens manually" and enter Client Id / Client Secret / Access Token / Refresh Token.
* Click Save — status shows Connected.
{% endstep %}

{% step %}
### Create Connection

* Choose Roku Ads as the source.
* Enter your Account UID (see [Multiple Accounts](roku-ads-reporting.md#multiple-accounts) below if reporting on more than one).
* Optionally enter a Campaign UID to scope the report to specific campaigns.
* Select the Metrics you want (at least one required).
* Select the Dimensions you want (optional).
* Choose your destination (S3, Snowflake, BigQuery, etc.).
{% endstep %}

{% step %}
### Set Date Range

Choose Fixed Window (enter explicit Start/End dates) or Rolling Window (Loop Back) (the platform recalculates the date range automatically at each scheduled run, based on your chosen loop-back period) — both are fully supported. Timezone is set once here and reused for every run — no separate timezone field elsewhere.
{% endstep %}

{% step %}
### Run or Schedule

* Run a Manual Transfer for one-time data retrieval.
* Or set up a Scheduled Transfer for recurring pulls.
{% endstep %}

{% step %}
### Review and Save Connection

* Review the configuration and click Save.
{% endstep %}
{% endstepper %}

## Multiple Accounts

One MadConnect connection can pull data for more than one Roku ad account:

* Enter multiple Account UIDs as a **comma-separated list** in the Account UID field (e.g. `PauioyuiotCh,PpopuiklJmEH`) — this returns one combined report covering all listed accounts in a single run, not separate runs per account.
* **Each account must be added under Organization → Partners → Edit roles and ad accounts first.** Belonging to the same Roku organization is not enough on its own — an account with no explicit Viewer-role grant there will cause the run to fail with a permission error, even if another account in the same connection works fine.
* Leaving Account UID blank pulls data for every account the connected developer app currently has access to.

## Data Dictionary

Full list of supported metrics and dimensions, with descriptions, is maintained by Roku here: [Roku Ads API — Metrics & Dimensions reference](https://developer.ads.roku.com/ads/reference/about-reports#metrics).

### Sample Report Output

| Column         | Example Value | Notes                                   |
| -------------- | ------------- | --------------------------------------- |
| date           | 09/01/2026    | Format as returned by Roku (MM/DD/YYYY) |
| campaign\_id   | j78kl90       | Text                                    |
| campaign\_name | Fall Promo    | Text                                    |
| impressions    | 120345        | Whole number                            |
| spend          | 1843.22       | Currency                                |
| cpm            | 15.32         | Currency                                |

Each column you select becomes a column in the output CSV, in the same order you selected Dimensions then Metrics.

### Activity-Dependent Metrics

If any of **actions**, **cpa**, **order\_value**, or **total\_unique\_actions** is selected under Metrics, **activity\_name must also be present under Dimensions** — MadConnect adds it automatically if it's missing. Without it, those columns come back empty rather than erroring, so this auto-add exists specifically to prevent silently blank data.

## Additional Considerations

* **Report generation takes time:** Roku builds reports asynchronously. Roku's documented SLA is roughly **1 hour**. MadConnect handles the wait automatically — no manual action needed.
* **Data latency:** impressions delivered in the last **8 hours** may not appear in a report yet. Historical date ranges are reliable.
* **Attribution window:** conversions can be attributed up to **14 days** after the impression, and are reported at the time of conversion, not the time of the original impression.
* **Lookback window:** a report can only go back **18 months** from today. Roku's own public documentation states 13 months — that figure is incorrect; 18 months is the real, live-confirmed limit.
* **No future dates:** the start date of a report can't be in the future — Roku rejects the request if it is.
* **Read-only:** this connector only reads data out of Roku Ads. It never writes or changes anything in the client's account.
* **No data in range:** if no records match your selected date range, account, or filters, the run completes successfully with an empty result rather than erroring.
* **Report file availability:** once ready, the report file is available to download for about **7 days** — this isn't officially documented by Roku, only observed from live testing, so treat it as a practical guideline rather than a guaranteed contract.

For more information, see the official [Roku Ads API documentation](https://developer.ads.roku.com/ads/reference/introduction).
