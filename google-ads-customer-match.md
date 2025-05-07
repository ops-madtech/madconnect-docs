# Google Ads - Customer Match

![](https://lh7-us.googleusercontent.com/3_4u7MdvEDTHiJXHBwoG-YO8YNPJ1HcdONprxuMMbRp-H4rh8V3VWtH99_m6OSo_7OWm-MoqX9FG-Df2tjoJcdF_-yJpnsFQSdpfC7OEHsa--UuqQpy47CobMMnrky4d2bmETU4ZM9G2fRnFxCFwFA)

## **Google Ads – Customer Match Connector Overview**

MadConnect enables seamless integration with the Google Ads Customer Match API, allowing you to upload first-party customer data for targeting across Search, Gmail, YouTube, Display, and the Shopping tab. This connector automates the audience sync workflow, giving you control over list creation, population, and removal from within the MadConnect platform.

### **Connector Overview**

* **Source/Destination**: Destination
* **Connector Type**: Audience
* **Data Type**: Customer Match (Hashed IDs)
* **Description**: Activate customer match audiences by syncing your first-party customer identifiers to Google Ads.
* **Supported Actions**: Add, Remove

***

### **Prerequisites**

* **Account Eligibility**: Your Google Ads account must meet Google’s eligibility criteria for Customer Match (good compliance and billing history).
* **OAuth Authentication**: MadConnect uses OAuth to authenticate your Google Ads account. You will be prompted to sign in and grant permission to manage audiences.
* **Required Permissions**: During OAuth, the following scopes are requested:
  * Manage your Google Ads campaigns
  * Manage your audience lists
  * View and manage your account reports

***

### **Configure Connector**

1. **Navigate to My Platforms**
   * Go to "My Platforms" in the MadConnect UI.
2. **Add a New Platform**
   * Click "Add Platform."
3. **Select Google Ads – Customer Match Connector**
   * Choose the **"Google Ads – Customer Match"** tile and click **"Configure."**
4. **Go to Configuration**
   * Open the **"Configuration"** tab.
5. **Authenticate via OAuth**
   * Click **"Sign in with Google Ads"** and complete the OAuth flow.
6. **Verify Configuration**
   * Ensure the platform status is marked as **"Configured"** under **My Platforms**.

***

### **Audience Creation Steps**

1. **Create a Segment (Optional)**
   * Provide a unique `segment_name`. If `segment_id` is not provided, MadConnect will create the segment in Google Ads and return the `segment_id`.
2. **Add Members**
   * Use the `action` field set to `add` to send new members to the segment.
3. **Remove Members**
   * Use the `action` field set to `remove` to delete members from the segment.

***

### **Audience Schema Requirements for Google Ads – Customer Match**

To successfully send data to the Google Ads Customer Match API via MadConnect, the following minimum schema must be used:

1.  **ID Field**

    * **Field Name**: `email_sha256`, `phone_sha256`, `maid`, `postal_code`, `fname_sha256`, `lname_sha256`, `country_code`
    * **Data Type**: String (Hashed for PII; plain for MAID and location fields)
    * **Description**:
      * `email_sha256`: SHA-256 hashed lowercase email address
      * `phone_sha256`: SHA-256 hashed phone number in E.164 format
      * `maid`: Mobile Advertising ID (non-hashed)
      * `postal_code`: Zip or postal code (used with `country_code`, `fname_sha256`, and `lname_sha256`)
      * `fname_sha256`: SHA-256 hashed lowercase first name
      * `lname_sha256`: SHA-256 hashed lowercase last name
      * `country_code`: ISO 3166-1 alpha-2 country code

    **Upload Guidance:**

    * For contact info, set `upload_key_type` to `CONTACT_INFO`.
      * **Before hashing:**
        * Trim whitespace.
        * Lowercase all values (email, names, address).
        * Format phone numbers to E.164 (e.g., `+12125650000`).
      * **Hashing:**
        * SHA-256 hash is required for email, phone, first/last names.
      * **Mailing address matching** requires:
        * `country_code`, `postal_code`, `fname_sha256`, and `lname_sha256`
    * For CRM IDs:
      * Set `upload_key_type` to `CRM_ID`
      * Provide advertiser-generated `third_party_user_id`
    * For Mobile IDs:
      * Set `upload_key_type` to `MOBILE_ADVERTISING_ID`
      * Provide the `maid` and `app_id`
2. **Segment ID Field**
   * **Field Name**: `segment_id`
   * **Data Type**: String
   * **Description**: The unique user list ID in Google Ads. Required for updating or removing existing audiences.
3. **Segment Name Field**
   * **Field Name**: `segment_name`
   * **Data Type**: String
   * **Description**: The name of the audience to be created in Google Ads. Used only when creating new audiences.
4. **Action Field**
   * **Field Name**: `action`
   * **Data Type**: String
   * **Accepted Values**: `add`, `remove`
   * **Description**: Defines whether the user should be added or removed from the audience.

***

### 💡 **How the Schema Works in MadConnect**

* **Audience Creation**:
  * If `segment_name` is provided without a `segment_id`, MadConnect creates the segment in Google Ads and stores the returned `segment_id`.
  * The `action` must be `add` when creating an audience.
* **Managing Existing Audiences**:
  * With a known `segment_id`, MadConnect will append or remove users based on the `action` field.
  * Any supported ID type can be used in the request.
* **UI Enhancements**:
  * Additional metadata such as `membership_life_span` and `upload_key_type` can be configured in the MadConnect UI when setting up the connection.

For more details, refer to the [Google Ads Customer Match documentation](https://developers.google.com/google-ads/api/docs/remarketing/audience-segments/customer-match).

_**Disclosure**_**:** _MadConnect's use and transfer of information received from Google APIs to any other app will adhere to_ [_Google API Services User Data Policy_](https://developers.google.com/terms/api-services-user-data-policy#additional_requirements_for_specific_api_scopes)_, including the Limited Use requirements._

