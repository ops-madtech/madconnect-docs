# Permutive - Audience

MadConnect sends audience segment data from your data warehouse into Permutive, so the people in your source data show up as segment members in your Permutive Import — ready to activate anywhere Permutive connects.

## Connector Overview

| Field             | Description                                                                                                                                     |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Connector Type    | Destination                                                                                                                                     |
| Data Type         | Audience Activation                                                                                                                             |
| Description       | Create, rename and delete segments directly via Permutive's API, and deliver segment membership to Permutive's Import through an Amazon S3 file |
| Primary Use Case  | Syncing first- or third-party audience segments from your warehouse into Permutive for activation                                               |
| Supported Actions | Write (create, add, remove, replace, delete)                                                                                                    |

## Prerequisites

### Authentication

This connector uses static keys, not a sign-in flow. You'll enter:

* A **Permutive private API key**, sent with every request to identify your workspace
* An **AWS Access Key** and **AWS Secret Key**, used to upload the membership file to S3

### Access Requirements

* A Permutive workspace with API access, and a private API key from Permutive
* A **Permutive Import already created** in your Permutive dashboard, set to read from S3 (MadConnect writes the file — it does not create the Import for you)
* An S3 bucket and folder that both MadConnect (to write) and Permutive (to read) can access
* AWS keys with write permission to that bucket path

### Know These Before Configuring

