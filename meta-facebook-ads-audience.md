# META / Facebook Ads - Audience

![](<.gitbook/assets/image (16).png>)

#### Meta Ads -Custom  Audiences Overview

MadConnect integrates seamlessly with Meta’s Custom Audiences API, enabling advertisers to effectively manage and target their audiences on Meta platforms, including Facebook and Instagram. With this connector, businesses can sync customer data directly from their data sources to Meta, ensuring more precise and impactful audience targeting.

***

**Connector Overview**

* **Connector Type**: Destination
* **Data Type**: Custom Audiences
* **Description**: Custom audiences allow advertisers to target their ads to a specific set of people with whom they have already established a relationship.
* **Supported Actions**: Create / Add / Remove

***

#### Prerequisites

1. **Active Meta Ads Account**
   * Ensure you have an active Meta Ads account with permissions to manage Custom Audiences.
2. **OAuth Authentication**
   * Authenticate your Meta Ads account using OAuth within MadConnect.

***

#### Configure Connector

1. **Navigate to the My Platforms Section**
   * Go to "My Platforms" in the MadConnect UI.
2. **Add a New Platform**
   * Click on "Add Platform."
3. **Select Meta - Custom Audiences**
   * Choose the "Meta - Custom Audiences" tile and click "Configure."
4. **Go to Configuration**
   * Open the "Configuration" tab.
5. **Sign in with Meta**
   * Click "Sign in with Meta" and log in using your Meta account credentials.
6. **Authorize MadConnect**
   * Grant MadConnect permission to access and manage your Meta Custom Audiences.
7. **Verify Configuration**
   * Ensure the platform status is marked as "Configured" under My Platforms.



***

#### **Audience Schema Requirements for Meta Ads – Custom Audiences**

To successfully send data to **Meta Custom Audiences** via **MadConnect**, the following minimum schema must be used:

**ID Field**

* **Field Name:** `email_hash`, `phone_hash`, `maid`
* **Data Type:** String (Hashed for emails and phone numbers; plain for MAID)
* **Description:** Contains the hashed emails, phone numbers, or plain Mobile Advertising IDs (MAIDs) of the audience members.
* **Accepted Hashing Algorithm:** SHA-256
* **Example:**
  * Email: `5d41402abc4b2a76b9719d911017c592`
  * Phone: `98f6bcd4621d373cade4e832627b4f6`
  * MAID: `cdda802e-fb9c-47ad-0794d394c912`

**Segment ID Field**

* **Field Name:** `segment_id`
* **Data Type:** String
* **Description:** The unique identifier for the specific audience segment assigned by Meta.
* **Example:** `987654321`

**Segment Name Field**

* **Field Name:** `segment_name`
* **Data Type:** String
* **Description:** The name of the audience segment to be created in Meta. If a `segment_id` is not provided, MadConnect will use the `segment_name` to create a new audience in Meta.
* **Example:** `Winter Campaign Audience`

**Action Field**

* **Field Name:** `action`
* **Data Type:** String
* **Description:** Specifies the intended action for the audience member—either adding or removing the user from the segment.
* **Accepted Values:** `add`, `remove`
* **Example:** `add`

***

#### **💡 How the Schema Works in MadConnect:**

* **Audience Creation:**
  * If a `segment_name` is provided and `segment_id` is missing, MadConnect will automatically create a **new audience** in Meta using the provided name.
  * **Action must be `add`** when creating a new audience.
* **Managing Existing Audiences:**
  * If `segment_id` is provided, IDs will be **added** or **removed** based on the `action` field.
  * The **`id`** field serves as the matching identifier (e.g., `email_hash`, `UID2`, `MAID`), ensuring proper alignment with Meta’s audience matching requirements.
* **UI Enhancements:**
  * When creating a new audience, additional fields (e.g., audience source, duration) can be configured directly within the **MadConnect UI**.
  * These settings can be updated at the **connection level** as needed.

For more details on Meta Custom Audiences, please refer to the[ Meta Custom Audiences documentation](https://www.facebook.com/business/help/744354708981227).
