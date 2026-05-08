# Google Ads Data Manager - Audience

### Google Ads Data Manager - Audiences Overview

MadConnect integrates with Google’s Data Manager framework to support Customer Match audience workflows across Google destinations. With this connector, advertisers, agencies, and data partners can securely send first-party audience data from supported data sources into Google Ads or Display & Video 360 for audience activation and membership management.

This connector is intended for audience onboarding and ongoing audience updates using Google’s Data Manager-based workflow.

***

#### **Connector Overview**

| Field                 | Description                                                                                                                                                                       |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Connector Type**    | Destination                                                                                                                                                                       |
| **Data Type**         | Customer Match Audiences                                                                                                                                                          |
| **Description**       | Customer Match allows advertisers to use first-party customer data to create and update audiences in Google platforms for targeting, suppression, and audience refresh workflows. |
| **Supported Actions** | Create / Add / Remove                                                                                                                                                             |

***

#### Prerequisites

Before configuring Google Ads Data Manager Audiences in MadConnect, ensure the following:

**Google Access Requirements**

1. Active **Google Ads** or **Display & Video 360** account
2. Permission to manage audience workflows in the destination account
3. Access to the advertiser/account that will receive the audience data
4. Eligibility to use customer data for audience workflows under Google policies
5. Any required Google approvals for Data Manager API usage

***

**Authentication Requirements**

You must authenticate your Google environment using **OAuth**.

OAuth is required to allow MadConnect to create and manage audience workflows in the authorized Google account.

**Steps**

1. Go to **My Platforms → Google Ads Data Manager – Audiences → Configure**
2. Click **Sign in with Google**
3. Log in with the Google account that has access to the target destination account
4. Approve the requested permissions
5. Once successful, the platform will appear as **Configured** in My Platforms

**One-Time OAuth Link (Copy Icon)**

A copy icon generates a **single-use OAuth URL**, which can be shared with a teammate who must complete authentication.

**Notes:**

1. The link expires after opening the first time
2. A new link must be generated for additional authentication attempts

***

#### Connection Configuration (UI Fields)

When creating a Google Ads Data Manager Audiences connection in MadConnect, the destination configuration requires the following fields:

**Google Destination Fields**

| Field                          | Required | Description                                                                                                                                                                                                                                  |
| ------------------------------ | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Account Type**               | Yes      | Select the Google destination account type. Supported options are **Google Ads** and **Display & Video 360**.                                                                                                                                |
| **Advertiser ID**              | Yes      | Enter the destination advertiser/account ID for the selected Google product. For **Google Ads**, this is the **Google Ads Customer ID**. For **DV360**, this is the **DV360 Advertiser ID**. **Do not include dashes** when entering the ID. |
| **Audience Type**              | Yes      | Select the audience identifier family being used for the upload. Current options include **Contact Information** and **Mobile Advertising Id**.                                                                                              |
| **Data Source Type**           | Yes      | Specifies the customer data source type used for the audience upload.                                                                                                                                                                        |
| **Ad User Data Consent**       | Yes      | Indicates whether user data consent has been granted for the uploaded records.                                                                                                                                                               |
| **Ad Personalization Consent** | Yes      | Indicates whether uploaded records can be used for ad personalization.                                                                                                                                                                       |

These fields determine how MadConnect structures calls to Google’s Data Manager audience APIs.

***

#### Segment Fields, Actions, and Allowed IDs

Google uses audience metadata, action logic, and identifier type to determine how a Customer Match audience is created and updated in MadConnect.

**Audience and Segment Fields**

| Field             | Required                                 | Description                                                                                                                                  |
| ----------------- | ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **segment\_name** | Conditional (required for new audiences) | Name of the audience to be created in Google                                                                                                 |
| **segment\_id**   | Conditional (Required for updates)       | Google-assigned audience identifier                                                                                                          |
| **action**        | Yes                                      | `add` is used to create a new audience and/or add users to an existing audience. `remove` is used to remove users from an existing audience. |

***

#### **Allowed IDs by Audience Type**

Google Data Manager supports one identifier family per configured audience workflow. For this connector, the main audience types exposed in the UI are **Contact Information** and **Mobile Advertising Id**.

#### **Contact Information**

