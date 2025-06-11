# Criteo Retail Media - Audience Activation

## **Criteo Retail Media - Audience Connector Overview**

### **Connector Overview**

| Source / Destination | Destination                                                                                                                                                                                                                                                   |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Connector Type       | Audience Activation                                                                                                                                                                                                                                           |
| Data Type            | Audience                                                                                                                                                                                                                                                      |
| Description          | Sync first-party audience data to **Criteo Retail Media**, enabling precise targeting for retail media campaigns. This connector allows advertisers to create and update audience segments for personalized ad delivery within Criteo's retail media network. |
| Supported Actions    | Create / Add / Remove                                                                                                                                                                                                                                         |

***

### **Prerequisites**

To activate the connector, ensure the following:

1. **Active Criteo Retail Media Account**
   * Ensure you have an active Criteo Retail Media account with the necessary permissions to manage audience segments.
2. **Authentication Requirements**
   * **OAuth Authentication**: Authenticate your Criteo Retail Media account using OAuth within MadConnect.
3. **Necessary Information:**
   * **RetailerID** is mandatory when creating a segment.

***

### **Configure Connector**

To configure the **Criteo Retail Media - Audience** API connector in MadConnect, follow these steps:

1. **Navigate to the My Platforms Section**
   * In the **MadConnect UI**, go to **"My Platforms"**.
2. **Add a New Platform**
   * Click on **"Add Platform"**.
3. **Select Criteo Retail Media - Audience**
   * Choose the **"Criteo Retail Media - Audience"** tile and click **"Configure"**.
4. **Go to Configuration**
   * Navigate to the **"Configuration"** tab.
5. **Sign in with Criteo**
   * Click the **"Sign in with Criteo"** button.
6. **Authenticate with Criteo**
   * You will be redirected to **Criteo’s login page**. Sign in using your Criteo account credentials.
7. **Authorize MadConnect**
   * Grant MadConnect permission to manage audiences in Criteo Retail Media.
8. **Verify Configuration**
   * Ensure the connector status is marked as **"Configured"** under **My Platforms**.

***

### **Audience Schema Requirements for Criteo Retail Media - Audience Connector**

To successfully send audience data to Criteo Retail Media via MadConnect, ensure your data follows the required schema:

1. **ID Field**
   * **Field Name**: `email_sha256`, `phone_sha256`, `maid`
   * **Data Type**: String (Hashed using SHA-256)
   * **Description**: Contains user identifiers that Criteo will use for audience matching.
   * **Example**:
     * Email: `c67a544ff3ec2feafb9b22…`
     * Phone: `f0e4c2f76c58916ec258f2…`
     * MAID: `cdda802e-fb9c-47ad-0794d394c912`

#### **Segment Name Field**

* **Field Name**: `segment_name`
* **Data Type**: String
* **Description**: The name of the audience being created or updated in Criteo Retail Media.
* **Example**: `Holiday_Shoppers_Q4`

#### **Segment ID Field**

* **Field Name**: `segment_id`
* **Data Type**: String
* **Description**: Unique identifier for the audience segment assigned by Criteo.
* **Example**: `123456789`

#### **Action Field**

* **Field Name**: `action`
* **Data Type**: String
* **Description**: Specifies whether to add or remove users from the audience.
* **Accepted Values**: `add`, `remove`
* **Example**: `add`

***

### **💡 How the Schema Works in MadConnect**

**Audience Creation:**

* If a `segment_name` is provided and `segment_id` is not, MadConnect will create a new audience in Criteo using the provided name.
* The `action` field must be set to `add` during audience creation.

**Managing Existing Audiences:**

* If a `segment_id` exists, IDs will be added or removed based on the `action` field.
* Any valid ID field sent to Criteo that the platform supports will be used for matching. This includes hashed emails, hashed phone numbers, MAIDs, and identifiers such as identityLink or customerid (if applicable).

**UI Enhancements:**

* Users can configure additional metadata (e.g., audience source, lifespan) via dropdowns in the MadConnect UI.
* These settings can be modified at the connection level as needed.

***

For more details on the Criteo Retail Media Audience API, refer to the Criteo API [Documentation](https://developers.criteo.com/retail-media/docs/audience-segment-endpoints).
