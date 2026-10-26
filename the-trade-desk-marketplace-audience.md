# The Trade Desk Marketplace - Audience

MadConnect connects to The Trade Desk (TTD) third-party data APIs, letting a data provider publish its audience segments to The Trade Desk Data Marketplace, where advertisers and agencies can find and buy them. MadConnect creates each segment and its rate in TTD when they are missing, then uploads, removes or expires the segment's members.

## Connector Overview

| Field             | Value                                                                                                                               |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| Connector Type    | Destination                                                                                                                         |
| Data Type         | Audience Activation                                                                                                                 |
| Description       | Publish audience segments and their members to The Trade Desk third-party data marketplace                                          |
| Primary Use Case  | Data providers making their audiences available to TTD buyers, with segments, rates and members kept up to date from their own data |
| Supported Actions | Add, Create, Remove, Opt-out, Delete                                                                                                |

## Authenticate The Trade Desk Account

Key-based (API token) authentication, not OAuth. Required field: API Token. Tokens last one week to one year and can be revoked anytime; an expired/revoked token stops the transfer at its first request with TTD's error, nothing created/uploaded. The token is shown only once — copy it before closing the dialog; if lost, generate a new one in TTD and re-enter it here.

## Access Requirements

* A TTD data provider account, with its Provider ID (always required)
* A Brand registered under the provider, if your TTD setup assigns rates by brand (one MadConnect connection per brand). Simpler, single-brand accounts may not need this — ask your TTD account manager if you're not sure
* An API Token created by a user with access to the data provider account
* Outbound HTTPS access to api.thetradedesk.com and The Trade Desk Data API

## Know These Before Configuring

* Your Provider ID (always required)
* Your Brand ID, if your account uses brand-scoped rates — not every client needs one; check with your TTD account manager
* Whether your segments are Custom or Syndicated (taxonomy type)
* Rate level — System, Partner or Advertiser — and, for Partner/Advertiser, the IDs the rate applies to
* The rate: Percent of media cost and the CPM cap amount (USD)

## Steps to Obtain an API Token

1. Sign in with your TTD account (a user with access to your data provider account).
2. Open the OpenTTD Access Management app.
3. Click Generate Token / Key.
4. Enter a descriptive name (e.g. MadConnect) and select Platform API as the Application.
5. Choose the token's lifetime (one week to one year).
6. Click Save, then copy the token from the confirmation message.

The token is shown once only. If lost or expired, generate a new one and update it in MadConnect.

## Configure Connector

1. Add Platform — My Platforms → Add Platform → The Trade Desk – Marketplace Audience → Configure.
2. Authenticate — paste API Token, click Save. Status shows Configured.
3. Create Connection — Connections → Create Connection. Select Source (holding the audience data). Select Destination: The Trade Desk – Marketplace Audience. Fill in Destination Configuration fields (see Connection Fields). Schema Mapping: map file columns to the audience schema (see Data Dictionary); if you map columns, map all of them since unmapped columns are not sent.
4. Run Configuration — Load Type: Full Load (sends all data every time) or Incremental Load (only new/updated records on later runs). Transfer Type: Manual Transfer (once) or Scheduled Transfer (regular schedule, with Schedule Configuration: Initial Sync start time, Timezone, Sync Frequency/unit). Click Next to Review.
5. Review and Save Connection.

## Connection Fields

Each connection publishes to one Provider ID. Rate type (Hybrid) and currency (USD) are fixed by the connector.

