# Google Ads - Customer Match

## Google Ads - Customer Match Overview

MadConnect enables seamless integration with the Google Ads Customer Match API, allowing you to upload first-party customer data for targeting across Search, Gmail, YouTube, Display, and the Shopping tab. This connector automates the audience sync workflow, giving you control over list creation, population, and removal from within the MadConnect platform.

### Connector Overview

| Field             | Description                                                                                       |
| ----------------- | ------------------------------------------------------------------------------------------------- |
| Connector Type    | Destination                                                                                       |
| Data Type         | Customer Match (Hashed IDs)                                                                       |
| Description       | Activate customer match audiences by syncing your first-party customer identifiers to Google Ads. |
| Supported Actions | Add / Remove                                                                                      |

***

### Prerequisites

Before configuring Google Ads in MadConnect, ensure the following:

#### Access Requirements

* Account Eligibility: Your Google Ads account must meet Google’s eligibility criteria for Customer Match (typically involves good compliance and billing history).
* Standard Access: You must have a Google Ads account with permissions to manage audiences.

#### Authentication Requirements

You must authenticate your Google Ads account using OAuth.

**Option 1: OAuth Authentication (Recommended)**

OAuth provides the most secure and stable authentication.

Steps

1. Go to My Platforms → Google Ads – Customer Match → Configure.
2. Click Sign in with Google Ads.
3. Log in with your Google Ads credentials.
4. Approve all requested permissions (Campaigns, Audience Lists, Reporting).
5. Once successful, the platform will appear as Configured in My Platforms.

**One-Time OAuth Link (Copy Icon)**

A copy icon generates a single-use OAuth URL, which can be shared with a teammate who must complete authentication.

Use cases:

* Business admin must authenticate under a shared business account.
* Authentication cannot be completed by the immediate user.

Notes:

* The link expires after one successful authentication.
* A new link must be generated for additional authentication attempts.

***

### Connection Configuration (UI Fields)

When creating a Google Ads Customer Match connection in MadConnect, the destination configuration requires the following fields:

#### Google Ads Destination Fields

| Field                      | Required    | Description                                                                                                                                                              |
| -------------------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Customer Id                | Yes         | Your Google Ads Customer ID (e.g., 123-456-7890).                                                                                                                        |
| Audience Type              | Yes         | <p>Select the type of data being uploaded:</p><p>• Contact Information: For emails, phones, names, and addresses.</p><p>• Mobile Advertising ID: For IDFA/AAID data.</p> |
| Mobile App ID              | Conditional | The unique ID of the mobile application. Optional; only relevant if Audience Type is set to "Mobile Advertising ID".                                                     |
| Retention (Days)           | Yes         | The number of days a user remains in the audience before expiring. Default is 540 days (maximum allowed).                                                                |
| Ad User Data Consent       | Yes         | Specifies consent status for sending user data to Google for advertising purposes. Options: Granted, Denied, or Unspecified.                                             |
| Ad Personalization Consent | Yes         | Specifies consent status for ad personalization. Options: Granted, Denied, or Unspecified.                                                                               |



***

### Audience Creation & Management Logic

MadConnect uses the following fields to manage user lists:

#### Segment Fields

| Field         | Required                | Description                                           |
| ------------- | ----------------------- | ----------------------------------------------------- |
| segment\_name | Yes (for new audiences) | Name of the audience to be created in Google Ads.     |
| segment\_id   | Required for updates    | The unique user list ID in Google Ads (e.g., 123456). |

#### Action Field

| Field  | Required | Description                                                                                        |
| ------ | -------- | -------------------------------------------------------------------------------------------------- |
| action | Yes      | Controls whether the user is added or removed. Use add when creating a new audience. add or remove |

#### Creating a New Audience

1. Provide segment\_name but omit segment\_id.
2. Set the action field to add.
3. MadConnect will create the segment in Google Ads and return the new segment\_id in the transfer report.

#### Updating an Existing Audience

1. Provide the segment\_id of the existing list.
2. Set the action field to add or remove.
3. MadConnect will append or remove users from that specific list.

***

### Matching Keys (Google Customer Match)

To successfully send data to the Google Ads Customer Match API, your data must have one of the following matching fields.

MadConnect will normalize and hash any identifiers that platforms require if sending raw values.

#### Supported ID Fields

| ID Type           | Field Name                      | Hashed? | Notes                                                      |
| ----------------- | ------------------------------- | ------- | ---------------------------------------------------------- |
| Email             | <p>email</p><p>email_sha256</p> | Yes     | Lowercase + trim before hashing.                           |
| Phone             | <p>phone</p><p>phone_sha256</p> | Yes     | E.164 format (e.g., +15551234567) before hashing.          |
| First Name        | <p>fname</p><p>fname_sha256</p> | Yes     | Lowercase + trim before hashing.                           |
| Last Name         | <p>lname</p><p>lname_sha256</p> | Yes     | Lowercase + trim before hashing.                           |
| MAID              | maid                            | No      | Mobile Advertising ID (IDFA/AAID). Sent in plain text.     |
| Zip / Postal Code | postal\_code                    | No      | Sent in plain text. Used in combination with country/name. |
| Country Code      | country\_code                   | No      | ISO 3166-1 alpha-2 (e.g., "US"). Sent in plain text.       |



#### 2. MadConnect Standard Schema Example

Your source file or table should generally follow this structure to ensure seamless integration:

| Field Name    | Data Type | Required?       | Description                                                                                   |
| ------------- | --------- | --------------- | --------------------------------------------------------------------------------------------- |
| segment\_name | String    | Yes (New)       | Name of the audience (e.g., "Holiday Shoppers").                                              |
| segment\_id   | String    | Yes (Update)    | The segment ID as it appears in the destination platform (only if updating an existing list). |
| action        | String    | Yes             | add or remove. Use add for creation.                                                          |
| email\_sha256 | String    | At least one ID | User's email address.                                                                         |

#### Hashing Requirements

* Algorithm: SHA-256.
* Normalization: All PII (Email, Phone, Name) must be lowercased and trimmed of whitespace before hashing.
* Phone Numbers: Must be converted to[ E.164 format](https://en.wikipedia.org/wiki/E.164) before hashing.

***

### Important Notes and Resources

#### Common Errors

App Blocked / 403 Errors during OAuth If you see an "App Blocked" message during authentication:

* Third-party access blocked: Your Google Workspace admin may need to allow-list the MadConnect OAuth client ID.
* Advanced Protection Program: Accounts enrolled in this program may restrict third-party API access.
* Resolution: Have your Google Ads admin review "App Access Control" settings in the Google Admin console.

#### Disclosure

MadConnect's use and transfer of information received from Google APIs to any other app will adhere to[ Google API Services User Data Policy](https://developers.google.com/terms/api-services-user-data-policy), including the Limited Use requirements.

#### Resources

* [Google Ads API: Customer Match Guide](https://developers.google.com/google-ads/api/docs/remarketing/audience-segments/customer-match/get-started)
* [Google Ads: Customer Match Policy](https://support.google.com/adspolicy/answer/6299717)

<br>
