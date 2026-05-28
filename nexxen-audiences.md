# Nexxen - Audiences

MadConnect integrates with Nexxen's Partner Portal via SFTP to support file-based audience data onboarding. Data providers and agencies can use this connector to deliver segment data files to the Nexxen platform for audience activation across Nexxen's programmatic ecosystem.

This connector supports Nexxen's two-phase ingestion workflow: first establishing a data contract via configuration and taxonomy files, then delivering recurring data files against that contract. Both phases are managed through MadConnect using Google Cloud Storage, S3, or another supported data store as the source.

***

### Connector Overview

| Field             | Description                                                                                                                                                                   |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Connector Type    | Destination                                                                                                                                                                   |
| Data Type         | Data Store (File-Based Audience Delivery)                                                                                                                                     |
| Description       | Delivers segment data files to Nexxen's SFTP server for audience ingestion via the Nexxen Partner Portal. Supports both data contract setup and recurring data file delivery. |
| Supported Actions | Put                                                                                                                                                                           |
| Transfer Method   | SFTP with SSH key-based authentication                                                                                                                                        |

***

### Prerequisites

Before configuring the Nexxen SFTP connector in MadConnect, the following must be in place.

**Nexxen-Side Requirements**

1. A provisioned SFTP directory or S3 bucket on Nexxen's server, established by contacting `dataoperations@nexxen.com` with your public SSH key and preferred folder name
2. A Data Provider ID assigned by Nexxen following initial account setup
3. Full path to the assigned SFTP directory, provided by Nexxen after access is provisioned
4. A data contract created and approved by Nexxen before data file ingestion can begin
5. Taxonomy finalized and uploaded before data files are sent

**MadConnect-Side Requirements**

1. Source data store configured in MadConnect (GCS, S3, or equivalent) containing either config/taxonomy files or data files depending on the phase
2. SFTP credentials obtained from Nexxen (hostname, username, SSH private key)
3. Data Provider ID and assigned SFTP path ready for connection configuration

***

### Authentication Requirements

Nexxen SFTP uses SSH private key–based authentication. Password-based authentication is not supported. The private key must not be passphrase-protected.

**Steps**

1. Go to My Connectors → Nexxen → Configuration tab
2. Enter the following fields and save:

| Field           | Required | Description                                                                                            |
| --------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| Hostname        | Yes      | Nexxen SFTP server hostname (e.g., `ftp2.turn.com`)                                                    |
| Port            | Yes      | SFTP port                                                                                              |
| Username        | Yes      | SFTP username provided by Nexxen during access provisioning                                            |
| SSH Private Key | Yes      | Private key corresponding to the public SSH key submitted to Nexxen. Must not be passphrase-protected. |

Once saved successfully, the platform will appear as Configured under My Connectors.

***

### Connection Configuration (UI Fields)

When creating a connection with Nexxen SFTP as the destination, the following fields are required:

| Field            | Required | Description                                                                                                       |
| ---------------- | -------- | ----------------------------------------------------------------------------------------------------------------- |
| Path             | Yes      | The destination directory path on the Nexxen SFTP server where files will be written. Example: `home/madconnect/` |
| Data Provider Id | Yes      | The unique Data Provider ID assigned to your organization by Nexxen. Example: `1151826295`                        |

The Path must correspond to your provisioned directory on the Nexxen SFTP server. Nexxen provides this path during initial onboarding.

***

### Nexxen Ingestion Workflow Overview

Delivering data to Nexxen requires two distinct phases. MadConnect handles both, but they must be run in sequence.

**Phase 1 – Data Contract Setup**

A data contract defines the taxonomy and configuration for how Nexxen will interpret and classify the incoming data. This phase requires uploading two files to your Nexxen SFTP directory:

1. A configuration file (`<dataProviderId>_<uniqueId>_config.json`) containing contract metadata
2. A taxonomy file (`<dataProviderId>_<uniqueId>_taxonomy.xls`, `.xlsx`, or `.csv`) defining segment hierarchy

Nexxen processes these files on an hourly basis. Once validated, Nexxen **emails the assigned data contract ID to the email address specified in the config file**. This contract ID is required for Phase 2 file naming.

If validation fails, both files are moved to an `InvalidFiles` folder and an error message is sent. Corrected files must be re-uploaded with the same naming convention.

**Phase 2 – Data File Ingestion**

After the data contract is confirmed, data files containing user identifiers and their associated category IDs are delivered to the same SFTP directory. Nexxen processes new files hourly. Each file must reference the assigned data contract ID in its filename and must follow Nexxen's required format.

***

### Two Approaches for Running This Connector in MadConnect

Because Phase 1 and Phase 2 require different source files and different schema mapping configurations, there are two recommended approaches for managing this workflow in MadConnect.

#### Approach 1: Single Connection (Reconfigure Between Phases)

Create one connection using Nexxen SFTP as the destination.

