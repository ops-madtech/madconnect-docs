# Amazon Ads Data Manager

## Amazon Ads Data Manager (Audiences)

MadConnect integrates with Amazon's Ads Data Manager framework to support first party audience workflows across Amazon advertising destinations. With this connector, advertisers, agencies, and data partners can securely send first-party audience data from supported data sources into Amazon Ads for audience activation, targeting, and membership management.

This connector is intended for audience onboarding and ongoing audience updates using Amazon's Data Manager–based workflow.

***

### Connector Overview

| Field             | Description                                                                                                                                    |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| Connector Type    | Destination                                                                                                                                    |
| Data Type         | First Party Audience                                                                                                                           |
| Description       | Allows advertisers to use first-party customer data to create and update audiences in Amazon Ads for targeting and audience refresh workflows. |
| Supported Actions | Create / Add / Remove                                                                                                                          |

***

### Prerequisites

Before configuring Amazon Ads Data Manager in MadConnect, ensure the following:

**Amazon Access Requirements**

1. Active Amazon Ads account with permissions to manage audience workflows
2. Access to Amazon Ads Data Manager with an active Manager ID
3. Permission to manage customer data uploads under the destination account
4. Eligibility to use customer data for audience workflows under Amazon advertising policies
5. Any required Amazon approvals for Data Manager API usage

***

### Authentication Requirements

You must authenticate your Amazon environment using OAuth.

OAuth is required to allow MadConnect to create and manage audience workflows in the authorized Amazon account.

**Steps**

1. Go to My Platforms → Amazon Ads (Audiences – Data Manager) → Configure
2. Click **Connect to Amazon Account**
3. Log in with the Amazon account that has access to the target destination account
4. Approve the requested permissions
5. Once successful, the platform will appear as **Configured** in My Platforms

**One-Time OAuth Link (Copy Icon)**

A copy icon generates a single-use OAuth URL that can be shared with a teammate who must complete authentication (for example, if the account admin is a different user).

Notes:

1. The link expires after first use
2. A new link must be generated for additional authentication attempts

***

### Connection Configuration (UI Fields)

When creating an Amazon Ads (Audiences – Data Manager) connection in MadConnect, the destination configuration requires the following fields:

| Field                         | Required | Description                                                                                                                                       |
| ----------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| Manager ID                    | Yes      | The unique identifier for your Amazon Ads Data Manager account. This routes the audience upload to the correct Data Manager instance.             |
| Amazon consent for ad storage | Yes      | Indicates the consent status for storing user data for ad purposes. Accepted values: `GRANTED`, `DENIED`, `NOT_APPLICABLE`, `UNKNOWN`.            |
| Amazon consent for user data  | Yes      | Indicates the consent status for using the uploaded user data for advertising. Accepted values: `GRANTED`, `DENIED`, `NOT_APPLICABLE`, `UNKNOWN`. |

**Locating Your Manager ID**

Your Manager ID is found within the Amazon Ads Data Manager UI. Contact your Amazon Ads account representative if you do not have direct access to the Data Manager console.

**Consent Field Guidance**

The consent fields reflect the data usage permissions associated with the records being uploaded. Both fields must be set before a transfer can run. The accepted values for each field are:

| Value            | When to Use                                                                   |
| ---------------- | ----------------------------------------------------------------------------- |
| `GRANTED`        | The user has explicitly consented to this data use                            |
| `DENIED`         | The user has explicitly declined consent                                      |
| `NOT_APPLICABLE` | Consent is not required for this data or region (e.g., non-regulated markets) |
| `UNKNOWN`        | Consent status has not been determined or recorded                            |

1. Both consent fields apply at the connection level and should reflect the applicable consent posture for all records being sent through that connection
2. If records in your dataset reflect mixed consent states, segment them into separate connections with the appropriate consent configuration per segment
3. Amazon may restrict or filter delivery for records where consent is `DENIED`

***

### Audience Schema Requirements

MadConnect uses the standard audience schema for this connector. Your source data must include the required fields to create or update audiences in Amazon Ads.

#### Segment Fields, Actions, and Allowed IDs

| Field         | Required                                 | Description                                                                                                         |
| ------------- | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| segment\_name | Conditional (required for new audiences) | Name of the audience to be created in Amazon Ads Data Manager                                                       |
| segment\_id   | Conditional (required for updates)       | Amazon-assigned audience identifier. Returned in the Reports tab after initial audience creation.                   |
| action        | Yes                                      | `add` to create a new audience or add users to an existing one. `remove` to remove users from an existing audience. |

#### Creating a New Audience

1. Provide a `segment_name` value
2. Leave `segment_id` empty
3. Set `action` to `add`
4. MadConnect will create the segment in Amazon Ads Data Manager and return the generated `segment_id` in the Reports tab

