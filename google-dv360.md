# Google DV360

![](<.gitbook/assets/image (24).png>)

## Google Display & Video 360 (DV360) – Customer Match Overview

MadConnect integrates with the Google Display & Video 360 (DV360) Customer Match API to enable advertisers to activate first-party audiences for targeting and suppression across DV360 inventory. This connector automates audience creation, population, and maintenance using Google-approved identifiers, fully managed within the MadConnect platform.

MadConnect handles identifier normalization, hashing, schema validation, and API orchestration to ensure compliant and reliable audience delivery.

***

### Connector Overview

| **Field**         | **Description**                                                                     |
| ----------------- | ----------------------------------------------------------------------------------- |
| Connector Type    | Destination                                                                         |
| Data Type         | Customer Match (Hashed & Approved IDs)                                              |
| Primary Use Case  | Audience Activation                                                                 |
| Description       | Activate and manage Customer Match audiences in DV360 using first-party identifiers |
| Supported Actions | Add / Remove                                                                        |

***

### Prerequisites

Before configuring the DV360 Customer Match connector, ensure the following:

#### Access & Eligibility Requirements

1. An active DV360 Partner and Advertiser
2. Partner and advertiser must have:
3. Good policy compliance history
4. Good billing and payment standing
5. Customer Match allowlisting enabled at the partner level
6. Eligible accounts are typically allowlisted automatically

#### Data Upload Eligibility

1. Customer Match uploads must be enabled for the advertiser’s parent partner
2. Eligibility can be verified by attempting to create a Customer Match audience directly in the DV360 UI
3. If audience creation is blocked in the UI, API-based uploads will fail

#### Consent Requirements

1. MadConnect submits Customer Match data with consent values set to Granted
2. Customers must ensure:
3. User data consent is granted
4. Ad personalization consent is granted
5. Uploading data without proper consent may violate Google policies

***

### Authentication Requirements

DV360 Customer Match requires OAuth authentication.

#### OAuth Authentication (Required)

1. Navigate to My Platforms → Google DV360 – Customer Match
2. Click Configure
3. Select Sign in with Google
4. Authenticate using a Google account with DV360 access
5. Approve permissions to manage Customer Match audiences
6. Upon success, the platform status will display Configured in My Platforms

OAuth tokens are securely stored and automatically refreshed by MadConnect.

***

#### One-Time OAuth Link (Copy Icon)

A copy icon generates a single-use OAuth URL.

Use cases

1. A business administrator must authenticate under a shared Google account
2. Authentication cannot be completed by the initiating user

Notes

1. The link expires after one successful authorization
2. A new link must be generated for additional authentication attempts

***

### Connection Configuration (UI Fields)

When creating a connection with Google DV360 – Customer Match as the destination, the following fields are required or available in the UI.

#### DV360 Destination Fields

| Field            | Required | Description                                                                    |
| ---------------- | -------- | ------------------------------------------------------------------------------ |
| Advertiser ID    | Yes      | DV360 Advertiser ID under the authenticated partner                            |
| Audience Source  | Optional | Metadata describing the origin of the audience (e.g., CRM, Website, App)       |
| Audience Type    | Yes      | Defines the identifier category being uploaded (e.g., Contact Info, MAID)      |
| Retention (days) | Yes      | Number of days users remain in the audience (DV360-supported retention window) |

These fields control how DV360 interprets and manages the resulting Customer Match audience.

***

### Audience Creation & Management Logic

DV360 uses the following fields to manage Customer Match audiences.

#### Segment Fields

| Field         | Required            | Description                               |
| ------------- | ------------------- | ----------------------------------------- |
| segment\_name | Yes (new audiences) | Name of the DV360 audience to be created  |
| segment\_id   | Required (updates)  | Existing DV360 Customer Match audience ID |

#### Action Field

| Field  | Required | Description                                 |
| ------ | -------- | ------------------------------------------- |
| action | Yes      | Controls membership updates (add or remove) |

***

#### Creating a New Audience

1. Provide segment\_name
2. Omit segment\_id
3. Set action = add
4. MadConnect will:
5. Create a new Customer Match audience in DV360
6. Upload identifiers
7. Return the generated segment\_id in the Reports → info object

***

#### Updating an Existing Audience

1. Provide the existing segment\_id
2. Set action to add or remove
3. MadConnect will update membership for that audience

***

### Matching Keys (DV360 Customer Match)

At least one supported identifier must be present per row.\
MadConnect will normalize and hash identifiers when raw values are provided.

#### Supported Identifier Fields

| **ID Type**  | **Field Name**        | **Hashed** | **Notes**                          |
| ------------ | --------------------- | ---------- | ---------------------------------- |
| Email        | email / email\_sha256 | Yes        | Lowercase + trim before hashing    |
| Phone        | phone / phone\_sha256 | Yes        | Must be E.164 before hashing       |
| MAID         | maid                  | No         | Mobile Advertising ID (plain text) |
| First Name   | fname / fname\_sha256 | Yes        | Used with last name + postal       |
| Last Name    | lname / lname\_sha256 | Yes        | Used with first name + postal      |
| Postal Code  | postal\_code          | No         | Used with name + country           |
| Country Code | country\_code         | No         | ISO-3166-1 alpha-2                 |

Important:\
Name, postal code, and country code must be supplied together as a valid matching group.

***

### MadConnect Standard Schema Example (Audience Connectors)

Your source file or table should generally follow this structure to ensure seamless integration:

| **Field Name** | **Data Type** | **Required?**             | **Description**                                                                               |
| -------------- | ------------- | ------------------------- | --------------------------------------------------------------------------------------------- |
| segment\_name  | String        | Yes (Create)              | Name of the audience (e.g., "Holiday Shoppers").                                              |
| segment\_id    | String        | Yes (Update)              | The segment ID as it appears in the destination platform (only if updating an existing list). |
| action         | String        | Yes                       | add or remove. Use add for creation.                                                          |
| email\_sha256  | String        | At least one supported ID | User's email address.                                                                         |



***

### Hashing & Normalization Requirements

1. Algorithm: SHA-256
2. Normalization:
3. Lowercase all PII
4. Trim whitespace
5. Remove punctuation for names
6. Phone Numbers:
7. Convert to E.164 format before hashing
8. Pre-hashed values:
9. Must already meet Google normalization requirements

Improper normalization may reduce match rates or cause API rejection.

***

### Important Notes & Limitations

1. Does not support true “replace” semantics
2. Full replacement must be modeled as remove + add
3. Audience availability may be delayed while Google processes uploads
4. Audience size may not display until minimum thresholds are met
5. Common API errors indicate:
6. Missing consent
7. Partner-level Customer Match not enabled
8. Schema or identifier violations

***

### Disclosure

MadConnect’s use and transfer of information received from Google APIs complies with the Google API Services User Data Policy, including Limited Use requirements.

***

### Resources

* [Google DV360 Customer Match Documentation](https://developers.google.com/display-video/api/guides/audiences/upload-customer-match)
* [Google API Services User Data Policy](https://developers.google.com/terms/api-services-user-data-policy#additional_requirements_for_specific_api_scopes)
