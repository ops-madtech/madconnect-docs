# Amazon Ads - DSP Audiences

![](<.gitbook/assets/image (30).png>)

## Amazon Ads – DSP Audiences

### **Amazon Ads - DSP Audiences Connector Overview**

#### **Connector Overview**

| Field                       | Value                                                                                                                                                                                    |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Source / Destination**    | Destination                                                                                                                                                                              |
| **Data Type**               | Audience                                                                                                                                                                                 |
| **Description**             | Sync first-party audience data to Amazon DSP for activation in programmatic campaigns. This connector supports audience creation and membership updates for targeting within Amazon DSP. |
| **Supported Actions**       | Create / Add / Remove                                                                                                                                                                    |
| **Amazon Activation Input** | Use **advertiserId** when activating audiences directly in DSP                                                                                                                           |

***

#### **Prerequisites**

Ensure the following before configuring:

1. **Active Amazon DSP Account**\
   Permissions to create and manage audiences
2. **Authentication**
   * OAuth via Amazon Ads (recommended)
   * OR Client ID / Client Secret from Amazon Ads Developer account
3. **DSP Advertiser ID**\
   Required for audience activation
   * Found in the **Amazon DSP UI → Campaign Manager → Advertiser**
   * Typically visible in the **URL or advertiser settings**

***

#### **Authentication Requirements**

MadConnect supports two methods for authenticating with Amazon Ads:

**Option 1: OAuth (Recommended)**

The easiest and most secure method.

1. Click **"Sign in with Amazon"**
2. Log in using your Amazon Ads credentials
3. Grant permissions to MadConnect
4. Connector will automatically store and manage tokens

***

**Option 2: Manual OAuth (Advanced)**

For users who prefer to manage credentials directly or are working in restricted environments.

Enable **“Allow me to enter the tokens manually”**, then provide:

| Field             | Description                               |
| ----------------- | ----------------------------------------- |
| **Client ID**     | Your Amazon Ads application client ID     |
| **Client Secret** | Your Amazon Ads application client secret |
| **Access Token**  | OAuth access token used for API requests  |
| **Refresh Token** | Token used to refresh the access token    |

***

#### **Connection Configuration Fields**

When creating a connection that uses **Amazon DSP –  Audiences** as the destination, the following fields are required:

| Field                | Description                                                                                                                                                             |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Advertiser ID**    | The Amazon DSP advertiser identifier used to route the audience to the correct advertiser account. This is required when activating audiences directly into Amazon DSP. |
| **External User ID** | A unique user identifier defined by the data provider to represent an individual. Amazon uses this field for audience matching.                                         |

**Advertiser ID**

To locate your **Amazon DSP Advertiser ID**:

1. Log in to the **Amazon DSP console**
2. Go to **Campaign Manager**
3. Select the advertiser
4. The ID is typically visible in the **URL** or within **advertiser settings**

**External User ID**

MadConnect supports two behaviors for this field:

* If an **external\_user\_id** value is available in your source data, it can be mapped directly
* If this field is not available, MadConnect will automatically generate and encrypt/hash a value using the provided **email** or **phone** field

> This allows users to continue activating audiences even when their source data does not include a native external user identifier.

***

#### **Audience Schema Requirements**

| Field                                                      | Required              | Description                                                                                                        |
| ---------------------------------------------------------- | --------------------- | ------------------------------------------------------------------------------------------------------------------ |
| `segment_name`                                             | For create            | Name of the audience being created                                                                                 |
| `segment_id`                                               | For updates           | Existing Amazon audience identifier                                                                                |
| `action`                                                   | Yes                   | Supported values: `add`, `remove`                                                                                  |
| `email_sha256` / `phone_sha256` / other hashed identifiers | Required for matching | User identifiers used for Amazon matching                                                                          |
| `external_user_id`                                         | Preferred             | Unique identifier for the individual user; can be supplied directly or generated by MadConnect from email or phone |

#### **Important Notes**

* **external\_user\_id is required by Amazon**, but MadConnect can generate it if missing
* **All PII must be SHA-256 hashed** before ingestion
* **Minimum audience size**: \~2,000 matched users
* **Processing time**:
  * \~15 minutes for ingestion
  * Up to 36 hours to appear in DSP

***

#### **Additional Resources**

* [Amazon DSP Audience Management API](https://advertising.amazon.com/API/docs/en-us/guides/amazon-marketing-cloud/audiences/audience-management-service)
