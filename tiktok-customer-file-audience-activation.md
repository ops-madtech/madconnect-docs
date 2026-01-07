# TikTok Customer File – Audience Activation

## TikTok Customer File – Audience Activation&#x20;

#### Getting Started & Prerequisites

MadConnect enables advertisers to activate first-party customer data directly within **TikTok Ads Manager** through **Customer File Audiences**. This integration allows marketers to securely upload hashed customer identifiers (emails, phone numbers, MAIDs) to create or update TikTok audiences for more personalized ad targeting.

***

### Connector Overview

| **Field**         | **Description**                                                                    |
| ----------------- | ---------------------------------------------------------------------------------- |
| Connector Type    | Destination                                                                        |
| Data Type         | Customer File Audience                                                             |
| Description       | Activate customer file audiences by syncing hashed customer identifiers to TikTok. |
| Supported Actions | Create / Add / Remove                                                              |

***

### Prerequisites

Before configuring TikTok in MadConnect, ensure the following:

#### Access Requirements

1. **Active TikTok Business Account:** Ensure the account is active, verified, and not under review or restricted for API use.
2. **Account Privileges:** You must have a TikTok Ads Business Manager user with sufficient privileges to manage Customer File Audiences.
3. **Audience Minimums:** A TikTok Custom Audience must contain at least **1,000 matched users** before it can be used in an ad group.
4. **EU Policy Compliance:** Advertisers targeting EU users must confirm compliance with the Politics, Governments, and Elections Advertising Policy by **January 10, 2026**, within TikTok Ads Manager (Account Settings → Compliance). Failure to confirm may restrict API access.

#### Authentication Requirements

You must authenticate your TikTok Ads account using **OAuth**.

**Option 1: OAuth Authentication (Recommended)**

OAuth provides the most secure and stable authentication.

Steps

1. Go to **My Platforms** → **TikTok Customer File** – **Audience Activation** → **Configure**.
2. Click **Sign in with TikTok**.
3. Log in with your TikTok Ads credentials.
4. Grant MadConnect permission to manage audiences.
5. Once successful, the platform will appear as **Configured** in My Platforms.

**One-Time OAuth Link (Copy Icon)**

A copy icon generates a **single-use OAuth URL**, which can be shared with a teammate who must complete authentication (e.g., if the admin is a different user).

***

### Connection Configuration (UI Fields)

When creating a TikTok Customer File connection in MadConnect, the destination configuration requires the following fields:

#### TikTok Destination Fields

| **Field**     | **Required** | **Description**                                                                          |
| ------------- | ------------ | ---------------------------------------------------------------------------------------- |
| Advertiser ID | Yes          | Your TikTok Advertiser ID (e.g., found in the URL parameter **aadvid**= in Ads Manager). |

<figure><img src=".gitbook/assets/unknown (5).png" alt=""><figcaption></figcaption></figure>

***

### Audience Creation & Management Logic

TikTok uses the following fields to manage audiences:

#### Segment Fields

| **Field**     | **Required**            | **Description**                                                                         |
| ------------- | ----------------------- | --------------------------------------------------------------------------------------- |
| segment\_name | Yes (for new audiences) | Name of the audience to be created in TikTok. Used only if segment\_id is not provided. |
| segment\_id   | Required for updates    | The TikTok-assigned ID for an existing audience. Used for add/remove operations.        |

#### Action Field

| **Field** | **Required** | **Description**                                                                 |
| --------- | ------------ | ------------------------------------------------------------------------------- |
| action    | Yes          | Indicates whether to add or remove users. Use add when creating a new audience. |

#### Creating a New Audience

1. Provide **segment\_name** value(s) but leave **segment\_id** empty.
2. Set the **action** field to **add**.
3. MadConnect will create the segment in TikTok.

#### Updating an Existing Audience

1. Provide the **segment\_id** of the existing audience.
2. Set the **action** field to **add** or **remove**.
3. MadConnect will append or remove users from that specific list.

***

### Matching Keys (TikTok Customer File)

To successfully send data to the TikTok API via MadConnect, your data must have one of the following matching fields.

MadConnect will normalize and hash any identifiers that platforms require if sending raw values.

#### Supported ID Fields

| **ID Type** | **Field Name**                  | **Hashed?** | **Notes**                                                                                        |
| ----------- | ------------------------------- | ----------- | ------------------------------------------------------------------------------------------------ |
| Email       | <p>email</p><p>email_sha256</p> | Yes         | SHA-256 hashed.                                                                                  |
| Phone       | <p>phone</p><p>phone_sha256</p> | Yes         | SHA-256 hashed.                                                                                  |
| MAID        | <p>maid</p><p>maid_sha256</p>   | Optional    | Mobile Advertising ID Use GAID (Android) or IDFA (iOS). Can be uploaded plain or SHA-256 hashed. |

#### 2. MadConnect Standard Schema Example

Your source file or table should generally follow this structure to ensure seamless integration:

| **Field Name** | **Data Type** | **Required?**   | **Description**                                                                               |
| -------------- | ------------- | --------------- | --------------------------------------------------------------------------------------------- |
| segment\_name  | String        | Yes (New)       | Name of the audience (e.g., "Holiday Shoppers").                                              |
| segment\_id    | String        | Yes (Update)    | The segment ID as it appears in the destination platform (only if updating an existing list). |
| action         | String        | Yes             | add or remove. Use add for creation.                                                          |
| email\_sha256  | String        | At least one ID | User's email address.                                                                         |

#### Hashing & Normalization Requirements

Before hashing with **SHA-256**, data must be normalized as follows:

1. **Email**:
   * Convert to **lowercase**.
   * **Trim** leading/trailing spaces.
2. **Phone**:
   * E.164 format, "+"\[country code]\[phone number]
3. **MAID**:
   * Can be sent raw or hashed.
   * Case: all uppercase or all lowercase
   * Remove any spaces at the beginning or end

***

### Important Notes and Resources

#### Audience Eligibility

1. Minimum Size: Audiences must reach at least **1,000 matched users** to be active and usable in campaigns.
2. **Rate Limits**: Audience creation and updates are subject to TikTok’s moderation and API rate limits.

#### Compliance

1. EU Policy: As noted in prerequisites, failure to confirm compliance with **TikTok's Politics, Governments, and Elections Advertising Policy by Jan 10, 2026**, may result in restricted API access.

#### Resources

1. [TikTok Business API: Customer File Audiences](https://business-api.tiktok.com/portal/docs?id=1747012327159809)
2. [TikTok Business API: File Upload Requirements](https://business-api.tiktok.com/portal/docs?id=1739940567842818)
