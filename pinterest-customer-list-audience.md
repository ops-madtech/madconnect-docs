# Pinterest - Customer List Audience

## Pinterest Ads – Customer List Audiences Overview

MadConnect supports integration with Pinterest’s Customer List Audiences API, enabling advertisers to securely activate first-party audiences for targeting or exclusion on Pinterest. This includes support for hashed emails, mobile device identifiers (MAIDs), and custom external IDs — all processed and validated in accordance with Pinterest's requirements.

***

### &#x20;Connector Overview

| Field                 | Description                                                                                                              |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| **Connector Type**    | Destination                                                                                                              |
| **Data Type**         | Customer List Audiences                                                                                                  |
| **Description**       | Upload customer data (emails, device IDs, external IDs) to create audiences in Pinterest Ads for targeting or exclusion. |
| **Supported Actions** | Create / Add / Remove                                                                                                    |

***

#### Pinterest Audience Details

1. A **Pinterest Customer List Audience** must contain at least **100 matched users** before it can be used for targeting.
2. Matching occurs after Pinterest completes its internal hash reconciliation process.

***

### Prerequisites

Before configuring the Pinterest connector in MadConnect, ensure the following:

1. **Pinterest Ads Account Access**
   * You must have access to a valid Pinterest Ad Account with permission to manage customer lists and audiences.
2. **OAuth Authentication**&#x20;
   * Authenticate using your Pinterest Business login through MadConnect’s OAuth flow. You’ll be prompted during connector setup.
3. **Audience Schema Alignment**
   * Ensure your audience file or table conforms to the **MadConnect Standard Audience Schema**. Key fields below.
4. **Advertiser ID**
   * You will need the **Pinterest Advertiser ID** associated with your account.
   * You can find this by logging in to Pinterest Ads Manager
   * The ID is typically shown in the URL or account selector dropdown.
5. **Active Pinterest Business Account**
   * Account must be active, not under review, and have the “Audiences” feature enabled.

***

### Audience Schema Requirements for Pinterest

MadConnect’s **Standard Audience Schema** supports multiple identifier fields (e.g., `email`, `phone`, `maid`, `idfa`, etc.) in a single dataset.\
Each platform’s API has its own rules for how many identifiers can be included per audience upload:

* If a platform **supports multiple ID types** (like Pinterest), you may include several ID columns in your file or table. MadConnect will automatically structure and format the payload according to that platform’s specifications.
* If a platform **accepts only one ID type per upload** (like some custom or legacy APIs), MadConnect will process only the supported identifier for that destination.

MadConnect handles all normalization, hashing, and payload transformation internally to meet each platform’s requirements, ensuring schema consistency regardless of input structure.

| ID Type         | Field Name                | Hashed                            | Formatting Guidelines                                                                    |
| --------------- | ------------------------- | --------------------------------- | ---------------------------------------------------------------------------------------- |
| **Email**       | `email` or `email_sha256` | Optional (SHA-256, SHA-1, or MD5) | Lowercase, trimmed. MadConnect will normalize and hash automatically if un-hashed.       |
| **MAID / IDFA** | `maid` or `idfa_sha256`   | Required (SHA-256, SHA-1, or MD5) | Must be hashed before upload. If provided raw, MadConnect will hash prior to submission. |
| **External ID** | `extern_id_sha256`        | Required (SHA-256, SHA-1, or MD5) | Hashed CRM, loyalty, or platform ID.                                                     |

> **Pinterest API Requirement:**\
> Emails can be plain text or hashed (SHA-1, SHA-256, MD5).\
> MAIDs and IDFAs **must** be hashed using one of these algorithms.

***

**Segment Fields**

| Field Name     | Description                                                                            |
| -------------- | -------------------------------------------------------------------------------------- |
| `segment_name` | Name of the audience to create. Required if `segment_id` is not provided.              |
| `segment_id`   | Pinterest-assigned ID for an existing audience. Required when updating existing lists. |

***

**Action Field**

| Field Name | Accepted Values | Description                                                                                                |
| ---------- | --------------- | ---------------------------------------------------------------------------------------------------------- |
| `action`   | add, remove     | Specifies whether to add or remove users from the audience. Value should be add when creating an audience. |

***

#### Getting Started in MadConnect

1. **Add Pinterest as a Destination**
   * Go to **My Platforms → Add Platform**
   * Select **Pinterest Customer List – Audience Activation** and click **Configure**
2. **Configure Connector**
   * Click **Sign in with Pinterest**
   * Log in with your Pinterest Business credentials and grant access
     * Optionally, enter in API token information.&#x20;
3. **Configure Connection**
   * Once the connector is configured, confirm the platform shows as **Configured** under _My Platforms_
   * Navigate to Create Connection
   * Enter your **Ad Account ID**
4. **Schedule or Transfer**
   * Choose **Manual Transfer** for one-time loads or **Scheduled Transfer** for recurring updates
   * MadConnect will validate, normalize, hash, and securely upload compliant records to Pinterest’s API

***

#### How Audience Schema Works in MadConnect

1. If only `segment_name` is provided, MadConnect will **create a new audience** in Pinterest.
2. If `segment_id` is provided, MadConnect will **update an existing audience** using the `add` or `remove` action.

***

#### MadConnect Hashing & Normalization Policy

MadConnect automatically performs **data normalization and hashing** according to each platform’s API requirements.

1. Raw identifiers are normalized (lowercased, trimmed, formatted) and hashed before upload.
2. If you choose to **pre-hash data**, you are responsible for ensuring it meets the platform's accepted algorithms (SHA-1, SHA-256, MD5) and normalization rules.
3. Allowing MadConnect to handle hashing is recommended for consistent match quality and compliance.

***

#### Important Notes

* Pinterest may delay audience activation while internal moderation completes.
* MadConnect handles rate limits that apply to audience creation and update endpoints.
* Ensure your account remains eligible for audience targeting (no policy restrictions).

***

#### Additional Resources

* [Pinterest API – Customer Lists (Developers)](https://developers.pinterest.com/docs/api/v5/customer_lists-create/)
* [Pinterest Audience Targeting Help Center](https://help.pinterest.com/en/business/article/audience-targeting)

