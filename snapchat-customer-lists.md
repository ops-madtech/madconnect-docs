# SnapChat- Customer Lists

![](<.gitbook/assets/image (5).png>)

#### Snapchat Marketing API for Customer Lists - Connection Overview

MadConnect integrates seamlessly with the Snapchat Marketing API, enabling you to manage customer lists by adding or removing records for targeted advertising. This connector allows you to efficiently sync your customer data directly to Snapchat, ensuring your campaigns reach the right audience.

***

**Connector Overview**

* **Source / Destination**: Destination
* **Data Type**: Audience
* **Description**: Manage Snapchat customer lists by creating audiences or updating existing audiences by adding or removing members.
* **Supported Actions**: Create / Add / Remove

***

**Prerequisites**

To set up the Snapchat Marketing API for Customer Lists in MadConnect, ensure the following prerequisites are met:

1. **Authenticate using OAuth:**
   * Navigate to the connector configuration and authenticate your Snapchat account using OAuth.
2. **Ensure the data for activation contains necessary inputs:**
   * Confirm that the audience data aligns with MadConnect’s standard schema, including required fields such as user\_id, segment\_id, segment\_name, and action.

***

**Configure Connector**

1. **Navigate to the My Platforms Section:**
   * In the MadConnect UI, go to the "My Platforms" section.
2. **Add a New Platform:**
   * Click on "Add Platform."
3. **Select Snapchat - Customer Lists:**
   * Choose the "Snapchat - Customer Lists" tile and click on "Configure."
4. **Go to Configuration:**
   * Navigate to the "Configuration" tab.
5. **Sign in with Snapchat:**
   * Click the "Sign in with Snapchat" button.
6. **Authenticate with Snapchat:**
   * You will be redirected to Snapchat’s login page. Sign in using your Snapchat account credentials.
7. **Authorize MadConnect:**
   * Grant MadConnect permission to access and manage your Snapchat account.
8. **Verify Configuration:**
   * Ensure the connector status is marked as "Configured" under My Platforms.

***

\
**Where Can I Find My Snapchat Segment ID?**

To obtain your Snapchat Segment ID, follow these steps:

1. Log in to your Snapchat Ads Manager: Go to your dashboard.
2. Go to Assets > Audiences: Under the Audience Library, select the required audience.
3. Copy the Segment ID: The ID appears after /audiences/custom-audience/ in the URL of the resulting page.

***

#### **Audience Schema Requirements for Snapchat Ads – Custom Audiences**

To successfully send data to **Snapchat Ads** as audiences via **MadConnect**, the following minimum schema must be used:

1. **ID Field**
   * **Field Name:** `email_sha256`, `email_sha256`, `maid_sha256`
   * **Data Type:** String (Hashed for personal identifiers; unhashed for MAIDs)
   * **Data :** String (Hashed for personal identifiers; unhashed for MAIDs)
   * **Description:**
     * Contains audience member identifiers used for matching in Snapchat.
     * **Only one type of identifier can be used per request** — mixing multiple identifier types (e.g., emails and phone numbers) in a single request is **not supported**.
   * **Supported ID Types:**
     * `email_sha256` – SHA-256 hashed email addresses.
       * _Before hashing:_ Remove whitespace and convert to lowercase.
       * _Example:_ `5d41402abc4b2a76b9719d911017c592`
     * `email_sha256` – SHA-256 hashed phone numbers.
       * _Before hashing:_ Remove symbols, letters, and leading zeroes. Include the country code if applicable.
       * _Example:_ `98f6bcd4621d373cade4e832627b4f6`
     * `maid_sha256` – SHA-256 hashed Mobile Advertising ID (MAID).
       * _Example:_ `cdda802e-fb9c-47ad-0794d394c912`

💡 _For a complete list of supported match identifiers and formatting guidelines, review the official_ [_Snapchat Ads API Documentation_](https://developers.snap.com/api/marketing-api/Ads-API/customer-lists)_._

2. **Segment ID Field**
   * **Field Name:** `segment_id`
   * **Data Type:** String
   * **Description:**
     * The unique ID assigned by Snapchat Ads for the specific audience segment.
     * Used for adding/removing members from existing audiences.
     * _Example:_ `123456`
3. **Segment Name Field**
   * **Field Name:** `segment_name`
   * **Data Type:** String
   * **Description:**
     * The name of the audience to be created in Snapchat.
     * If a `segment_id` is not provided, MadConnect will use the `segment_name` to create a new audience in Snapchat and return the assigned `segment_id`.
     * _Example:_ `Holiday Campaign Audience`
4. **Action Field**
   * **Field Name:** `action`
   * **Data Type:** String
   * **Description:**
     * Specifies whether to **add** or **remove** the user from the audience.
     * Required for both audience creation and member management.
     * **Accepted Values:** `add`, `remove`
     * _Example:_ `add`

***

#### **💡 How the Schema Works in MadConnect:**

1. **Audience Creation:**
   * If a **`segment_name`** is provided and **`segment_id`** is not, MadConnect will **create a new audience** in Snapchat using the provided name.
   * The **`action`** field must be set to `add` during audience creation.
2. **Managing Existing Audiences:**
   * If a **`segment_id`** exists, IDs will be **added or removed** based on the **`action`** field.
   * **Only one identifier type can be used per request.** For example, a request can include only **hashed emails** or only **hashed phone numbers**, but not both.
3. **Continuous Audience Updates:**
   * Users can be **added to a segment at any time**.
   * Snapchat supports flexible matching using either **email**, **phone number**, or **mobile advertising IDs**, but only **one type per request**.
4. **UI Enhancements:**
   * Users can configure additional metadata (e.g., **audience source**, **lifespan**) using drop downs in the **MadConnect UI**.
   * These settings can be modified at the **connection level** as needed.



For more detailed instructions on the Snapchat Marketing API and managing customer lists, refer to the [Snapchat Marketing API](snapchat-customer-lists.md#snapchat-marketing-api-for-customer-lists-connection-overview) documentation.