#### Updating an Existing Audience

1. Provide the `segment_id` of the existing audience
2. Use `add` for incremental additions or `remove` for incremental removals
3. MadConnect will update membership for that audience

***

### Supported Identifier Fields

Amazon Ads Data Manager requires a `userIdentity` object on every record. Each record must include either a `hashedPiis` object or an `externalIdentities` object. Both may not be omitted on the same record, and up to 10 items are supported per array.

#### PII Identifiers

All PII fields must be normalized and SHA-256 hashed before delivery. Amazon does not accept plain-text PII. MadConnect will normalize and hash raw values if plain-text fields are provided in the source. If pre-hashed values are provided, they must already meet Amazon's normalization requirements.

| Amazon API Field | MadConnect Standard Field | Description                                        |
| ---------------- | ------------------------- | -------------------------------------------------- |
| `em`             | `email_sha256`            | Normalized and SHA-256 hashed email address        |
| `ph`             | `phone_sha256`            | Normalized and SHA-256 hashed phone number         |
| `fn`             | `fname_sha256`            | Normalized and SHA-256 hashed first name           |
| `ln`             | `lname_sha256`            | Normalized and SHA-256 hashed last name            |
| `ad`             | `address_sha256`          | Normalized and SHA-256 hashed street address       |
| `cty`            | `city_sha256`             | Normalized and SHA-256 hashed city name            |
| `st`             | `state_sha256`            | Normalized and SHA-256 hashed state or region code |
| `zip`            | `zip_sha256`              | Normalized and SHA-256 hashed postal or zip code   |

**PII Notes**

1. At least one PII field must be present if using the `hashedPiis` path
2. Multiple PII fields for the same user may be provided together to improve match rates (e.g., email and phone in the same record)
3. Up to 10 `hashedPiis` items are supported per record

#### External / Partner Identifiers (externalIdentities)

External identities are third-party data provider IDs or mobile advertising IDs. These are sent as plain text strings. Use this path when the source data contains partner-issued identifiers rather than raw PII.

| Amazon API Field | MadConnect Standard Field | Description                                                      |
| ---------------- | ------------------------- | ---------------------------------------------------------------- |
| `maId`           | `maid`                    | Mobile advertising identifier (IDFA for iOS or GAID for Android) |
| `liveRampId`     | `liveramp_id`             | User identifier provided by LiveRamp                             |
| `transunionId`   | `transunion_id`           | User identifier provided by TransUnion                           |
| `neustarId`      | `neustar_id`              | User identifier provided by Neustar                              |
| `merkleId`       | `merkle_id`               | User identifier provided by Merkle                               |
| `experianId`     | `experian_id`             | User identifier provided by Experian                             |
| `kantarId`       | `kantar_id`               | User identifier provided by Kantar                               |
| `realId`         | `real_id`                 | User identifier provided by RealId                               |
| `sambaTvId`      | `samba_tv_id`             | User identifier provided by Samba TV                             |

**External Identity Notes**

1. At least one external identity field must be present if using the `externalIdentities` path
2. External identity IDs are sent as plain text and are not hashed
3. Up to 10 `externalIdentities` items are supported per record
4. The data provider must have an established relationship with Amazon Ads Data Manager for the corresponding identifier type to be accepted
5. `hashedPiis` and `externalIdentities` serve as alternative identity paths. Records should use one or the other depending on what identifiers are available in the source data

***

### Hashing and Normalization Requirements

All `hashedPiis` fields must be normalized and SHA-256 hashed before delivery to Amazon. Amazon does not accept plain-text PII. External identity fields are sent as plain text and are not hashed.

MadConnect will normalize and hash raw PII values if plain-text source fields are provided. If pre-hashed values are provided, they must already comply with Amazon's normalization standards. MadConnect will not correct normalization on pre-hashed inputs.

**Algorithm:** SHA-256

| Field                 | Normalization Required Before Hashing                                        |
| --------------------- | ---------------------------------------------------------------------------- |
| Email (`em`)          | Lowercase, trim leading and trailing whitespace                              |
| Phone (`ph`)          | Convert to E.164 format (e.g., `+15551234567`), remove formatting characters |
| First Name (`fn`)     | Lowercase, trim whitespace                                                   |
| Last Name (`ln`)      | Lowercase, trim whitespace                                                   |
| Street Address (`ad`) | Lowercase, trim whitespace, abbreviate common terms (e.g., `street` → `st`)  |
| City (`cty`)          | Lowercase, trim whitespace                                                   |
| State (`st`)          | Lowercase, use standard two-letter abbreviation (e.g., `california` → `ca`)  |
| Postal Code (`zip`)   | Trim whitespace, use 5-digit format for US zip codes                         |

