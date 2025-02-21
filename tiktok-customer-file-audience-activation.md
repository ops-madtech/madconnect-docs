# TikTok Customer File – Audience Activation

## **TikTok Customer File – Audience Activation Overview**

MadConnect empowers advertisers to activate their customer data on TikTok, enabling precise targeting through TikTok’s Customer File Audience Activation. This connector allows businesses to upload customer lists to TikTok Ads Manager, utilizing various identifiers to reach specific audience segments effectively.

### **Connector Overview**

* **Connector Type**: Destination
* **Data Type**: Customer File Audience
* **Description**: TikTok Customer File Audience Activation allows advertisers to reach existing customers by uploading customer files with hashed identifiers such as email, phone number, or Mobile Advertising ID (MAID).
* **Supported Actions**: Create /Upload

### **Prerequisites**

1. **Active TikTok Ads Account**
   * Ensure you have an active TikTok Ads account with permissions to manage Customer File Audiences.
2. **OAuth Authentication**
   * Authenticate your TikTok Ads account within MadConnect using OAuth.
3. **Ensure the data for activation contains necessary inputs:**
   * Confirm that the audience data aligns with MadConnect’s standard schema, including required fields such as user\_id, segment\_id, segment\_name, and action.

### **Configure Connector**

1. **Navigate to the My Platforms Section**
   * Go to "My Platforms" in the MadConnect UI.
2. **Add a New Platform**
   * Click on "Add Platform."
3. **Select TikTok Customer File – Audience Activation**
   * Choose the "TikTok Customer File – Audience Activation" tile and click "Configure."
4. **Go to Destination Configuration**
   * Open the "Destination Configuration" tab.
5. **Sign in with TikTok**
   * Click "Sign in with TikTok" and log in using your TikTok Ads credentials.
6. **Authorize MadConnect**
   * Grant MadConnect permission to access and manage your TikTok Customer File audiences.
7. **Verify Configuration**
   * Ensure the platform status is marked as "Configured" under My Platforms.

### **Schema Requirements for TikTok Customer File – Audience Activation**

To successfully send data to **TikTok Customer File Audiences** via **MadConnect**, the following minimum schema must be used:

1. **ID Field**
   * **Field Name:** `email_hash`, `phone_hash`, `maid`
   * **Data Type:** String (Hashed for personal identifiers; plain or hashed for MAID)
   * **Description:** Contains the identifiers used for audience matching on TikTok.
   * **Supported Identifiers:**
     * **email\_hash** – SHA-256 hashed email addresses.
     * **`phone_hash`** – SHA-256 hashed phone numbers.
     * **`maid`** – Mobile Advertising ID (uploaded in plain text or hashed using SHA-256).
       * **Example:**
         * Email: `5d41402abc4b2a76b9719d911017c592`
         * Phone: `98f6bcd4621d373cade4e832627b4f6`
         * MAID: `cdda802e-fb9c-47ad-0794d394c912`
2. **Segment ID Field**
   * **Field Name:** `segment_id`
   * **Data Type:** String
   * **Description:** The unique ID assigned by TikTok Ads for the specific audience segment.
     * Used for adding/removing members from existing audiences.
   * **Example:** `987654321`
3. **Segment Name Field**
   * **Field Name:** `segment_name`
   * **Data Type:** String
   * **Description:** The name of the audience to be created in TikTok.
     * If a `segment_id` is not provided, MadConnect will use the `segment_name` to create a new audience in TikTok.
   * **Example:** `Holiday Campaign Audience`
4. **Action Field**
   * **Field Name:** `action`
   * **Data Type:** String
   * **Description:** Specifies whether to **add** or **remove** the user from the audience.
     * Required for both audience creation and member management.
   * **Accepted Values:** `add`, `remove`
   * **Example:** `add`

***

### **💡 How the Schema Works in MadConnect:**

1. **Audience Creation:**
   * If a **`segment_name`** is provided and **`segment_id`** is not, MadConnect will **create a new audience** in TikTok using the provided name.
   * The **`action`** field must be set to `add` during audience creation.
2. **Managing Existing Audiences:**
   * If a **`segment_id`** exists, IDs will be **added or removed** based on the **`action`** field.
   * Multiple identifiers can be included in a single request (e.g., **email**, **phone**, and **MAID**) to maximize match rates.
3. **Accepted Hashing Algorithm:**
   * **SHA-256** is required for **emails** and **phone numbers**.
   * **MAIDs** can be uploaded in plain text or hashed using SHA-256.
4. **Data Normalization Before Hashing:**
   * **Email:**
     * Remove whitespace and convert to lowercase.
     * Normalize by removing '+' and any following characters before '@'.
     * Remove all '.' characters in the username before '@'.
   * **Phone:**
     * Remove symbols, letters, and leading zeroes.
     * Include the country code if applicable.
   * **MAID:**
     * Can be uploaded unhashed or hashed.

For additional guidance on TikTok Customer File Audience Activation, refer to the [TikTok Business API documentation on Customer File Audiences](https://business-api.tiktok.com/portal/docs?id=1747012327159809) and [File Upload Requirements](https://business-api.tiktok.com/portal/docs?id=1739940567842818).
