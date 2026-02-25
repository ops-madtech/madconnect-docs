# META / Facebook Ads - Audience

![](<.gitbook/assets/image (16).png>)

## Meta Ads -Custom  Audiences Overview

MadConnect integrates seamlessly with Meta’s Custom Audiences API, enabling advertisers to effectively manage and target their audiences on Meta platforms, including Facebook and Instagram. With this connector, businesses can sync customer data directly from their data sources to Meta, ensuring more precise and impactful audience targeting.

***

### **Connector Overview**

| Field                 | Description                                                                                                                                |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **Connector Type**    | Destination                                                                                                                                |
| **Data Type**         | Custom Audiences                                                                                                                           |
| **Description**       | Custom audiences allow advertisers to target their ads to a specific set of people with whom they have already established a relationship. |
| **Supported Actions** | Create / Add / Remove / Replace / Delete                                                                                                   |

***

### Prerequisites

Before configuring Meta in MadConnect, ensure the following:

#### **Meta Access Requirements**

1. Active **Meta Ads Account**
2. Permission to manage **Custom Audiences** (Admin or Advertiser-level access)
3. Business Manager access aligned with the ad account
4. No policy restrictions on the account (audience creation disabled for compliance reasons)
5. To use Custom Audiences, the Business’ users must first sign Meta's Terms Of Service contracts. Verify status using the below link.
   * [https://business.facebook.com/ads/manage/customaudiences/tos/?act=**\<AD\_ACCOUNT\_ID>**. ](https://business.facebook.com/ads/manage/customaudiences/tos/?act=%3CAD_ACCOUNT_ID%3E.)

***

#### **Authentication Requirements**

You must authenticate your Meta business using **OAuth** or **API token entry** (depending on your setup).

**Option 1: OAuth Authentication (Recommended)**

OAuth provides the most secure and stable authentication.

**Steps**

1. Go to **My Platforms → Meta – Custom Audiences → Configure**
2. Click **Sign in with Meta**
3. Log in with your Meta Business credentials
4. Approve all requested permissions
5. Once successful, the platform will appear as **Configured** in My Platforms

**One-Time OAuth Link (Copy Icon)**

A copy icon generates a **single-use OAuth URL**, which can be shared with a teammate who must complete authentication.

**Use cases:**

1. Business admin must authenticate under a shared business account
2. Authentication cannot be completed by the immediate user

**Notes:**

1. The link expires after one successful authentication
2. A new link must be generated for additional authentication attempts

***

**Option 2: Token-Based Authentication (API Token Entry)**

If you prefer not to use OAuth, you may manually enter your Meta access tokens.

Required fields:

1. **Access Token**

These must be a valid token aligned to a Meta app with Custom Audience permissions.

***

### Connection Configuration (UI Fields)

When creating a Meta Custom Audience connection in MadConnect, the destination configuration requires the following fields:

**Meta Destination Fields**

| Field               | Required | Description                                                                                    |
| ------------------- | -------- | ---------------------------------------------------------------------------------------------- |
| **Advertiser ID**   | Yes      | Meta Ad Account ID (e.g. 1234567890) associated with the Custom Audience                       |
| **Audience Source** | Optional | Dropdown specifying audience origin (e.g., CRM, Website, App); affects metadata stored in Meta |

These fields determine how MadConnect structures calls to Meta's Custom Audiences API.

***

### Audience Creation & Management Logic in MadConnect

Meta uses the following fields to manage audiences:

**Segment Fields**

| Field             | Required                | Description                                |
| ----------------- | ----------------------- | ------------------------------------------ |
| **segment\_name** | Yes (for new audiences) | Name of the audience to be created in Meta |
| **segment\_id**   | Required for updates    | Meta-assigned Custom Audience ID           |

**Action Field**

| Field      | Values                       | Purpose                                                                              |
| ---------- | ---------------------------- | ------------------------------------------------------------------------------------ |
| **action** | add, remove, delete, replace | Controls whether a user is added or removed. Use 'add' when creating a new audience. |

#### **Creating a New Audience**

1. Provide **segment\_name** but **omit segment\_id**
2. First transfer will:
   * Create a new Custom Audience in Meta using segment\_name
   * Return the **segment\_id** in Reports → info
3. You then update the connection data with the new segment\_id for all future runs
4. Action must be **add** during initial creation

#### **Updating an Existing Audience**

1. Provide **segment\_id**
2. Use action:
   * `add` → incremental additions
   * `remove` → incremental removals
   * `replace` → full audience overwrite
   * `delete` → permantely deletes audience
3. Meta will match multiple identifier types automatically

***

### Matching Schema (Meta Custom Audiences)

Meta supports a wide range of identifiers. MadConnect allows all supported ID fields to be included in a single dataset.

**Supported ID Fields**

| ID Type           | Field Name                       | Hashed?  | Notes                                    |
| ----------------- | -------------------------------- | -------- | ---------------------------------------- |
| Email             | `email_sha256`                   | Yes      | Lowercase + trim before hashing          |
| Phone             | `phone_sha256`                   | Yes      | E.164 normalization before hashing       |
| First Name        | `fname_sha256`                   | Yes      | Romanize when applicable                 |
| Last Name         | `lname_sha256`                   | Yes      | Same normalization as fname              |
| ZIP / Postal Code | `postal_code_sha256`             | Yes      | 5-digit (US), formatted (UK)             |
| Country Code      | `country_code_sha256`            | Yes      | ISO-3166-1 alpha-2                       |
| DOB Month         | `dobm_sha256`                    | Yes      | MM format                                |
| DOB Day           | `dobd_sha256`                    | Yes      | DD format                                |
| DOB Year          | `doby_sha256`                    | Yes      | YYYY format                              |
| MAID              | `maid`                           | No       | Sent in plain form — not hashed          |
| External IDs      | `extern_id` / `extern_id_sha256` | Optional | Either hashed or raw (Meta accepts both) |

**Hashing Requirements**

1. All PII must use **SHA-256**
2. Pre-hashed fields must already be:
   * Lowercased
   * Trimmed
   * Normalized according to Meta’s rules

Meta's matching engine will unify all compatible identifiers into a single matched record.

**Multi-Key Matching Behavior**

Meta supports "best available" matching. MadConnect takes advantage of this by:

1. Sending all available IDs per row (email + phone + MAID + name + address)
2. Allowing Meta to match on any available identifiers
3. Improving audience scale without increasing operational overhead

Any field you include that Meta supports will be used.

***

### Running Transfers in MadConnect

Once platform configuration and connection setup are complete:

1. Select your **source** (Snowflake, S3, etc.)
2. Select **Meta – Custom Audiences** as the destination
3. Provide:
   * Advertiser ID
   * Audience Source (optional)
   * IDs field (optional)
   * Replace Existing Audience flag
4. Run a **Manual Transfer** or configure a **Scheduled Transfer**

MadConnect will:

1. Normalize and hash identifiers
2. Validate Meta schema compliance
3. Create or update Custom Audiences
4. Manage add/remove membership logic
5. Handle rate limits, retries, and batching
6. Return metadata (segment\_id, counts, errors) in **Reports**

***

### **Important Notes**

1. Meta may delay audience activation while internal processing completes
2. Audience sizes under Meta’s reporting threshold will show as “<1,000 users”
3. Accounts with policy violations cannot create/update audiences
4. Using multiple identifiers increases match rate
5. Replace Audience = full replacement (Meta overwrites audience membership)

***

### **Additional Resources**

1. [Meta Custom Audiences Developer Documentation](https://www.facebook.com/business/help/744354708981227)
2. [Meta Ads Manager – Custom Audience Overview](https://www.facebook.com/business/help/744354708981227?id=2469097953376494)
3. [Meta Business Access & Permissions Guide](https://www.facebook.com/business/help/442345745885606)