For **Contact Information** audiences, multiple identifiers can be provided for the same person in a single record. For example, a row can include both email and phone. Address fields can also be used when the required components are present.

| Google Allowed Field      | MadConnect Standard Field | Required?                 | Hashing / Normalization Guidance                                                                                               |
| ------------------------- | ------------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| **Email Address**         | `email` or `email_sha256` | Optional                  | Remove leading, trailing, and intermediate spaces, convert to lowercase, then hash using **SHA-256** if sending hashed values. |
| **Phone Number**          | `phone` or `phone_sha256` | Optional                  | Format according to **E.164** before hashing with **SHA-256** if sending hashed values.                                        |
| **First Name**            | `first_name`              | Required if using address | Should be normalized according to Google requirements before hashing when applicable.                                          |
| **Last Name**             | `last_name`               | Required if using address | Should be normalized according to Google requirements before hashing when applicable.                                          |
| **Country / Region Code** | `country`                 | Required if using address | Use the applicable country/region code.                                                                                        |
| **Postal Code**           | `postal_code`             | Required if using address | Use the applicable postal code format for the country.                                                                         |

**Contact Information Notes**

1. At least one contact identifier must be present for a contact-based upload
2. Email and phone may both be provided for the same person
3. If using address-based matching, the full required address components must be present
4. If pre-hashed fields are provided, they must already be normalized correctly before hashing

#### **Mobile Advertising Id**

For **Mobile Advertising Id** audiences, the source data must contain mobile advertising identifiers and the connection must be configured with the appropriate mobile context.

| Google Allowed Field       | MadConnect Standard Field | Required?                          | Hashing / Normalization Guidance                                        |
| -------------------------- | ------------------------- | ---------------------------------- | ----------------------------------------------------------------------- |
| **iOS Advertising ID**     | `idfa`                    | Optional                           | Plain mobile advertising ID. Requires **Key Space = IOS**.              |
| **Android Advertising ID** | `gaid`                    | Optional                           | Plain mobile advertising ID. Requires **Key Space = ANDROID**.          |
| **App ID**                 | Connection config field   | Required for Mobile Advertising Id | Required at the connector configuration level for mobile-based uploads. |

**Mobile Advertising Id Notes**

1. `IDFA` maps to **IOS**
2. `GAID` maps to **ANDROID**
3. A generic `MAID` field may not be sufficient unless the mobile platform is known
4. `App ID` should be configured at the connection level, not passed row by row
5. Contact Information and Mobile Advertising Id should not be mixed in the same configured audience workflow

***

#### Running Transfers in MadConnect

Once platform configuration and connection setup are complete:

1. Select your **source** (Snowflake, BigQuery, S3, GCS, etc.)
2. Select **Google Ads Data Manager – Audiences** as the destination
3. Configure:
   * Account Type
   * Advertiser ID
   * Audience Type
   * Data Source Type
   * Ad User Data Consent
   * Ad Personalization Consent
4. Map your source fields to the required MadConnect audience schema
5. Provide:
   * **segment\_name** for new audiences, or
   * **segment\_id** for existing audiences
6. Run a **Manual Transfer** or configure a **Scheduled Transfer**

MadConnect will:

1. Validate schema compatibility
2. Validate required destination configuration fields
3. Structure the upload based on the configured audience type
4. Normalize and hash identifiers where applicable
5. Create or update the audience in Google
6. Manage add/remove membership logic
7. Return metadata, counts, and errors in **Reports**

***

#### **Important Notes**

1. This connector supports both **Google Ads** and **Display & Video 360**
2. The **Advertiser ID** must be entered **without dashes**
   1. For Google Ads, use the **Google Ads Customer ID** with dashes removed
   2. For DV360, use the **DV360 Advertiser ID**
3. Audience type should align with the identifiers provided in the mapped source data
4. Providing multiple valid contact identifiers for the same user may improve match potential
5. Consent selections should reflect the data usage permissions associated with the uploaded records
6. Mobile Advertising Id workflows require additional mobile-specific configuration

***

#### **Additional Resources**

1. [Google Data Manager API Overview](https://developers.google.com/data-manager/api)
2. [Google Customer Match Documentation](https://developers.google.com/data-manager/api/devguides/audiences/google-ads/customer-match)
3. [Google Data Manager API Reference](https://developers.google.com/data-manager/api/reference/rest)