1. Point the source to the folder or bucket containing your config and taxonomy files
2. Run the connection to deliver Phase 1 files to Nexxen
3. Wait for the Nexxen approval email confirming the data contract ID
4. Reconfigure the same connection: update the source to point to your data files folder and configure schema mapping for the data file format
5. Run the connection for Phase 2 data delivery

**Important:** If the config files and data files reside in the same source folder, set the transfer mode to **Incremental Load**. This ensures MadConnect does not re-pick config files on subsequent Phase 2 runs, as only new files will be processed.

#### Approach 2: Two Separate Connections (Recommended)

Create two dedicated connections — one for each phase. This is the preferred approach because it provides clearer operational separation, avoids reconfiguration between phases, and makes it easier to monitor the status of each phase independently in the Reports tab.

**Connection A — Config File Delivery**

| Setting          | Value                                                                    |
| ---------------- | ------------------------------------------------------------------------ |
| Source           | GCS, S3, or other data store pointing to the config/taxonomy file folder |
| Destination      | Nexxen SFTP                                                              |
| Path             | Assigned Nexxen SFTP directory                                           |
| Data Provider Id | Your Nexxen-assigned Data Provider ID                                    |
| Schema Mapping   | Not required for config/taxonomy file transfer                           |

Run Connection A first. Do not proceed to Connection B until the Nexxen approval email is received.

**Connection B — Data File Delivery**

| Setting          | Value                                                         |
| ---------------- | ------------------------------------------------------------- |
| Source           | GCS, S3, or other data store pointing to the data file folder |
| Destination      | Nexxen SFTP                                                   |
| Path             | Assigned Nexxen SFTP directory                                |
| Data Provider Id | Your Nexxen-assigned Data Provider ID                         |
| Schema Mapping   | Map source fields to user identifier and category ID columns  |

**Important:** If Connection A and Connection B point to the same source folder, configure Connection B to use **Incremental Load** so that config and taxonomy files are not re-delivered during data ingestion runs.

***

### Phase 1 File Specifications

#### Configuration File

**Naming convention:** `<dataProviderId>_<uniqueId>_config.json`

Example: `1151826295_555_config.json`

The `<uniqueId>` is a random string unique to each config and taxonomy pair within your SFTP folder. It cannot be reused and is not the data contract ID.

**Required Fields:**

| Field                  | Description                                                                                                                     | Example               |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| `dataproviderId`       | Your Nexxen-assigned Data Provider ID                                                                                           | `1151826295`          |
| `name`                 | A descriptive name for the data contract                                                                                        | `MyAudiences_2026`    |
| `dataContractId`       | Set to `-1` for new contracts                                                                                                   | `-1`                  |
| `classificationSymbol` | Segment classification type: `1` (2nd Party), `2` (3rd Party Custom), `3` (3rd Party AlwaysOn), `4` (CRM Data), `11` (IP-based) | `2`                   |
| `marketId`             | Nexxen market ID. Use `-1` for availability across all markets                                                                  | `884`                 |
| `availability`         | `AllAdvertisers` or `SingleAdvertiser`                                                                                          | `AllAdvertisers`      |
| `advertiserId`         | Required only if `availability` is `SingleAdvertiser`                                                                           | `98764562`            |
| `currency`             | Currency code: `USD`, `GBP`, `EUR`, `CAD`, `AUD`, `JPY`, `SGD`                                                                  | `USD`                 |
| `rateType`             | Pricing type: `FF` (Flat Fee), `CPM`, `PM` (Percent of Media), `CPAP`                                                           | `CPM`                 |
| `currencyRate`         | Cost of the data contract. Cannot be `0` if rateType is `CPM`, `PM`, or `CPAP`                                                  | `1.50`                |
| `dataContractTypeId`   | `3` for file-based ingestion, `4` for API ingestion                                                                             | `3`                   |
| `dataPixelTypeId`      | Required if `dataContractTypeId` is `4`: `3` (Device ID), `4` (Nexxen ID)                                                       | `3`                   |
| `idfa/aaid`            | If `dataPixelTypeId` is `3`, enter `True` or `False` indicating whether device IDs are included                                 | `False`               |
| `emailId`              | Email address(es) for Nexxen success/failure notifications. Separate multiple with commas                                       | `partner@example.com` |
| `startDate`            | Contract start date in `YYYY-MM-DD` format                                                                                      | `2026-05-01`          |
| `endDate`              | Contract end date in `YYYY-MM-DD` format, or empty string for open-ended                                                        | `""`                  |

#### Taxonomy File

**Naming convention:** `<dataProviderId>_<uniqueId>_taxonomy.xls(x)` or `.csv`

Example: `1151826295_555_taxonomy.csv`

The taxonomy file is an Excel or CSV document that defines the segment hierarchy used to classify user data. It must contain at least one parent category and one sub-category. Taxonomy structure supports multi-level hierarchies (Level 1 through Level N).

**Required Columns:**

