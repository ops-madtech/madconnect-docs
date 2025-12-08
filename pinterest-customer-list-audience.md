# Pinterest - Customer List Audience

## Pinterest Ads – Customer List Audiences Overview

MadConnect integrates with Pinterest’s Customer List Audiences API to allow advertisers to activate first-party audiences for targeting and suppression. The connector supports hashed emails, mobile device identifiers (MAIDs), IDFAs, and custom external IDs, and automatically normalizes, hashes, and structures payloads to meet Pinterest’s requirements.

***

### &#x20;Connector Overview

| Field                 | Description                                                                                                                            |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **Connector Type**    | Destination                                                                                                                            |
| **Data Type**         | Customer List Audiences                                                                                                                |
| **Description**       | Upload customer data (emails, device IDs, external IDs) to create Customer List Audiences in Pinterest Ads for targeting or exclusion. |
| **Supported Actions** | Create / Add / Remove                                                                                                                  |

***

### **How Pinterest Audiences Work**

Pinterest’s audience model has **two linked objects**:

| Object                      | Description                                                 | Created/Managed By                          |
| --------------------------- | ----------------------------------------------------------- | ------------------------------------------- |
| **Customer List (Segment)** | The list containing user identifiers (emails, MAIDs, IDFAs) | Created/Updated by MadConnect               |
| **Audience**                | The Pinterest targeting entity used in Ads Manager          | Create and linked to a single Customer List |

1. **Customer List** (also referred to as _segment_)
2. **Audience** (the targetable object inside Pinterest Ads Manager)

**Key Rules**

1. Every Audience must reference **exactly one** Customer List.
2. A Customer List may be updated repeatedly, but an Audience can only be linked to **one** list.
3. MadConnect automatically creates, updates, and links these components.

***

### Prerequisites

Before configuring the Pinterest connector in MadConnect, ensure the following:

**Pinterest Access & Permissions**

1. You have access to a **Pinterest Business Account**.
2. You have permission to **manage Audiences and Customer Lists**.
3. The account must be active (not under review) and have the **Audiences** feature enabled.

**Advertiser (Ad Account) ID**

1. Required during connection setup.
2. Found in Pinterest Ads Manager (URL or account selector).

**Authentication Method**

You must authenticate using either:

1. **Pinterest OAuth (recommended)**, or
2. **Manual API token entry** (Client ID, Client Secret, Access Token, Refresh Token)

**Audience File or Table**

1. Must follow the **MadConnect Standard Audience Schema** (schema tables below).
2. Must contain **exactly one Customer List** (`segment_name`, `segment_id`).

***

### **Authentication Options in MadConnect**

MadConnect supports two authentication modes for Pinterest:

#### **Option 1: OAuth Authentication (Recommended)**

**Steps**

1. Go to **My Platforms → Pinterest → Configuration**
2. Click **Connect to Pinterest Account**
3. Log in using your Pinterest Business credentials
4. Approve all requested permissions
5. The platform will show **Configured** under **Status** within **My Platforms** page once authenticated

**One-Time OAuth Link (Copy Icon)**

The **copy** icon generates a **single-use OAuth URL**.

Use cases:

1. Allow a teammate (e.g., business admin) to authenticate
2. Allow authentication outside the MadConnect UI

**Important:**

1. The link expires after first successful authorization
2. After it is used, generate a new link if needed again by clicking the icon.

***

#### **Option 2: Manual Token Entry (API Token Authentication)**

Select **Allow me to enter the tokens manually** to authenticate without OAuth.

**Required Fields**

| Field             | Required | Description                            |
| ----------------- | -------- | -------------------------------------- |
| **Client ID**     | Yes      | The Pinterest App ID                   |
| **Client Secret** | Yes      | The Pinterest App Secret               |
| **Access Token**  | Yes      | Token used for authenticated API calls |
| **Refresh Token** | Yes      | Used to rotate the access token        |

**All four fields** are required and align with Pinterest’s OAuth2 token model.

***

### Required UI Configuration (Connection Setup)

When creating a Pinterest connection in MadConnect, the UI requires the following:

| Field             | Description                       | Required for First Transfer?           | Required for Subsequent Transfers?                                              |
| ----------------- | --------------------------------- | -------------------------------------- | ------------------------------------------------------------------------------- |
| **Ad Account ID** | Pinterest advertiser account      | Yes                                    | Yes                                                                             |
| **Audience Name** | Name for the Pinterest Audience   | Yes                                    | Yes                                                                             |
| **Audience ID**   | The Pinterest Audience identifier | **No** – leave blank for new audiences | Yes once audience is created (Retrieve **Audience ID** from **Reports → info)** |

### How New vs Existing Audiences Are Handled

#### **Creating a New Audience**

1. Enter **Ad Account ID** and **Audience Name** (leave Audience ID blank)
2. Run the first transfer
3. MadConnect will:
   * Create a **new Customer List**
   * Create a **new Audience** linked to that list
4. Retrieve **Audience ID** from **Reports → info**
5. Update the connection with **Audience ID** to ensure all future transfers update the same Audience

#### **Updating an Existing Audience**

1. Enter **Ad Account ID**, **Audience Name**, and **Audience ID**
2. MadConnect updates the existing Customer List and Audience mapping

***

### **Input Data Requirements**

Pinterest requires a **single Customer List** per audience upload.

**Required Customer List Fields**

| Field             | Required             | Purpose                                  |
| ----------------- | -------------------- | ---------------------------------------- |
| **segment\_name** | Yes                  | The name of the Customer List            |
| **segment\_id**   | Required for updates | Used to update an existing Customer List |

**Rules**

1. **Only `segment_name` present** → MadConnect creates a new Customer List
2. **`segment_id` present** → MadConnect updates an existing Customer List
3. **Multiple segment\_ids or segment\_names in the same dataset are not allowed**

**Behavior During Processing**

MadConnect will:

1. Add or remove users from the Customer List
2. Link the Customer List to the specified Audience
3. Return both `segment_id` and `audience_id` in the **info** object on the Reports page

***

### Supported Identifier Fields

Pinterest accepts the following identifiers:

| Identifier Type | Raw Field | Pre-Hashed Field |
| --------------- | --------- | ---------------- |
| **Email**       | `email`   | `email_sha256`   |
| **IDFA**        | `idfa`    | `idfa_sha256`    |
| **MAID**        | `maid`    | `maid_sha256`    |

**Hashing Rules**

* Raw identifiers → MadConnect normalizes + hashes to **SHA-256** automatically
* Pre-hashed values must be:
  * SHA-256
  * Normalized before hashing (emails lowercased + trimmed)

Pinterest requires **100 matched users** before an Audience becomes targetable.

***

### Sample Audience Schema Table

Below is a recommended subset of fields for Pinterest activation:

**Identifier & Segment Fields**

| Field Name               | Type   | Required                            | Notes                                                                |
| ------------------------ | ------ | ----------------------------------- | -------------------------------------------------------------------- |
| `segment_name`           | String | Yes                                 | Name of Pinterest Customer List                                      |
| `segment_id`             | String | Optional (but required for updates) | Returned after first run                                             |
| `email` / `email_sha256` | String | At least one ID field required      | At least one ID field required                                       |
| `idfa` / `idfa_sha256`   | String | At least one ID field required      | Optional                                                             |
| `maid` / `maid_sha256`   | String | At least one ID field required      | Optional                                                             |
| `action`                 | String | Yes                                 | Supports “add” or “remove”. Use "add" when creating a new audience.  |

MadConnect will ignore unsupported fields automatically.

***

### Running Transfers in MadConnect

Once authenticated and configured:

1. Navigate to **Connections → Create Connection**
2. Select your source and destination (Pinterest)
3. Enter Ad Account ID + Audience Name (+ Audience ID if updating)
4. Run a **Manual Transfer** or configure a **Scheduled Transfer**

MadConnect will:

1. Validate and normalize data
2. Hash identifiers
3. Construct Pinterest-compliant payloads
4. Create/update Customer Lists and associated Audience
5. Handle API retries and rate limits
6. Return processing details in the Reports tab

***

#### Important Notes

* Pinterest may apply internal moderation before making an Audience active
* UI may not reflect changes immediately
* Accounts under review or restricted cannot accept uploads
* MadConnect automatically handles hashing, normalization, and payload structuring to maximize match quality

***

#### Additional Resources

* [Pinterest API – Customer Lists (Developers)](https://developers.pinterest.com/docs/api/v5/customer_lists-create/)
* [Pinterest Audience Targeting Help Center](https://help.pinterest.com/en/business/article/audience-targeting)

