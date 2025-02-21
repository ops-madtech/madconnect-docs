# Google DV360

![](<.gitbook/assets/image (24).png>)

#### Google Display & Video 360 Customer Match Connector Overview

MadConnect enables seamless integration with DV360 Customer Match, allowing you to upload and manage customer data for targeted advertising. Leverage your first-party data to create personalized ad campaigns, ensuring optimal reach and engagement. Maximize your advertising impact with efficient data synchronization directly from your CRM or data sources to DV360.

***

**Connector Overview**

* **Source / Destination**: Destination
* **Data Type**: Audience
* **Description**: Activate your customer match audiences on Google DV360 by syncing your customer data.
* **Supported Actions**: Create / Add / Remove

***

**Prerequisites**

To activate audiences on Google DV360 using MadConnect, ensure the following prerequisites are met:

1. **Meet Google's Requirements for Using Customer Match**:
   * **Policy Compliance**: The advertiser account must have a good history of policy compliance.
   * **Payment History**: The advertiser account must have a good payment history.
   * **Allowlisting**: Customers with compliant accounts are automatically allowlisted by Google.
2. **Account Requirements**:
   * **OAuth Authentication**: Authenticate Google DV360 Account destination using OAuth.
   * **Compliant Advertiser Account**: Ensure the account adheres to Google's Customer Match requirements.
3. **Credentials**:
   * You can obtain your Google DV360 Account Credentials from your account settings in the Google Marketing Platform.

Warning: Verify that data upload features have been approved for the proposed audience's parent partner by first trying to create a Customer Match audience under a relevant advertiser in the UI. If data upload has not been enabled, attempts to create or update Customer Match audiences will return an error.

***

**Configure Connector**

To configure the DV360 Customer Match connector in MadConnect, follow these steps for authentication:

1. **Navigate to the My Platforms Section**:
   * In the MadConnect UI, go to the **My Platforms** section.
2. **Add a New Platform**:&#x20;
   * Click on **Add Platform**.
3. **Select Google DV360 - Customer Match**:&#x20;
   * Choose the **Google DV360 - Customer Match** tile and click on **Configure**.
4. **Go to Destination Configuration**:&#x20;
   * Navigate to the **Configuration** tab.
5. **Sign in with Google**:&#x20;
   * Click the **Sign in with Google** button to authenticate your Google DV360 account.
6. **Authenticate with Google**:&#x20;
   * You will be redirected to Google’s OAuth login page. Sign in using your Google account credentials.
7. **Authorize MadConnect:**&#x20;
   * Grant MadConnect permission to access and manage your DV360 Customer Match audiences.
8. **Redirect to MadConnect**:&#x20;
   * After successful authentication, you will be redirected back to MadConnect, where the configuration will be completed.

***



#### **Audience Schema Requirements for DV360 – Customer Match**

To successfully send data to **Google DV360 Customer Match API** via **MadConnect**, the following minimum schema must be used:

1. **ID Field**
   * **Field Name:** `email_hash`, `phone_hash`, `maid`, `zip_code`, `fname_hash`, `lname_hash`, `country_code`
   * **Data Type:** String (Hashed for personal identifiers; plain for MAID and location data)
   * **Description:** Contains audience member identifiers used for matching in DV360. The following ID types are supported:
     * `email_hash` – SHA-256 hashed email addresses.
       * _Before hashing:_ Remove all whitespace and convert to lowercase.
       * _Example:_ `5d41402abc4b2a76b9719d911017c592`
     * `phone_hash` – SHA-256 hashed phone numbers.
       * _Before hashing:_ Format using **E.164** (includes country calling code).
       * _Example:_ `98f6bcd4621d373cade4e832627b4f6`
     * `zip_code` – Member’s postal zip code.
       * _Must be used with:_ `country_code`, `fname_hash`, and `lname_hash`.
       * `fname_hash` – SHA-256 hashed first name.
       * _Before hashing:_ Remove whitespace and convert to lowercase.
       * _Example:_ `6dcd4ce23d88e2ee9568ba546c007c63`
       * _Must be used with:_ `country_code`, `lname_hash`, and `zip_code`.
     * `lname_hash` – SHA-256 hashed last name.
       * _Before hashing:_ Remove whitespace and convert to lowercase.
       * _Example:_ `7c6a180b36896a0a8c02787eeafb0e4c`
       * _Must be used with:_ `country_code`, `fname_hash`, and `zip_code`.
     * `country_code` – Two-letter country code of the member.
       * _Example:_ `US`
       * _Must be used with:_ `fname_hash`, `lname_hash`, and `zip_code`.
     * `maid` – Mobile Advertising ID (non-hashed).
       * _Example:_ `cdda802e-fb9c-47ad-0794d394c912`

_For a complete list of supported match identifiers and formatting guidelines, review the official_ [_DV360 Customer Match Documentation._](https://developers.google.com/display-video/api/reference/rest/v3/firstAndThirdPartyAudiences#contactinfo)

2. **Segment ID Field**
   * **Field Name:** `segment_id`
   * **Data Type:** String
   * **Description:** The unique ID assigned by Google for the specific audience segment.
   * **Example:** `123456`
3. **Segment Name Field**
   * **Field Name:** `segment_name`
   * **Data Type:** String
   * **Description:** The name of the audience to be created in DV360. If a `segment_id` is not provided, MadConnect will use the `segment_name` to create a new audience in DV360 and return the assigned `segment_id`.
   * **Example:** `Spring Sale Audience`
4. **Action Field**
   * **Field Name:** `action`
   * **Data Type:** String
   * **Description:** Specifies whether to **add** or **remove** the user from the audience.
   * **Accepted Values:** `add`, `remove`
   * **Example:** `add`

***

#### **💡 How the Schema Works in MadConnect:**

1. **Audience Creation:**
   * If a **`segment_name`** is provided and **`segment_id`** is not, MadConnect will **create a new audience** in DV360 using the provided name.
   * The **`action`** field must be set to `add` during audience creation.
2. **Managing Existing Audiences:**
   * If a **`segment_id`** exists, IDs will be **added or removed** based on the **`action`** field.
   * **Any valid ID field sent to DV360 that the platform supports will be used for matching.** This includes hashed emails, hashed phone numbers, MAIDs, and multi-field combinations like zip code, country code, and hashed names.
3. **UI Enhancements:**
   * Users can configure additional metadata (e.g., **audience source**, **lifespan**) via drop downs in the **MadConnect UI**.
   * These settings can be modified at the **connection level** as needed.

For more information on Google DV360 Audience Activation, please review the[ Google Customer Match documentation](https://developers.google.com/display-video/api/guides/audiences/upload-customer-match).

***

_**Disclosure**_**:** _MadConnect's use and transfer of information received from Google APIs to any other app will adhere to_ [_Google API Services User Data Policy_](https://developers.google.com/terms/api-services-user-data-policy#additional_requirements_for_specific_api_scopes)_, including the Limited Use requirements._
