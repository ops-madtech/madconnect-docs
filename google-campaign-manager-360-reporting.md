# Google Campaign Manager 360 - Reporting

MadConnect enables seamless integration with the Google Campaign Manager 360 (CM360) Reporting API, allowing you to pull ad serving and conversion reporting data — like impressions, clicks, media cost, and Floodlight conversions — directly from a client's CM360 account into your own data warehouse. Gain valuable insights to power dashboards and reporting alongside your other data sources.

## Connector Overview

| Field                  | Description                                                                                              |
| ---------------------- | -------------------------------------------------------------------------------------------------------- |
| Connector Type         | Source                                                                                                   |
| Data Type              | Reporting                                                                                                |
| Description            | Retrieve Standard (delivery) and Floodlight (conversion) report data from a Campaign Manager 360 account |
| Primary Use Case       | Campaign delivery and conversion reporting, blending CM360 data with other media and analytics sources   |
| Supported Report Types | Standard, Floodlight                                                                                     |
| Supported Actions      | Read                                                                                                     |

## Prerequisites

### Authentication

* A Google account that has access to the client's Campaign Manager 360 account.
* MadConnect connects using Google sign-in (OAuth 2.0). The user signs in once and approves access to view and manage CM360 reports. No keys or passwords are shared with MadConnect.
* **Manual entry option:** you can instead use Manual OAuth by providing your own Client ID, Client Secret, Access Token, and Refresh Token from a Google Cloud OAuth client.
* **Token expiry:** Google access tokens expire after about **1 hour**. MadConnect refreshes them automatically using the refresh token, so no action is needed between runs.

### Access Requirements

* The Google account must have a CM360 user profile on the account, with a user role that allows creating and running reports. Without this, calls will fail with a permission error.
* If API access is restricted on the CM360 account, ask the CM360 administrator to allow it.
* Outbound HTTPS access to `dfareporting.googleapis.com`.
* A MadConnect workspace with a destination configured (S3, Snowflake, BigQuery, etc.).

### Know These Before Configuring

* The numeric CM360 **User Profile ID** you want to report as. This is the user profile — not the Account ID or an Advertiser ID.
* The **Report Type**: Standard (delivery metrics such as impressions, clicks, media cost) or Floodlight (conversion activity).
* For Floodlight reports only: the numeric **Floodlight Configuration ID** of the advertiser.
* The **Dimensions** (e.g. date, campaign, placement) and **Metrics** (e.g. impressions, clicks, total conversions) you want in the report.

## Steps to Obtain Credentials

With Sign in with Google, no credentials need to be created. Access is granted by signing in when adding the platform in MadConnect. The client (or their CM360 administrator) only needs to confirm access and collect the IDs below. Step 4 applies only when using Manual OAuth.

1. **Confirm CM360 Access** — Sign in to campaignmanager.google.com with the Google account that will be used in MadConnect; confirm reports can be viewed under Reporting & Attribution.
2. **Find the User Profile ID** — Open the userProfiles.list API reference page, use 'Try this method' signed in with the same Google account, click Execute; each item in the response shows a profileId and the accountName it belongs to. Copy the profileId for the client's CM360 account (or ask the CM360 administrator).
3. **Find the Floodlight Configuration ID** (Floodlight reports only) — In CM360, open the advertiser that owns the Floodlight tags, go to Floodlight → Configuration, copy the numeric Floodlight Configuration ID shown.
4. **Generate OAuth Credentials** (Manual OAuth only) — In console.cloud.google.com, select/create a project and enable the Campaign Manager 360 API; under APIs & Services → Credentials → Create Credentials → OAuth client ID, note the Client ID and Client Secret; authorize the Google account with scope https://www.googleapis.com/auth/dfareporting; exchange the authorization code and copy the Access Token and Refresh Token.

## Setting Up the Connector in MadConnect

{% stepper %}
{% step %}
### Add Platform

Go to My Platforms → Add Platform. Select Campaign Manager 360 Reporting (Source) and click Configure.
{% endstep %}

{% step %}
### Authenticate

Click Sign in with Google and choose the Google account that has CM360 access. Review the requested permission and click Allow. One-Time OAuth Link: if someone else needs to sign in, click the copy icon to create a single-use link (expires after one successful use). Alternatively, select Manual OAuth and enter Client ID, Client Secret, Access Token and Refresh Token, then Save. Status shows Configured.
{% endstep %}

