# Bing Ads - Audiences

### Microsoft Advertising – Customer Match Overview

MadConnect integrates with Microsoft Advertising’s Customer Match API, enabling advertisers to upload and manage first-party audiences for activation across Microsoft Search and Audience Network inventory.

This connector allows businesses to sync hashed customer data directly into Microsoft Advertising for precise targeting.

***

#### **Connector Overview**

| Field                 | Description                                                                    |
| --------------------- | ------------------------------------------------------------------------------ |
| **Connector Type**    | Destination                                                                    |
| **Data Type**         | Customer Match Audiences                                                       |
| **Description**       | Customer Match allows advertisers to target ads using their own customer data. |
| **Supported Actions** | Create / Add / Remove / Replace                                                |

***

#### Prerequisites

Before configuring Microsoft Advertising in MadConnect, ensure the following:

**Microsoft Advertising Access Requirements**

1. Active Microsoft Advertising account
2. Access to the correct **Customer ID** and **Account ID**
3. Audience management permissions within the account
4. You must first create one customer list audience and accept the terms and conditions in the Microsoft Advertising UI.&#x20;
   1. The initial customer list doesn't need to contain any customer data, but you must select **"I ACCEPT."**
5. No policy or billing restrictions on the account

***

**Authentication Requirements**

You must authenticate using **OAuth** or **Manual Token Entry**.

***

#### **Option 1: OAuth Authentication (Recommended)**

OAuth provides the most secure and stable authentication.

**Steps**

1. Go to **My Connectors → Bing Ads (Audiences) → Configure**
2. Click **Sign in with Microsoft**
3. Log in with your Microsoft Advertising credentials
4. Approve requested permissions
5. Platform will show as **Configured**

**One-Time OAuth Link (Copy Icon)**

The copy icon generates a **single-use OAuth URL** that can be shared with a teammate.

**Use cases:**

1. Business admin must authenticate
2. Authentication must occur under a shared login

**Notes:**

1. The link expires after one successful authentication
2. A new link must be generated for additional attempts

***

#### **Option 2: Manual Token Entry**

Enable **“Allow me to enter the tokens manually”** and provide:

Required fields:

1. **Client ID**
2. **Client Secret**
3. **Access Token**
4. **Refresh Token**
5. **Developer Token**

These must be tied to a valid Microsoft Advertising application with Customer Match permissions.

***

#### Connection Configuration (UI Fields)

When creating a Microsoft Advertising Customer Match connection, the destination configuration requires:

| Field           | Required | Description                                      |
| --------------- | -------- | ------------------------------------------------ |
| **Customer ID** | Yes      | Parent Microsoft Advertising entity              |
| **Account ID**  | Yes      | Specific ad account where the audience will live |

Both values are required for API calls.

***

#### Where to Find Customer ID and Account ID

When logged into Microsoft Advertising, these values are visible in the browser URL.

Example:

```
https://ui.ads.microsoft.com/campaign/vnext/overview?uid=582910244&cid=731904582&aid=496210377
```

In this example:

* `cid=731904582` → **Customer ID**
* `aid=496210377` → **Account ID**

**Important:**

1. Enter only the numeric value (no “cid=” or “aid=”)
2. Customer ID and Account ID must correspond to the same hierarchy

***

#### Audience Creation & Management Logic in MadConnect

Microsoft Advertising manages Customer Match audiences using similar segment logic.

**Segment Fields**

| Field             | Required                | Description                        |
| ----------------- | ----------------------- | ---------------------------------- |
| **segment\_name** | Yes (for new audiences) | Name of the audience to be created |
| **segment\_id**   | Required for updates    | Microsoft Advertising Audience ID  |

***

**Action Field**

| Field      | Values                       | Purpose                      |
| ---------- | ---------------------------- | ---------------------------- |
| **action** | create, add, remove, replace | Controls membership behavior |

***

**Creating a New Audience**

1. Provide **segment\_name**
2. Omit **segment\_id**
3. First transfer will:
   * Create a new Customer Match list
   * Return the generated **segment\_id** in Reports
4. Action should be **add** during initial creation

***

**Updating an Existing Audience**

1. Provide **segment\_id**
2. Use:
   * `add` → incremental additions
   * `remove` → incremental removals
   * `replace` → full audience overwrite

***

#### Matching Schema (Microsoft Customer Match)

Microsoft primarily supports hashed email identifiers.

**Supported ID Fields**

| ID Type | Field Name     | Hashed? | Notes                                        |
| ------- | -------------- | ------- | -------------------------------------------- |
| Email   | `email_sha256` | Yes     | Must be lowercase and trimmed before hashing |

***

#### **Hashing Requirements**

1. All PII must use **SHA-256**
2. Emails must be:
   * Lowercased
   * Trimmed of whitespace
   * UTF-8 encoded before hashing

Microsoft will reject improperly formatted hashes.

***

#### Running Transfers in MadConnect

Once platform configuration and connection setup are complete:

1. Select your **source**
2. Select **Bing Ads (Audiences)** as destination
3. Provide:
   * Customer ID
   * Account ID
4. Run a **Manual Transfer** or schedule

MadConnect will:

1. Validate schema
2. Send hashed identifiers
3. Create or update Customer Match audiences
4. Handle batching and retries
5. Return segment metadata and counts in **Reports**

***

#### **Important Notes**

1. Audience processing may take several hours before becoming targetable
2. Minimum size thresholds may apply before delivery begins
3. Customer ID and Account ID mismatches will result in API errors
4. Replace fully overwrites existing audience membership
5. Permissions must exist at the Account ID level, not just Customer ID level

***

#### **Additional Resources**

1. [Microsoft Advertising Customer Match Documentation](https://learn.microsoft.com/en-us/advertising/bulk-service/customer-list?view=bingads-13)