| Column    | Description                                                                            |
| --------- | -------------------------------------------------------------------------------------- |
| ID        | Unique alphanumeric category ID for each segment node                                  |
| Hidden    | Enter `t` to hide a category. If a parent is hidden, all children must also be hidden. |
| Parent ID | ID of the parent category. Leave blank for root-level categories.                      |
| Level 1   | Main parent category name                                                              |
| Level 2   | Sub-category name (child of Level 1)                                                   |
| Level 3   | Sub-sub-category name (child of Level 2)                                               |

<figure><img src=".gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

Categories cannot be deleted after the taxonomy has been onboarded. They can only be hidden. Taxonomy updates use the same filename as the original and are processed on an hourly scan cycle.

***

### Phase 2 File Specifications

#### Data File Naming Convention

**Pattern:** `DataproviderID_UniqueID_YYYYMMDDHHMMSS_IDType`

The `UniqueID` here is the data contract ID assigned by Nexxen and communicated via email after Phase 1 approval.

**Examples:**

| File Type          | Example Filename                           |
| ------------------ | ------------------------------------------ |
| Cookie             | `1151826295_001_20260527083025_Cookie.txt` |
| Mobile (IDFA/AAID) | `1151826295_001_20260527083025_IDFA.txt`   |
| HEM                | `1151826295_001_20260527083025_HEM.txt`    |

#### Supported Identifier Types (ID Type Values)

| ID Type  | Description                                                                      |
| -------- | -------------------------------------------------------------------------------- |
| `Cookie` | Nexxen or partner cookie ID                                                      |
| `IDFA`   | iOS mobile advertising identifier                                                |
| `AAID`   | Android mobile advertising identifier (IDFA file can contain both IDFA and AAID) |
| `IP`     | IP address                                                                       |
| `HEM`    | Hashed email address                                                             |
| `UID2.0` | Unified ID 2.0                                                                   |
| `RampID` | LiveRamp RampID                                                                  |
| `LUID`   | LiveRamp LUID                                                                    |
| `ID5`    | ID5 identifier                                                                   |

Each identifier type must be delivered as a separate file. A single data file cannot mix identifier types.

#### Data File Format

Files must be delimited with no column headers. Headers must be removed before uploading.

| Column   | Description                                                                           |
| -------- | ------------------------------------------------------------------------------------- |
| Column 1 | User identifier (Cookie, IDFA, HEM, etc.)                                             |
| Column 2 | Comma-separated list of the lowest-level category IDs from the taxonomy for that user |

**Delimiters:**

1. Tab (`\t`) between Column 1 and Column 2
2. Comma (`,`) between category IDs within Column 2

**File format:** `.txt` (plain text) or compressed `.gz` / `.zip`

**Example (tab-delimited, no header):**

```
D2353971-B5F2-4967-B499-05C1D542B461    1008012211,1008012221
FFC77F99-AB88-4FAE-A437-8CB1A82F472A    1008012211
FFE45960-7917-4C7B-A4EC-DE71DCE91DF3    1008012221
```

***

### Running Transfers in MadConnect

**Recommended Sequence (Approach 2 — Two Connections):**

1. Configure Nexxen SFTP credentials under My Connectors
2. Create Connection A (config file delivery): source points to folder containing config and taxonomy files; no schema mapping required
3. Run Connection A
4. Wait for Nexxen email confirming the data contract ID
5. Create Connection B (data file delivery): source points to folder containing data files; configure schema mapping to map identifier and category ID fields
6. Run Connection B for initial data delivery and ongoing scheduled transfers

**Incremental Load Note:**

If the config and taxonomy files share the same source folder as data files, enable Incremental Load on the data file connection. This ensures only new files are processed on each run, preventing config and taxonomy files from being re-delivered during data ingestion runs.

**MadConnect will:**

1. Authenticate to the Nexxen SFTP server using the configured SSH private key
2. Write files to the specified destination path
3. Apply the Nexxen-required file naming convention based on the Data Provider ID and connection configuration
4. Report delivery status, file transfer success or failure, and run-level visibility in the Reports tab

***

### Important Notes

1. Do not deliver data files before Nexxen confirms the data contract via email. Files delivered without a valid data contract will not be ingested.
2. Taxonomy files must be finalized and validated before data ingestion begins. Update taxonomies before attempting to send data files.
3. Categories cannot be deleted from the taxonomy after onboarding — they can only be hidden. If a parent category is hidden, all child categories must also be hidden.
4. Each identifier type must be in a separate file. Mixing identifier types within a single file is not supported. The exception is IDFA and AAID, which can be delivered in the same file.
5. Data files must not contain column headers. Remove all headers before delivery.
6. Nexxen scans the SFTP directory for new files on an hourly basis. Allow up to one hour for files to be picked up after delivery.
7. If config validation fails, both the config and taxonomy files are moved to an `InvalidFiles` folder. Correct and re-upload both files to retry.
8. If taxonomy validation fails, no data contract is created. Correct the taxonomy file and re-upload using the same filename.
9. To update an existing taxonomy, upload the corrected file using the same original filename. The config file is not required for taxonomy updates.