* Your **Import ID** (from the Permutive dashboard, once the Import exists)
* The **S3 Bucket Path** — this must be the exact folder your Permutive Import reads from, not a parent folder
* The **Import Identifier** — which standard field your customer ID column is mapped to (see [Mapping Your Identifier](permutive-audience.md#mapping-your-identifier) below)
* Your **Action** values, and whether each row needs a segment name (see [Understanding the Action Column](permutive-audience.md#understanding-the-action-column) below)

## Steps to Get Your Permutive API Key

1. Sign in to your Permutive dashboard
2. Go to your workspace settings and open the API keys section
3. Generate (or copy) your private API key
4. Keep this ready to enter into MadConnect

If you don't see this option, your account may need to be granted API access first — contact Permutive support.

## Steps to Create Your Permutive Import

MadConnect only writes the file — the Import itself is set up once in Permutive's own dashboard:

1. Go to **Connectivity → Imports → Create Import**
2. Choose **S3** as the source type, and select (or create) your S3 connection
3. Choose the **schema** and **table** — the table is the exact folder MadConnect will upload files into (see [File Format & Location](permutive-audience.md#file-format--location) below)
4. Choose **User Segment** as the data type
5. Map your columns:
   * **User ID** → your identifier column, with its identifier type declared (this must match what you choose as Import Identifier in MadConnect)
   * **Segment ID** → your segment column
   * **Segments Deleted** → your segment-removal column (optional)
   * **Time Field** → your timestamp column (optional)
6. Click **Save**

## Setting Up the Connector in MadConnect

**Add Platform**

* Go to **My Platforms** → **Add Platform**
* Select **Permutive (Destination)** and click **Configure**

**Authenticate**

* Enter your **Permutive API Key**
* Select **Cloud** as **S3** (other storage options are shown but not yet available for this connector)
* Enter your **AWS Access Key** and **AWS Secret Key**
* Click **Save** — status shows **Configured**

**Create Connection**

* Choose **Permutive** as the destination
* Enter your **Import ID**
* Choose your **Import Identifier** from the dropdown (see [Mapping Your Identifier](permutive-audience.md#mapping-your-identifier))
* Enter your **S3 Bucket Path**
* Choose your source (Redshift, Snowflake, S3, etc.)

**Schema Mapping**

Map your source columns to these standard fields:

| Standard Field            | What it holds                                                                                                            |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| `action`                  | What to do with this row — see [Understanding the Action Column](permutive-audience.md#understanding-the-action-column)  |
| _(your identifier field)_ | The person's ID — mapped to whichever standard field matches your ID type, and selected again as Import Identifier above |
| `segment_id`              | The segment's code in Permutive                                                                                          |
| `segment_name`            | The segment's display name — required only when `action` is `create` or `replace`                                        |

**Run or Schedule**

* Run a **Manual Pull** for a one-time sync
* Or set up a **Scheduled Sync** for recurring runs

**Review and Save Connection**

* Review the configuration and click **Save**

## Mapping Your Identifier

Every person in your data has to be identified the same way — MadConnect supports **one identifier type per connection**. Map your ID column to whichever standard field matches the type of ID you're sending, then select that same field as your Import Identifier. It must match the identifier type declared for your Permutive Import.

**Example:** if your data identifies people by Permutive's own cookie ID, map that column to `permutive_id` in Schema Mapping, and choose `permutive_id` as your Import Identifier too. If you instead track people by a hashed email, map to `email_sha256` and choose that instead.

| Standard Field     | What it is                                                                   |
| ------------------ | ---------------------------------------------------------------------------- |
| `gaid`             | Google Advertising ID (Android device ID)                                    |
| `idfa`             | Apple's Identifier for Advertisers (iOS device ID)                           |
| `email_sha256`     | A SHA-256 hashed email address                                               |
| `ip4`              | An IPv4 address                                                              |
| `ip6`              | An IPv6 address                                                              |
| `ttid`             | The Trade Desk ID                                                            |
| `extern_id`        | Any other external or custom ID that doesn't fit the types below             |
| `idfv`             | Apple Identifier for Vendor, shared across apps from the same developer      |
| `idfv_sha256`      | A SHA-256 hashed IDFV                                                        |
| `idfv_md5`         | An MD5 hashed IDFV                                                           |
| `ppid`             | Publisher Provided ID, for Google Ad Manager first-party audiences           |
| `ppid_sha256`      | A SHA-256 hashed PPID                                                        |
| `amp_id`           | Google AMP Client ID                                                         |
| `xandr_id`         | Xandr (formerly AppNexus) cookie-based user ID                               |
| `google_cookie_id` | Google cookie-based ID used for DV360 and Google Marketing Platform matching |
| `permutive_id`     | Permutive's own first-party cookie ID                                        |
| `gigya_id`         | SAP Customer Data Cloud (formerly Gigya) user ID                             |
| `piano_id`         | Piano subscription and paywall platform user ID                              |
| `mparticle_id`     | mParticle CDP user ID (mPID)                                                 |
| `sailthru_id`      | Sailthru email marketing platform user ID                                    |

If your ID column contains a plain email address, MadConnect hashes it to SHA-256 automatically before sending it — you don't need to hash it yourself.

## Understanding the Action Column

Each row's `action` value decides what happens to that person and that segment:

| Action    | What it does                                                  | Needs `segment_name`? |
| --------- | ------------------------------------------------------------- | --------------------- |
| `create`  | Creates a new segment in Permutive, and adds the person to it | Yes                   |
| `add`     | Adds the person to a segment that already exists              | No                    |
| `remove`  | Removes the person from a segment                             | No                    |
| `replace` | Renames an existing segment (no change to who's in it)        | Yes                   |
| `delete`  | Deletes a segment from Permutive (no change to membership)    | No                    |

**If the same person and segment appear more than once in a run:** the **last action wins**. For example, `add` followed by `remove` for the same person and segment ends in a removal, not both.

**If the same segment code appears more than once in a run with different actions** (for example, `create` and `delete` both sent for code `1001`): **both are sent to Permutive**, not just the last one. Permutive does not guarantee the order these are applied in, so the final state of that segment isn't predictable from our side. If you need a segment created and later deleted in a specific order, send those in separate runs rather than the same one.

## File Format & Location

MadConnect writes one file per run (or one per 1,000,000 rows, if a run is larger — extra files are created automatically, no action needed) directly into your configured S3 Bucket Path. **No sub-folder is created** — files land straight in that one folder:

```
<your S3 Bucket Path>/permutive_members_<connectionId>_<timestamp>_<n>.parquet
```

For example:

```
s3://your-bucket/your-import-folder/permutive_members_4c58e9121e845cc8_20260929_074552_1.parquet
```

Because every file name includes a timestamp and a counter, a later run never overwrites an earlier file.

The file is **Parquet** format, compressed with ZSTD, with these columns:

| Column             | Type          | Notes                                                                                       |
| ------------------ | ------------- | ------------------------------------------------------------------------------------------- |
| `user_id`          | Text          | The value from your mapped identifier column (hashed automatically if it was a plain email) |
| `segments`         | List of codes | The segment codes this person was added to in this run                                      |
| `segments_deleted` | List of codes | The segment codes this person was removed from in this run (an empty list if none)          |
| `updated_at`       | Timestamp     | When Permutive processed this person's segment changes                                      |

**Important:** your S3 Bucket Path must point at the exact folder your Permutive Import is set to read (its "table" folder), not the parent folder it sits in — otherwise Permutive won't discover the files.

## Additional Considerations

* **The Import must already exist**: MadConnect only writes files to S3; it does not create the Import in Permutive. Set that up first (see above).
* **One identifier type per connection**: if you track people by more than one ID type, set up a separate connection for each one.
* **Segment codes, not names, drive matching**: Permutive matches people to segments by the code you send, not the display name.
* **A blank or invalid value only skips that one row**, not the whole run — the rest of your data still processes normally, and skipped rows are noted in the run's details.
* **Large runs split automatically**: a run of more than 1,000,000 rows produces more than one file, with no configuration needed.
* **Write-only**: this connector only sends data to Permutive; it never reads anything back.

For more information, see Permutive's own documentation on [connecting to Amazon S3](https://docs.permutive.com/guides/connectivity/connections/connecting-to-amazon-s3) and [importing user segments](https://docs.permutive.com/guides/connectivity/imports/importing-user-segments).
