# SnapChat- Customer Lists

## **Snapchat – Customer Lists Connector Overview**

MadConnect integrates with the **Snapchat Marketing API** to enable advertisers to activate and manage first-party audiences using Snapchat Customer Lists. This connector supports creating new Snapchat audiences and updating existing audiences by adding or removing members using approved, hashed identifiers.

MadConnect manages schema validation, hashing enforcement, batching, and API orchestration to ensure compliant and reliable audience delivery to Snapchat.

***

### **Connector Overview**

| Field                 | Description                                                                 |
| --------------------- | --------------------------------------------------------------------------- |
| **Connector Type**    | Destination                                                                 |
| **Data Type**         | Audience Activation                                                         |
| **Primary Use Case**  | First-party Audience Activation                                             |
| **Description**       | Create and manage Snapchat Customer List audiences using hashed identifiers |
| **Supported Actions** | Create / Add / Remove                                                       |

***

### **Prerequisites**

Before configuring the Snapchat Audiences connector, ensure the following:

#### **Snapchat Account Requirements**

1. An active **Snapchat Ads account**
2. Permission to manage **Audiences** within Snapchat Ads Manager
3. Ability to authenticate using **OAuth 2.0**

#### **Audience Prerequisites**

1. If updating an existing audience, you must know the **Snapchat Segment ID**
2. Segment IDs can be found in Snapchat Ads Manager under:
   * **Assets → Audiences**
   * The Segment ID appears in the URL after `/audiences/custom-audience/`

***

### **Authentication Requirements**

Snapchat Audiences uses **OAuth authentication**.

#### **OAuth Authentication (Required)**

1. Navigate to **My Platforms → Snapchat – Audiences**
2. Click **Configure**
3. Select **Sign in with Snapchat**
4. Log in using your Snapchat Ads credentials
5. Approve permissions to manage customer lists
6. Upon success, the platform status will show **Configured** in My Platforms

***

### **Connection Configuration (UI Fields)**

Once the connector is configured, create a connection and provide the following fields when Snapchat is selected as the destination.

#### **Snapchat Destination Fields**

| Field                 | Required | Description                                    |
| --------------------- | -------- | ---------------------------------------------- |
| **Advertiser ID**     | Yes      | Snapchat **Ad Account ID**                     |
| **Audience Metadata** | Optional | Additional metadata such as lifespan (in days) |

<figure><img src=".gitbook/assets/Snapchat Ad Account ID Location.png" alt=""><figcaption></figcaption></figure>

Also, located in URL: `/ad-accounts?ref_aid=`**`abc12def-3456-7gh8-i90k-m123n4op5q6r`**

***

### **Audience Creation & Management Logic**

Snapchat audiences are managed using the following core fields.

#### **Segment Fields**

| Field          | Required     | Description                                 |
| -------------- | ------------ | ------------------------------------------- |
| `segment_name` | Yes (Create) | Name of the Snapchat audience to be created |
| `segment_id`   | Yes (Update) | Existing Snapchat audience ID               |

#### **Action Field**

| Field    | Required | Description                                     |
| -------- | -------- | ----------------------------------------------- |
| `action` | Yes      | Controls membership updates (`add` or `remove`) |

***

#### **Creating a New Audience**

1. Provide `segment_name` value(s)
2. Omit `segment_id` value(s)
3. Set `action = add`
4. MadConnect will:
   * Create a new Snapchat Customer List audience
   * Upload identifiers
   * Return the generated `segment_id` in **Reports → info**

***

#### **Updating an Existing Audience**

1. Provide `segment_id` value(s)
2. Set `action` to `add` or `remove`
3. MadConnect will update membership for that audience

***

### **Matching Keys (Snapchat Audiences)**

Snapchat supports **hashed identifiers only** and enforces **one identifier type per request**.

#### **Supported Identifier Fields**

| ID Type | Field Name     | Hashed | Notes                                    |
| ------- | -------------- | ------ | ---------------------------------------- |
| Email   | `email_sha256` | Yes    | SHA-256 hashed email                     |
| Phone   | `phone_sha256` | Yes    | SHA-256 hashed phone (with country code) |
| MAID    | `maid_sha256`  | Yes    | SHA-256 hashed IDFA or GAID              |

**Important Snapchat Constraint**

1. You **cannot mix identifier types** in a single request
2. Each transfer must contain **only one ID type** (email _or_ phone _or_ MAID) Snapchat - Audiences

***

### **MadConnect Standard Audience Schema Example**

Your source file or table should generally follow this structure to ensure seamless integration.

| Field Name        | Data Type | Required?                 | Description                                          |
| ----------------- | --------- | ------------------------- | ---------------------------------------------------- |
| **segment\_name** | String    | Yes (Create)              | Name of the audience (e.g., _Holiday Shoppers_).     |
| **segment\_id**   | String    | Yes (Update)              | Snapchat audience ID (required only when updating).  |
| **action**        | String    | Yes                       | Accepted values: `add` or `remove`.                  |
| **email\_sha256** | String    | At least one supported ID | User’s email address, normalized and SHA-256 hashed. |

**Notes**

1. At least one supported identifier is required per row
2. `email_sha256` is shown as an example; `phone_sha256` or `maid_sha256` may be used instead
3. This schema is shared across **all MadConnect audience connectors**

***

### **Hashing & Normalization Requirements**

1. **Algorithm:** SHA-256
2. **Normalization (before hashing):**
   1. Lowercase values
   2. Trim whitespace
3. **Phone numbers:**
   1. Remove symbols and letters
   2. Include country code
4. Pre-hashed values must already meet [Snapchat’s normalization requirements](https://developers.snap.com/api/marketing-api/Ads-API/audience-creation/customer-lists#normalizing--hashing)

Improper normalization or hashing may cause upload failures or low match rates.

***

### **Important Notes & Limitations**

1. Only **one identifier type per request** is supported
2. Mixing ID types in a single transfer will fail
3. Newly created audiences may take several seconds before accepting member updates
4. API errors may occur if:
   1. Segment ID is incorrect
   2. Upload occurs immediately after audience creation (wait briefly)
   3. Hashing requirements are not met

***

### **Resources**

1. [Snapchat Marketing API](https://developers.snap.com/api/marketing-api/Ads-API/audience-creation/customer-lists) – Audiences
