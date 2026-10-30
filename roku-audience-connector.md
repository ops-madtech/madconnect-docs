# Roku - Audience Connector

MadConnect connects to the Roku Ads API, letting you send first-party customer lists (hashed emails, phone numbers, mobile ad IDs and IP addresses) from your data warehouse into a client's Roku Ads account as custom audiences, ready to target in Roku CTV campaigns.

## Connector Overview

| Field             | Description                                                                  |
| ----------------- | ---------------------------------------------------------------------------- |
| Connector Type    | Destination                                                                  |
| Data Type         | Audience                                                                     |
| Description       | Create custom file audiences in a Roku Ads account, one audience per segment |
| Primary Use Case  | Targeting and retargeting known customers on Roku CTV                        |
| Supported Actions | Create (a new audience for each segment)                                     |

## Prerequisites

### Authentication

This connector uses OAuth. You'll sign in with a Roku Ads Manager account through MadConnect and approve access — no manual key entry needed.

Roku doesn't require a separate developer token — the OAuth grant is all you need.

**Token expiry:** Roku access tokens expire every 24 hours. MadConnect refreshes them automatically using the refresh token, about 10 minutes before they expire — no manual action needed, and no impact to scheduled runs.

### Access Requirements

* The signed-in Roku account must have a role on the target ad account that allows creating audiences.
* The ad account must be granted access to MadConnect's developer app in Roku Ads Manager under **Organization → Partners → Edit roles and ad accounts**. If the account isn't added there, audience uploads fail with a permission error even though sign-in succeeded.
* Outbound HTTPS access to `api.ads.roku.com`.

### Know These Before Configuring

* Your **Account UID** — found in Roku Ads Manager under **Ad accounts**, in the ID column. Audiences are created in this account.
* Your source data must have a **`segment_name`** column; it is mandatory. Each segment becomes one Roku audience with the same name.
* Each input file must contain **one segment** (one `segment_name` value). Rows with an empty `segment_name` are given the file's segment name.
* Each row needs at least one identifier: `email`, `email_sha256`, `phone`, `maid` or `ip`.
* Your source data must have an `action` column; it is mandatory. Roku can only create audiences, so only rows with `create` or `add` are sent. Rows with any other action (`remove`, `delete`, `opt-out`, empty) are skipped.
* Segment names must be **new** — Roku requires unique audience names and can't add users to an audience that already exists.
* Roku needs about **1,000 matched households** for an audience to be usable in campaigns.

## Setting Up the Connector in MadConnect

{% stepper %}
{% step %}
### Add Platform

* Go to **My Platforms → Add Platform**
* Select **Roku Ads (Destination)** and click **Configure**
{% endstep %}

{% step %}
### Authenticate

* Click **Connect to Roku Account** and approve access
* Click **Save** — status shows **Connected**
{% endstep %}

{% step %}
### Create Connection

* Choose your source (S3, SFTP, Azure, Snowflake, etc.)
* Choose **Roku Ads Audience** as the destination
* Enter your **Account UID**
* Map your source columns to `segment_name`, `action` and the identifier columns (see Data Dictionary below)
{% endstep %}

{% step %}
### Run or Schedule

* Run a **Manual Transfer** for a one-time upload
* Or set up a **Scheduled Transfer** to send new files as they arrive
{% endstep %}

{% step %}
### Review and Save Connection

Review the configuration and click **Save**.
{% endstep %}
{% endstepper %}

## Data Dictionary

| Your column    | Sent to Roku as | What MadConnect does                                                                                                  |
| -------------- | --------------- | --------------------------------------------------------------------------------------------------------------------- |
| `segment_name` | Audience name   | Trimmed; one audience per value                                                                                       |
| `email`        | `sha256email`   | Trimmed, lowercased, then SHA-256 hashed (lowercase hex)                                                              |
| `email_sha256` | `sha256email`   | Already hashed (SHA-256, lowercase hex); sent as is. If a row has both, this value is used instead of hashing `email` |
| `phone`        | `phone`         | Sent as provided                                                                                                      |
| `maid`         | `maid`          | Mobile advertising ID, sent as provided                                                                               |
| `ip`           | `ip`            | Sent as provided                                                                                                      |
| `action`       | Not sent        | Mandatory. `create` or `add` (any case, spaces ignored). Rows with any other value are skipped                        |

* Column names are not case sensitive (`Email` and `email` are treated the same).
* Any other column is dropped before upload. Roku only receives `sha256email`, `phone`, `maid` and `ip`.
* Plain-text email never leaves MadConnect; it is hashed before upload.
* Rows where every identifier is empty are skipped. Duplicate rows are not removed; they are sent as they are.
* Roku matching is case sensitive. If you send pre-hashed emails, hash the trimmed, lowercased address.
* Rows whose action is not `create` or `add` are skipped and counted as skipped records; the rest of the file is still sent.
* A file with no `action` column is skipped with an error (see Troubleshooting).

## One Segment per File

Roku builds one audience from one uploaded file, so MadConnect uploads **exactly one segment per file**. Each file is named `{segment_name}.csv` and becomes one Roku audience with the same name as the segment.

**Unique audience names:** a segment whose name already exists in the Roku account is rejected (HTTP 422), whether it was created earlier in the same run or in a previous run. Use a new `segment_name` for each new audience.

## Additional Considerations

* **Matching happens in Roku:** Roku matches each file asynchronously after the audience is created, which can take several minutes.
* **Minimum audience size:** Roku needs about 1,000 matched households for an audience to be usable.
* **No updates to existing audiences:** Roku has no API to add users to an existing audience, so every run creates new audiences.
* **Audience expiration:** audiences are created with a 90-day expiration.
* **A failed segment does not stop the run.**
* Send the file as CSV or `.csv.gz`.

For more information, see the official [Roku Ads API documentation](https://developer.ads.roku.com/ads/reference/about-audiences).
