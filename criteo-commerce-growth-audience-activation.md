# Criteo Commerce Growth - Audience Activation

## **Criteo Commerce Growth - Audience Connector Overview**

### **Connector Overview**

| Field                    | Value                                                                                                                                                                                                                |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Source / Destination** | Destination                                                                                                                                                                                                          |
| **Connector Type**       | Audience                                                                                                                                                                                                             |
| **Data Type**            | Audience                                                                                                                                                                                                             |
| **Description**          | Sync hashed customer data to Criteo Commerce Growth to enable precise targeting and personalized advertising experiences. This connector supports the creation and updating of contact list-based audience segments. |
| **Supported Actions**    | Create / Add / Remove                                                                                                                                                                                                |

***

### **Prerequisites**

To activate the connector, ensure the following:

1. **Active Criteo Commerce Growth Account**
   * Ensure your organization has access to Criteo Commerce Growth with API permissions enabled for managing audience segments.
2. **Authentication Requirements**
   * **OAuth Authentication**: Authenticate your Criteo Commerce Growth account using OAuth within MadConnect.
3. **Permission Requirements**
   * The authenticating user must have permissions to manage **contact list audience segments** via API.

***

### **Configure Connector**

To configure the **Criteo Commerce Growth - Audience** API connector in MadConnect, follow these steps:

1. **Navigate to the My Platforms Section**
   1. In the **MadConnect UI**, go to **"My Platforms"**.
2. **Add a New Platform**
   1. Click on **"Add Platform"**.
3. **Select Criteo Commerce Growth - Audience**
   1. Choose the **"Criteo Commerce Growth - Audience"** tile and click **"Configure"**.
4. **Go to Configuration**
   1. Navigate to the **"Configuration"** tab.
5. **Sign in with Criteo**
   1. Click the **"Sign in with Criteo"** button.
6. **Authenticate with Criteo**
   1. You will be redirected to **Criteo’s login page**. Sign in using your Criteo account credentials.
7. **Authorize MadConnect**
   1. Grant MadConnect permission to manage audiences in Criteo Commerce Growth.
8. **Verify Configuration**
   1. Ensure the connector status is marked as **"Configured"** under **My Platforms**.

***

### **Audience Schema Requirements for Criteo Commerce Growth - Audience Connector**

To successfully send audience data to Criteo Commerce Growth via MadConnect, ensure your data follows the required schema:

#### **ID Field**

* **Field Name**: `email`, `madid`, `identityLink`, `gum`, `customerid`, `phonenumber`
* **Data Type**: String (Hashed or plain depending on type)
* **Description**: Criteo supports the following identifiers:
  * `email`: plain, MD5, SHA256, or SHA256MD5 hashed email addresses
  * `madid`: Mobile Ad ID (IDFA or GAID)
  * `identityLink`: LiveRamp Identity Link
  * `gum`: Criteo GUM cookie ID
  * `customerid`: Criteo customer-defined ID
  * `phonenumber`: plain or SHA256 hashed, supported only for advertisers in India

#### **Segment Name Field**

* **Field Name**: `segment_name`
* **Data Type**: String
* **Description**: The name of the segment being created or updated.
* **Example**: `Summer_Promo_Buyers`

#### **Segment ID Field**

* **Field Name**: `segment_id`
* **Data Type**: String
* **Description**: Unique identifier for the audience segment assigned by Criteo.
* **Example**: `abcdef123456`

#### **Action Field**

* **Field Name**: `action`
* **Data Type**: String
* **Description**: Specifies whether to add or remove users from the segment.
* **Accepted Values**: `add`, `remove`
* **Example**: `add`

***

### **💡 How the Schema Works in MadConnect**

**Audience Creation:**

* If a `segment_name` is provided and `segment_id` is not, MadConnect will create a new audience in Criteo using the provided name.
* The `action` field must be set to `add` during audience creation.

**Managing Existing Audiences:**

* If a `segment_id` exists, IDs will be added or removed based on the `action` field.
* Any valid ID field sent to Criteo that the platform supports will be used for matching.

**UI Enhancements:**

* Users can configure additional metadata (e.g., audience source, lifespan) via dropdowns in the MadConnect UI.
* These settings can be modified at the connection level as needed.

***

For more details on the Criteo Commerce Growth API, refer to the [Criteo Audience Segments documentation](https://developers.criteo.com/marketing-solutions/docs/audience-segments#manage-contact-lists).
