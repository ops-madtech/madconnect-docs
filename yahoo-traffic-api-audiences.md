# Yahoo Traffic API - Audiences

## **Yahoo DSP – Audiences Connector Overview**

MadConnect integrates with the **Yahoo DSP Audiences API** to enable advertisers to activate and maintain first-party audiences using hashed identifiers. This connector allows advertisers to create new Yahoo DSP audiences or continuously add users to existing audiences to support targeting and activation workflows.

MadConnect manages schema validation, hashing enforcement, and API orchestration to ensure compliant delivery to Yahoo DSP.

***

### **Connector Overview**

| Field                 | Description                                                    |
| --------------------- | -------------------------------------------------------------- |
| **Connector Type**    | Destination                                                    |
| **Data Type**         | Audience Activation                                            |
| **Primary Use Case**  | First-party Audience Activation                                |
| **Description**       | Sync hashed email and phone identifiers to Yahoo DSP audiences |
| **Supported Actions** | Create / Add                                                   |

***

### **Prerequisites**

Before configuring the Yahoo DSP Audiences connector, ensure the following:

#### **Yahoo DSP Account Requirements**

1. An active **Yahoo DSP advertiser account**
2. Access to **Audience management** within Yahoo DSP
3. A valid Yahoo DSP user with permission to generate API credentials

#### **DSP API Enablement**

1. The **Yahoo DSP API must be enabled** for your account
2. API access is not enabled by default
3. You must contact your **Yahoo DSP Account Manager or Product Support** to enable API access

If API access is not enabled, authentication will fail and credentials may appear as undefined in MadConnect.

***

### **Authentication Requirements**

Yahoo DSP uses **API credential–based authentication**.

#### **API Credential Authentication**

To authenticate Yahoo DSP in MadConnect:

1. Log in to the **Yahoo DSP UI**
2. Click your username in the top-right corner and select **My Account**
3. Click **Activate** and agree to the API terms of service
4. Securely copy the **Client ID** and **Client Secret**
5. In MadConnect, navigate to **My Platforms → Yahoo DSP – Audiences**
6. Open the **Configuration** tab
7. Enter the Client ID and Client Secret
8. Save and confirm the platform status shows **Configured**

***

### **Connection Configuration (UI Fields)**

Once the connector is configured, create a connection and provide the following fields when Yahoo DSP is selected as the destination.

#### **Yahoo DSP Destination Fields**

| Field               | Required | Description                                 |
| ------------------- | -------- | ------------------------------------------- |
| **Advertiser Name** | No       | Human-readable advertiser name in Yahoo DSP |
| **Advertiser ID**   | Yes      | Yahoo DSP advertiser identifier             |

<figure><img src=".gitbook/assets/Yahoo DSP Advertiser ID Location.png" alt="" width="375"><figcaption></figcaption></figure>

These fields determine which advertiser account the audience will be created or updated under.

***

### **Audience Creation & Management Logic**

Yahoo DSP audiences are managed using the following core fields.

#### **Segment Fields**

| Field          | Required     | Description                        |
| -------------- | ------------ | ---------------------------------- |
| `segment_name` | Yes (Create) | Name of the audience to be created |
| `segment_id`   | Yes (Update) | Existing Yahoo DSP audience ID     |

#### **Action Field**

| Field    | Required | Description                                    |
| -------- | -------- | ---------------------------------------------- |
| `action` | Yes      | Defines the operation. Only `add` is supported |

***

#### **Creating a New Audience**

1. Provide `segment_name` value
2. Omit `segment_id` value
3. Set `action = add` for all records
4. MadConnect will:
   * Create a new Yahoo DSP audience
   * Add users to the audience
   * Return the generated `segment_id` in **Reports → info**

***

#### **Updating an Existing Audience**

1. Provide `segment_id` value
2. Set `action = add` for all records
3. MadConnect will add users to the existing audience

**Note:**\
Removing users (`remove`) is **not supported** by this connector Yahoo Audiences.

***

### **Matching Keys (Yahoo DSP)**

The Yahoo DSP Audiences API supports **hashed identifiers only** for this connector.

#### **Supported Identifier Fields**

| ID Type | Field Name     | Hashed | Notes                  |
| ------- | -------------- | ------ | ---------------------- |
| Email   | `email_sha256` | Yes    | Must be SHA-256 hashed |
| Phone   | `phone_sha256` | Yes    | Must be SHA-256 hashed |

Raw identifiers are **not accepted** by Yahoo DSP.

***

### **MadConnect Standard Audience Schema Example**

Your source file or table should generally follow this structure to ensure seamless integration.

| Field Name        | Data Type | Required?                 | Description                                                    |
| ----------------- | --------- | ------------------------- | -------------------------------------------------------------- |
| **segment\_name** | String    | Yes (Create)              | Name of the audience (e.g., _Holiday Shoppers_).               |
| **segment\_id**   | String    | Yes (Update)              | Audience ID assigned by Yahoo DSP (required only for updates). |
| **action**        | String    | Yes                       | Accepted value: `add`.                                         |
| **email\_sha256** | String    | At least one supported ID | User’s email address, normalized and SHA-256 hashed.           |

**Notes**

* At least one supported identifier is required per row
* `email_sha256` is shown as an example; `phone_sha256` may be used instead
* This schema is shared across **all MadConnect audience connectors**

***

### **Hashing & Normalization Requirements**

1. **Algorithm:** SHA-256 only
2. **Normalization (before hashing):**
   * Lowercase values
   * Trim whitespace
3. Pre-hashed values must already meet Yahoo DSP requirements

Failure to normalize correctly will reduce match rates or cause upload errors.

***

### **Important Notes & Limitations**

1. This connector supports **add-only** behavior
   1. User removal is not currently supported
2. Yahoo DSP may delay audience availability after ingestion
3. API access must be explicitly enabled by Yahoo
4. Only **hashed email and phone identifiers** are supported
5. Additional metadata (e.g., audience source, lifespan) may be configurable via the MadConnect UI

***

### **Resources**

* Yahoo DSP API – [About Audiences](https://developer.yahooinc.com/dsp/api/docs/traffic/audience/about-audience.html)<br>