{% step %}
### Create Connection

Choose Campaign Manager 360 Reporting as the source. Enter the numeric User Profile ID. Select the Report Type: Standard or Floodlight. For Floodlight, enter the Floodlight Configuration ID. Select Dimensions (e.g. date, campaign\_name, placement\_name) and Metrics (e.g. impressions, clicks, media\_cost). Choose your destination.
{% endstep %}

{% step %}
### Set Date Window

Choose Fixed Window (specific Start/End Date) or Rolling Window (duration in days from Today/Yesterday).
{% endstep %}

{% step %}
### Run or Schedule

Run a Manual Pull, or configure a Scheduled Sync.
{% endstep %}

{% step %}
### Review and Save Connection

Review the configuration and click Save.
{% endstep %}
{% endstepper %}

## Configuration Fields

| Field                       | Required        | Description                                                           |
| --------------------------- | --------------- | --------------------------------------------------------------------- |
| User Profile ID             | Yes             | Numeric CM360 user profile ID used to create and run the report       |
| Report Type                 | Yes             | Standard (delivery) or Floodlight (conversions). Defaults to Standard |
| Floodlight Configuration ID | Floodlight only | Numeric Floodlight configuration of the advertiser                    |
| Dimensions                  | No              | Fields to group the report by                                         |
| Metrics                     | Yes             | At least one metric                                                   |
| Date Window                 | Yes             | Fixed Window or Rolling Window                                        |
| Destination                 | Yes             | Where the report is delivered                                         |
| Schedule                    | No              | Manual Pull or Scheduled Sync                                         |

## Data Dictionary

### Supported Fields

| Category                         | Fields                                                                                                                                                                                                        |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Time                             | date, week, month                                                                                                                                                                                             |
| Delivery dimensions              | advertiser\_name, advertiser\_id, campaign\_name, campaign\_id, placement\_name, placement\_id, site\_name, site\_id, ad\_name, ad\_id, creative\_name, creative\_id, creative\_type, platform\_type, country |
| Floodlight dimensions            | activity\_name, activity\_id, activity\_group, floodlight\_config\_id                                                                                                                                         |
| Delivery metrics (Standard only) | impressions, clicks, click\_rate, media\_cost, viewable\_impressions, measurable\_impressions, video\_plays, video\_views, video\_completions                                                                 |
| Conversion metrics               | total\_conversions, total\_conversion\_revenue, click\_through\_conversions, view\_through\_conversions, click\_through\_revenue, view\_through\_revenue                                                      |

### Sample Report Output (Standard)

| Column      | Example           | Notes            |
| ----------- | ----------------- | ---------------- |
| Date        | 2026-08-05        | date, YYYY-MM-DD |
| Campaign    | Fall Launch 2026  | campaign\_name   |
| Placement   | Homepage\_300x250 | placement\_name  |
| Impressions | 125430            | Whole number     |
| Clicks      | 842               | Whole number     |
| Media Cost  | 1250.40           | Account currency |

## Additional Considerations

* Report processing is async; MadConnect waits and downloads automatically (errors out if not ready within 45 minutes).
* MadConnect creates a report named MadConnect\_CM360\_STANDARD or MadConnect\_CM360\_FLOODLIGHT in the user's CM360 Reports list; safe to delete after a run. Avoid running two connections with the same Profile ID + Report Type simultaneously.
* Read-only: never changes campaigns, ads, Floodlight tags or other CM360 settings.
* Report summary lines and Grand Total row are stripped; '(not set)'/'(unknown)' delivered as empty values.
* Impressions/clicks/media cost/Active View/video metrics are Standard-only; Floodlight dimensions require a Floodlight report.
* CM360 API has daily request limits; MadConnect retries temporary errors/rate limits automatically.
* Reports use whole days from the date window; time of day not applied.
* Most-recent-day data may still be updating; Floodlight conversions can be credited after the original impression/click per the lookback window — use a Rolling Window for accuracy.
* Figures may differ slightly from the CM360 UI due to attribution timing/reporting delays.
* One User Profile ID per connection; create one connection per profile for multiple accounts.
* No data in range → run completes successfully with no results.

For more information, see the official Campaign Manager 360 API documentation, the Reports API reference (v5), the Creating and running reports guide, or the Campaign Manager 360 Help Center.
