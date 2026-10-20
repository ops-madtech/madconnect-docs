# AdsWizz - Reporting Connector

MadConnect connects to AdsWizz's Domain API, letting you pull podcast and newsletter ad performance data directly from AdsWizz into your own data warehouse. See how your campaigns performed — broken out by whichever dimension you choose, such as advertiser or campaign — for a chosen date window, and keep that data automatically synchronized so your team always has an up-to-date view of ad performance.

## Connector Overview

| Field             | Description                                                                                                                    |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| Connector Type    | Source                                                                                                                         |
| Data Type         | Reporting                                                                                                                      |
| Description       | Retrieve ad performance data from AdsWizz's Domain API, dimensioned by the metrics, breakdown, and time granularity you choose |
| Primary Use Case  | Podcast and newsletter ad performance reporting, blending AdsWizz data with other platforms                                    |
| Supported Actions | Read                                                                                                                           |

## Prerequisites

### Authentication

AdsWizz uses key-based (API key) authentication, not OAuth.

Required field:

* **API Key**

The key is issued by AdsWizz support — request it through your AdsWizz partner or your MyAdsWizz account. There is no sign-in step and no scopes to approve; the key itself determines what your account can access.

There is no token expiration or refresh cycle for this key. It continues to work until you replace it in AdsWizz or ask AdsWizz to revoke it — if that happens, generate a new one and update it here.

### Access Requirements

* Make sure the API key's account has access to the Agency Identifier you enter. If it does not, report requests fail.

### Know These Before Configuring

We recommend confirming the following before setting up the connection:

* Your **Agency Identifier** — the numeric agency ID for your account, found in MyAdsWizz.
* **The Metrics and Dimension you want in the report.** Metrics — pick one or more (for example, impressions). Dimension — pick exactly one breakdown, such as Advertiser or Campaign. Available metrics and dimensions depend on your AdsWizz account and environment.
* Your **Granularity** — the time bucket for the report: Hour, Day, Week, Month, Year, or All.
* **Your intended date range.** Rules:
  * Reports can look back up to 15 months.
  * MadConnect converts your Start Date and End Date to UTC before requesting the report. If you enter a date with a timezone offset, that offset is applied — for example, a Start Date of 2026-08-09T00:00:00+05:30 is sent to AdsWizz as the equivalent UTC instant. If you enter a plain calendar date with no time or offset, it is treated as UTC.
  * End Date is inclusive — its full day is included in the results.
  * The separate Timezone setting only controls how results are grouped into day/week/month buckets — it does not shift the reporting window itself.

## Setting Up the Connector in MadConnect

{% stepper %}
{% step %}
### Add Platform

* Go to **My Platforms** → **Add Platform**
* Select the **AdsWizz** tile and click **Configure**
{% endstep %}

{% step %}
### Authenticate

* Open the **Configuration** tab
* Enter your **API Key** — the key AdsWizz issued you
* Enter the **Agency Identifier** — the numeric agency ID from MyAdsWizz
* Click **Save** — status shows **Configured**
{% endstep %}

{% step %}
### Create Connection

* Choose **AdsWizz** as the source
* Select one or more **Metrics** and exactly one **Dimension** from the dropdowns — at least one metric is required, and the dimension is required; an empty selection fails the run
* Choose your **Granularity** — Hour, Day, Week, Month, Year, or All
* Set **Timezone** (optional) — used to bucket results by day/week/month; defaults to UTC if left blank
* Choose your destination (S3, Snowflake, BigQuery, etc.)
{% endstep %}

{% step %}
### Set Date Range

Enter a Start Date and End Date, keeping the Start Date within the last 15 months.
{% endstep %}

{% step %}
### Run or Schedule

* Choose **Manual Transfer** (run once, immediately) or **Scheduled Transfer** (run automatically on a recurring schedule)
{% endstep %}

{% step %}
### Review and Save Connection

* Review the configuration and verify the platform status shows **Configured** under My Platforms
{% endstep %}
{% endstepper %}

## Data Dictionary

AdsWizz does not publish a single fixed column layout for this report — the columns returned always match the Metrics, Dimension, and Granularity you selected during configuration. There is no separate metric/dimension catalog to browse ahead of time; the choices available in the Metrics and Dimension dropdowns come directly from your AdsWizz account.

| Field         | Description                                                                                                                                          |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `__time`      | The time bucket for the row, at the Granularity you chose (Hour, Day, Week, Month, Year, or All).                                                    |
| `<metric>`    | One column per Metric you selected, named exactly as it appears in the Metrics dropdown (for example, impressions).                                  |
| `<dimension>` | The single column for the Dimension you selected, named exactly as it appears in the Dimension dropdown, holding that dimension's value for the row. |

Since dimensions and metrics vary by AdsWizz account, always check the resulting file against your own connection's Metrics and Dimension selections rather than a fixed schema.

## Additional Considerations

* **Dates are converted to UTC, not shifted by your local calendar.** Enter dates with a timezone offset if you want a specific local moment reported — MadConnect converts it to the equivalent UTC instant before sending it to AdsWizz.
* **One dimension per connection.** AdsWizz's reporting API breaks a report out by exactly one dimension at a time (plus your chosen time granularity). To see data broken out by more than one dimension — for example, both Advertiser and Campaign — set up one connection per dimension.
* **Every report is scoped to one agency.** Data from other agencies under the same AdsWizz account is not included; set up a separate connection per Agency Identifier if you need to report on more than one.
* **No token expiration.** The API key does not expire or need periodic refresh.
* **Report generation takes time.** AdsWizz builds reports asynchronously — this can take anywhere from a few seconds to several minutes depending on the size of your date window. MadConnect handles the wait automatically; no manual action is needed.
* **Column layout follows your selections.** Because AdsWizz doesn't publish one fixed report layout, the exact columns you get always mirror the Metrics and Dimension chosen in Configuration.
* **Reports use your account's default AdsWizz environment.** This connector queries AdsWizz's default reporting environment (AUDIOSERVE) for your account. \[confirm: support path/contact for accounts on a non-default AdsWizz environment]

For more information on the underlying API, please review [AdsWizz's Domain API documentation](https://docs.adswizz.com/domain-api/v9/).
