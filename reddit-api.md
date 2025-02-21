# Reddit API

![](<.gitbook/assets/image (46).png>)

## **Reddit Ads – Audience Connector Overview**

MadConnect integrates with Reddit Ads, enabling advertisers to seamlessly sync and manage their audiences. With this connector, businesses can efficiently target specific user segments on Reddit by leveraging existing customer data, ensuring relevant ad delivery and higher engagement.

***

### **Connector Overview**

* **Connector Type**: Destination
* **Data Type**: Audience
* **Description**: Audiences in Reddit Ads allow advertisers to target specific groups of users based on hashed emails or Mobile Advertising IDs (MAIDs).
* **Supported Actions**: Create / Add / Remove

***

### **Prerequisites**

1. **Active Reddit Ads Account**
   * Ensure you have an active Reddit Ads account with the necessary permissions to manage audiences.
2. **OAuth Authentication**
   * Authenticate your Reddit Ads account using OAuth within MadConnect.
3. **Ensure the data for activation contains necessary inputs:**
   * Confirm that the audience data aligns with MadConnect’s standard schema, including required fields such as user\_id, segment\_id, segment\_name, and action.

***

### **Configure Connector**

1. **Navigate to the My Platforms Section**
   * Go to "My Platforms" in the MadConnect UI.
2. **Add a New Platform**
   * Click on "Add Platform."
3. **Select Reddit Ads – Audience Connector**
   * Choose the "Reddit Ads – Audience" tile and click "Configure."
4. **Go to Destination Configuration**
   * Open the "Destination Configuration" tab.
5. **Sign in with Reddit**
   * Click "Sign in with Reddit" and log in with your account credentials.
6. **Authorize MadConnect**
   * Grant MadConnect permission to access and manage your Reddit audiences.
7. **Verify Configuration**
   * Ensure the platform status is marked as "Configured" under My Platforms.

***

### **Schema Requirements for Reddit Ads – Audience Connector**

To successfully sync data with **Reddit Ads** via **MadConnect**, the following schema must be used:

* **ID Field**
  * **Field Name:** `email_hash`, `maid_hash`
  * **Data Type:** String (Hashed)
  * **Description:** Contains the hashed emails and/or Mobile Advertising IDs (MAIDs) used for audience matching.
    * **Both identifiers can be uploaded together** in the same request to improve match accuracy.
  * **Accepted Identifiers:**
    * **`email_hash`** – SHA-256 hashed email addresses.
    * **`maid_hash`** – SHA-256 hashed Mobile Advertising IDs.
  * **Example:**
    * Email: `5d41402abc4b2a76b9719d911017c592`
    * MAID: `cdda802e-fb9c-47ad-0794d394c912`
* **Segment ID Field**
  * **Field Name:** `segment_id`
  * **Data Type:** String
  * **Description:** The unique identifier assigned by Reddit Ads for the specific audience segment.
    * Used for adding/removing members from existing audiences.
  * **Example:** `123456789`
* **Segment Name Field**
  * **Field Name:** `segment_name`
  * **Data Type:** String
  * **Description:** The name of the audience to be created in Reddit Ads.
    * If a `segment_id` is not provided, MadConnect will use the `segment_name` to create a new audience in Reddit and return the assigned `segment_id`.
  * **Example:** `Holiday Promo Audience`
* **Action Field**
  * **Field Name:** `action`
  * **Data Type:** String
  * **Description:** Specifies whether to **add** or **remove** the user from the audience.
    * Required for both audience creation and member management.
  * **Accepted Values:** `add`, `remove`
  * **Example:** `add`

***

### **💡 How the Schema Works in MadConnect:**

1. **Audience Creation:**
   * If a **`segment_name`** is provided and **`segment_id`** is not, MadConnect will **create a new audience** in Reddit Ads using the provided name.
   * The **`action`** field must be set to `add` during audience creation.
2. **Managing Existing Audiences:**
   * If a **`segment_id`** exists, IDs will be **added or removed** based on the **`action`** field.
   * **Both hashed emails and MAIDs can be included in a single request** to optimize audience matching.
3. **Accepted Hashing Algorithm:**
   * **SHA-256** is required for both **emails** and **MAIDs**.
4. **Data Normalization Before Hashing:**
   * **Email:**
     * Remove whitespace and convert to lowercase.
     * _Example:_ `Example.Email+test@gmail.com` → `exampleemail@gmail.com`
   * **MAID:**
     * Remove whitespace and ensure all letters are lowercase before hashing.

For more details on Reddit Ads audiences, please refer to the [Reddit Ads documentation](https://ads-api.reddit.com/docs/v3/operations/Update%20Custom%20Audience%20Users).
