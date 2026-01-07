# Yahoo Traffic API - Audiences

## **Yahoo DSP – Audience Connector Overview**

MadConnect enables seamless audience activation on Yahoo DSP by syncing hashed emails and phone numbers to existing audience segments. This connector allows advertisers to keep their Yahoo DSP segments updated with first-party identifiers for more effective targeting.

### **Connector Overview**

* **Source/Destination**: Destination
* **Connector Type**: Audience
* **Data Type**: Hashed Emails / Phones
* **Description**: Sync your Email and Phone Numbers to update audiences in Yahoo DSP for activation.
* **Supported Actions**: Create, Add, Remove

***

### **Prerequisites**

1. **Enable DSP API**
   * Contact your Yahoo DSP Account Manager or Product Support to enable DSP API access for your account.
   * If the DSP API is not enabled, you will see a message in MadConnect indicating that the Client ID and Client Secret are "undefined."
2. **Client Credentials Required**
   * **Client ID**
   * **Client Secret**
3. **Steps to Obtain Client ID and Secret**:
   * Log into Yahoo DSP UI.
   * Click your name in the upper-right corner.
   * Select **My Account** and then click the **Activate** button.
   * Agree to the terms of service.
   * Copy and store the provided Client ID and Client Secret securely.

***

### **Configure Connector**

1. **Navigate to My Platforms Section**
   * Go to **My Platforms** in the MadConnect UI.
2. **Add New Platform**
   * Click **Add Platform**.
3. **Select Yahoo DSP – Custom Audiences**
   * Choose the **Yahoo DSP – Custom Audiences** tile and click **Configure**.
4. **Go to Configuration**
   * Open the **Configuration** tab.
5. **Enter Credentials**
   * Input your Yahoo DSP **Client ID** and **Client Secret**.
6. **Verify Connection**
   * Confirm the connector status updates to **Configured** in the My Platforms section.

***

### **Audience Schema Requirements for Yahoo DSP – Custom Audiences**

To successfully sync data to Yahoo DSP, your dataset must match the following schema:

1. **ID Field**
   * **Field Name**: `email_sha256`
   * **Data Type**: String (SHA-256 Hashed)
   * **Description**: Hashed email addresses of audience members. Ensure data is normalized (lowercase, trimmed) prior to hashing.
   * **Example**: `5d41402abc4b2a76b9719d911017c592`
2. **Segment ID Field**
   * **Field Name**: `segment_id`
   * **Data Type**: String
   * **Description**: The unique segment ID in Yahoo DSP that you want to update.
   * **Example**: `123456`
3. **Segment Name Field**
   * **Field Name**: `segment_name`
   * **Data Type**: String
   * **Description**: The name of the audience to be created in Google Ads. Used only when creating new audiences.
   * **Example**: `q1_audience`
4. **Action Field**
   * **Field Name**: `action`
   * **Data Type**: String
   * **Accepted Values**: `add`
   * **Description**: Defines whether the record is being added to the segment. Only `add` is currently supported.
   * **Example**: `add`

> 🔒 Only hashed identifiers are accepted. For improved match rates, ensure identifiers are preprocessed and SHA-256 hashed correctly.

***

### 💡 **How the Schema Works in MadConnect**

* **Audience Creation**:
  * If a `segment_name` is provided and `segment_id` is not, MadConnect will create a new audience in DV360 using the provided name.
  * The `action` field must be set to `add` during audience creation.
* **Managing Existing Audiences**:
  * If a `segment_id` exists, IDs will be added or removed based on the `action` field.
  * Any valid ID field sent to DV360 that the platform supports will be used for matching. This includes hashed emails, hashed phone numbers, MAIDs, and multi-field combinations like zip code, country code, and hashed names.
* **UI Enhancements**:
  * Users can configure additional metadata (e.g., audience source, lifespan) via drop downs in the MadConnect UI.
  * These settings can be modified at the connection level as needed.

For more information on Yahoo DSP Authentication, please review the [Yahoo DSP Documentation](https://developer.yahooinc.com/dsp/api/docs/traffic/audience/about-audience.html).

***

<br>