| Field                 | Description                                                                                                                                                                                                                                                     |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Provider ID           | Required. Your TTD data provider ID                                                                                                                                                                                                                             |
| Brand ID              | Required only if your TTD account uses brand-scoped rates; leave blank for simpler, single-brand setups. When used, must be registered under the provider, otherwise TTD rejects the rate (reported as an error) while the segment and its users are still sent |
| Taxonomy type         | Required. Custom or Syndicated                                                                                                                                                                                                                                  |
| Rate level            | Required. System, Partner or Advertiser. Syndicated taxonomy uses System; Custom uses Partner or Advertiser                                                                                                                                                     |
| Partner ID            | Shown/required when Rate level is Partner. One or more Partner IDs, each gets the same rate                                                                                                                                                                     |
| Advertiser ID         | Shown/required when Rate level is Advertiser. One or more Advertiser IDs, each gets the same rate                                                                                                                                                               |
| Percent of media cost | Required. Whole percentage (e.g. 20 = 20%), above 0 up to 100                                                                                                                                                                                                   |
| CPM cap amount (USD)  | Required. The CPM cap of the rate, in USD                                                                                                                                                                                                                       |
| Sub-Provider ID       | Optional. Only for clients with an onboarder setup managing multiple brands under one Provider ID. Set on new segments only; cannot be changed afterward                                                                                                        |
| Default TTL (days)    | Optional. How long a member stays when the file has no ttl. Default 90, maximum 180                                                                                                                                                                             |

## Supported Actions

Each input row carries an action. A row without one is skipped and reported.

| Action           | What happens                                                                                                                                         |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Add / Create     | Creates the segment and its rate if they don't exist yet, then uploads the row's users. A create row with no users only creates the segment          |
| Remove / Opt-out | Removes the row's users from the segment. Needs a segment\_id                                                                                        |
| Delete           | Marks the segment as not for sale and removes the users listed in the file. Needs a segment\_id. Users not in the file stay until their own TTL ends |
| Replace          | Not supported; rows reported as errors, rest of file still sent                                                                                      |

## Data Dictionary

Input files use the MadConnect Standard Audience Schema (v1.3). Column names are not case-sensitive.

| Column                 | TTD Field           | Notes                                                                          |
| ---------------------- | ------------------- | ------------------------------------------------------------------------------ |
| segment\_id            | Segment ID          | Recommended. Required for remove, opt-out and delete                           |
| segment\_name          | Display name        | Full path, e.g. In-Market > Travel > Flights. Required to create a segment     |
| segment\_description   | Description         | Optional, trimmed to 256 characters                                            |
| action                 | —                   | add, create, remove, opt-out or delete                                         |
| ttl                    | TTL                 | Days a member stays, whole days, capped at 180. Empty/invalid uses Default TTL |
| tdid                   | TDID                | The Trade Desk ID                                                              |
| daid, maid, idfa, gaid | DAID                | Raw (not hashed) device IDs                                                    |
| uid2, uid2\_token      | UID2, UID2 token    | Sent exactly as provided (case-sensitive)                                      |
| euid, euid\_token      | EUID, EUID token    | Sent exactly as provided (case-sensitive)                                      |
| rampid, idl            | RampID              | Must be the RampID LiveRamp maps for The Trade Desk                            |
| id5, netid, firstid    | ID5, netID, FirstId | Europe only                                                                    |

Columns TTD does not accept (email, phone, hashed IDs, other traits) are not sent; a row with no accepted ID is skipped and counted.

## Important Notes

* Always send segment\_id: a row with only segment\_name gets a newly generated segment ID, so a file without segment\_id creates a new segment every time it runs.
* Approval takes time: TTD reviews new segments (\~2 hours) and processes new rates at end of day (UTC). MadConnect uploads users right away; a new rate is reported as submitted and pending.
* Visibility to buyers: a segment appears in the marketplace only once approved, buyable, has an approved rate, and has more than 1,000 received IDs or more than 10 active IDs.
* Low rates: a CPM cap under 0.25 USD or percent of media cost under 10% goes to TTD's manual review instead of auto-approval.
* Segment names cannot contain a tab, ', " or ^, have an empty level, or exceed 1,024 characters.
* Duplicate users: if the same user appears more than once for a segment in one file, only the last row is sent.
* Partial success: rejected/unsendable rows are reported with the exact reason; rest of file still delivered.
* Existing segments are not renamed by a changed segment\_name in a later file.
* Full Load re-sends every member each run (renewing TTL); with Incremental Load, members not re-sent leave the segment when their TTL ends (default 90 days).

For more information, see the official The Trade Desk third-party data management guide.