Improper normalization is the most common cause of low match rates and will not be corrected by MadConnect if values arrive pre-hashed.

***

### MadConnect Standard Schema Example

Your source file or table should follow this structure. The identity path used (PII or external) determines which identifier columns are required.

**PII-based records:**

| Field Name       | Data Type | Required                        | Description                                                 |
| ---------------- | --------- | ------------------------------- | ----------------------------------------------------------- |
| `segment_name`   | String    | Yes (Create)                    | Name of the audience (e.g., "Loyalty Members Q2")           |
| `segment_id`     | String    | Yes (Update)                    | The segment ID as returned by Amazon after initial creation |
| `action`         | String    | Yes                             | `add` or `remove`                                           |
| `email_sha256`   | String    | At least one PII field required | Normalized and SHA-256 hashed email address                 |
| `phone_sha256`   | String    | Optional                        | Normalized and SHA-256 hashed phone in E.164 format         |
| `fname_sha256`   | String    | Optional                        | Normalized and SHA-256 hashed first name                    |
| `lname_sha256`   | String    | Optional                        | Normalized and SHA-256 hashed last name                     |
| `address_sha256` | String    | Optional                        | Normalized and SHA-256 hashed street address                |
| `city_sha256`    | String    | Optional                        | Normalized and SHA-256 hashed city                          |
| `state_sha256`   | String    | Optional                        | Normalized and SHA-256 hashed state abbreviation            |
| `zip_sha256`     | String    | Optional                        | Normalized and SHA-256 hashed postal code                   |

**External identity-based records:**

| Field Name      | Data Type | Required                          | Description                              |
| --------------- | --------- | --------------------------------- | ---------------------------------------- |
| `segment_name`  | String    | Yes (Create)                      | Name of the audience                     |
| `segment_id`    | String    | Yes (Update)                      | Amazon-assigned audience identifier      |
| `action`        | String    | Yes                               | `add` or `remove`                        |
| `maid`          | String    | At least one external ID required | IDFA (iOS) or GAID (Android), plain text |
| `liveramp_id`   | String    | Optional                          | LiveRamp-issued identifier, plain text   |
| `transunion_id` | String    | Optional                          | TransUnion-issued identifier, plain text |
| `neustar_id`    | String    | Optional                          | Neustar-issued identifier, plain text    |
| `merkle_id`     | String    | Optional                          | Merkle-issued identifier, plain text     |
| `experian_id`   | String    | Optional                          | Experian-issued identifier, plain text   |
| `kantar_id`     | String    | Optional                          | Kantar-issued identifier, plain text     |
| `real_id`       | String    | Optional                          | RealId-issued identifier, plain text     |
| `samba_tv_id`   | String    | Optional                          | Samba TV–issued identifier, plain text   |

***

### Running Transfers in MadConnect

Once platform configuration and connection setup are complete:

1. Select your source (Snowflake, BigQuery, S3, GCS, etc.)
2. Select **Amazon Ads Data Manager (Audiences)** as the destination
3. Configure:
   * Manager ID
   * Amazon consent for ad storage
   * Amazon consent for user data
4. Map your source fields to the required MadConnect audience schema
5. Provide `segment_name` for new audiences or `segment_id` for existing audiences in your source data.
6. Run a Manual Transfer or configure a Scheduled Transfer

MadConnect will:

1. Validate schema compatibility
2. Validate required destination configuration fields and consent values
3. Normalize and hash identifiers where applicable
4. Create or update the audience in Amazon Ads Data Manager
5. Manage add/remove membership logic
6. Return metadata, record counts, and errors in the Reports tab

***

### Important Notes

1. This connector routes audience uploads through Amazon's Data Manager framework, which is distinct from the Amazon Ads DSP Audiences connector. Use the correct connector based on your Amazon account setup.
2. The Manager ID must match the Data Manager account that has access to the target advertising destination.
3. Every record must include either a `hashedPiis` identity or an `externalIdentities` identity. Records with neither will be rejected.
4. `hashedPiis` and `externalIdentities` are alternative identity paths. Do not mix PII and external identity fields in the same record.
5. Consent fields apply at the connection level. Segment records with different consent states into separate connections.
6. Pre-hashed PII fields must already meet Amazon's normalization requirements before delivery. MadConnect will not correct normalization on pre-hashed values.
7. Newly created audiences may take time to become available for targeting in the Amazon Ads UI after upload.
8. Minimum audience size thresholds may apply before an audience can be used in campaign targeting.

***

### Additional Resources

1. [Amazon Ads Data Manager Audience API Documentation](https://advertising.amazon.com/API/docs/en-us/ads-data-manager#tag/Audiences)
2. [Amazon Ads Data Manager API Documentation](https://advertising.amazon.com/API/docs/en-us/ads-data-manager)
