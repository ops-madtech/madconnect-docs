# Viant DSP - Audience Activation

MadConnect integrates with the Viant DSP API to enable advertisers and data partners to activate first-party audiences for targeting on the Viant platform. The Viant DSP Audience connector supports creating new “1st Party Segments” and updating existing ones using SHA-256 hashed identifiers, allowing advertisers to reach relevant Viant users based on first-party data.

***

### Connector Overview

| Field             | Description                                                         |
| ----------------- | ------------------------------------------------------------------- |
| Connector Type    | Destination                                                         |
| Data Type         | Audience Activation                                                 |
| Description       | Create and manage Viant 1st Party Segments using hashed identifiers |
| Supported Actions | Add                                                                 |

***

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Before configuring the Viant DSP Audience connector, ensure the following:

#### Viant Account Requirements

1. An active relationship with a Viant representative
2. A signed Viant API Terms & Conditions agreement
3. API credentials issued by Viant, separate from standard DSP login credentials — for CDPs/data partners, this includes a client\_id and client\_secret
4. A registered redirect URI on file with Viant (required for CDP/data partner onboarding)

#### Authentication Requirements

Viant DSP Audience requires OAuth 2.0 authentication using the CDP/Data Partners flow. MadConnect supports two methods for authenticating with Viant:

**Option 1: OAuth (Recommended)**

1. Request API access from your Viant representative, providing your name, email, title, and company name
2. Sign Viant's API Terms & Conditions agreement
3. Provide MadConnect's redirect URI to Viant so they can issue your client\_id and client\_secret
4. Click “Connect to Viant Account” and complete the one-time authorization login via Viant's hosted login page to generate an initial refresh token
5. MadConnect securely stores the client\_id, client\_secret, and refresh token, and automatically refreshes the access token for all subsequent API calls

Upon success, the platform status will show Configured in My Platforms. OAuth tokens are securely stored via the platform.

**Option 2: Manual OAuth (Advanced)**

For users who prefer to supply credentials directly (e.g., when the redirect-based flow isn't available in their environment), enable “Allow me to enter the tokens manually,” then provide:

| Field         | Description                                                                               |
| ------------- | ----------------------------------------------------------------------------------------- |
| Client ID     | Your Viant CDP/Data Partner client\_id                                                    |
| Client Secret | Your Viant CDP/Data Partner client\_secret                                                |
| Access Token  | OAuth access token used for API requests                                                  |
| Refresh Token | Token obtained via Viant's authorization\_code exchange, used to refresh the access token |
|               |                                                                                           |

***

### Connection Configuration (UI Fields)

Once the connector is configured, create a connection and provide the following when Viant is selected as the destination.

#### Viant Destination Fields

<table><thead><tr><th width="234">Field</th><th width="159">Required?</th><th>Description</th></tr></thead><tbody><tr><td>Account ID</td><td>Yes</td><td>Viant account identifier</td></tr><tr><td>Advertiser ID</td><td>Yes</td><td>Viant advertiser identifier the audience will be created or updated under</td></tr><tr><td>External Audience ID</td><td>Yes</td><td>Your own reference ID for the audience, passed through to Viant</td></tr><tr><td>Country Code</td><td>Yes</td><td>Country code associated with the audience data (e.g., US)</td></tr></tbody></table>

***

### Audience Creation & Management Logic

Viant audiences are managed using the following core fields.

#### Segment Fields

| Field         | Required?    | Description                                                                |
| ------------- | ------------ | -------------------------------------------------------------------------- |
| segment\_name | Yes (Create) | Name of the Viant segment to be created                                    |
| segment\_id   | Yes (Update) | Existing Viant segment ID (format: 1p\_segm followed by 23 hex characters) |

#### Action Field

| Field  | Required? | Description                       |
| ------ | --------- | --------------------------------- |
| action | Yes       | Controls membership updates (add) |

#### Creating a New Audience

1. Provide **segment\_name**
2. Omit **segment\_id**
3. Set **action** field to **add**

MadConnect will:

* Create a new Viant segment
* Upload hashed/matched identifiers
* Return the generated segment\_id in Reports → info

If the same segment\_name appears more than once within the same transfer (e.g., across multiple files), MadConnect creates the segment only once and reuses it for every subsequent row referencing that name.

#### Updating an Existing Audience

1. Provide the existing **segment\_id** (and **segment\_name**, if known)
2. Set **action** field to **add**

MadConnect will update membership for that segment directly, skipping segment creation.

***

### Matching Keys (Viant Audiences)

#### Supported ID Fields

Viant's API supports multiple identifier types for matching, not email alone. Supported data types include:

<table><thead><tr><th>ID Type</th><th width="204">Field Name</th><th width="132">Hashed?</th><th>Notes</th></tr></thead><tbody><tr><td>Email</td><td>email_sha256</td><td>Yes</td><td>SHA-256 hashed email</td></tr><tr><td>Phone Number</td><td>phonenumber_sha256</td><td>Yes</td><td>SHA-256 hashed phone number</td></tr><tr><td>Address</td><td>address_sha256</td><td>Yes</td><td>SHA-256 hash of concatenated street address, city, state, and zip</td></tr><tr><td>IP Address</td><td>ip</td><td>No</td><td>Raw IP address</td></tr><tr><td>Cookie</td><td>cookie</td><td>No</td><td>Raw cookie identifier</td></tr><tr><td>Mobile ID</td><td>mobile_id</td><td>No</td><td>Raw mobile advertising ID (MAID)</td></tr></tbody></table>

Each connection is configured for a single dataType at a time (set via the destination config), which determines which identifier field is expected in the source data.

#### MadConnect Standard Audience Schema Example

Your source file or table should generally follow this structure to ensure seamless integration.

| Field Name                                   | Data Type | Required?                     | Description                                                                                                                                                     |
| -------------------------------------------- | --------- | ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| segment\_name                                | String    | Yes (Create)                  | Name of the audience (e.g., Holiday Shoppers)                                                                                                                   |
| segment\_id                                  | String    | Yes (Update)                  | Viant segment ID (required only when updating)                                                                                                                  |
| action                                       | String    | Yes                           | Accepted value: add                                                                                                                                             |
| email / phone / address / ip / cookie / maid | String    | Yes (per configured dataType) | Raw identifier value; MadConnect normalizes and SHA-256 hashes email, phone, and address before sending. IP, cookie, and mobile ID are passed through unhashed. |

#### Notes

* Which identifier field is required depends on the connection's configured dataType
* This schema is shared across all MadConnect audience connectors

#### Hashing & Normalization Requirements

* Algorithm: SHA-256 (applies to email, phone, and address identifiers)

Normalization (before hashing):

* Lowercase the value
* Remove leading/trailing whitespace
* For phone: strip extensions, spaces, dashes, parentheses, +, and country code
* For address: concatenate street address, city, and “state + zip” with a comma separator before hashing
* Hashed values must be lowercase, 64-character hexadecimal strings

Incorrect normalization is the most common cause of low match rates.

***

### Important Notes & Limitations

* Viant may require a minimum audience size before a segment can be used for targeting
* Match rates depend heavily on data quality and normalization accuracy
* Newly created segments are initially returned with status INACTIVE and may take time to become active
* Each push request is limited to 10MB; MadConnect automatically chunks large files to stay within this limit
* API errors often indicate normalization, hashing, or segment ID format issues (segment\_id must start with 1p\_segm followed by 23 hex characters)

***

### Resources

For more information on the Viant API, please review the [Viant API Documentation](https://docs.api.viantinc.com/reference/how-to-get-started).
