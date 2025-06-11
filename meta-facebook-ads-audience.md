# META / Facebook Ads - Audience

![](<.gitbook/assets/image (16).png>)

#### Meta Ads -Custom  Audiences Overview

MadConnect integrates seamlessly with Meta’s Custom Audiences API, enabling advertisers to effectively manage and target their audiences on Meta platforms, including Facebook and Instagram. With this connector, businesses can sync customer data directly from their data sources to Meta, ensuring more precise and impactful audience targeting.

***

### **Connector Overview**

| Field                 | Description                                                                                                                                |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **Connector Type**    | Destination                                                                                                                                |
| **Data Type**         | Custom Audiences                                                                                                                           |
| **Description**       | Custom audiences allow advertisers to target their ads to a specific set of people with whom they have already established a relationship. |
| **Supported Actions** | Create / Add / Remove                                                                                                                      |

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

1.  **ID Field(s) for Matching**



    | **ID Type**  | **Field Name**        | **Hashed** | **Formatting Guidelines**                                                   | **Example**                        |
    | ------------ | --------------------- | ---------- | --------------------------------------------------------------------------- | ---------------------------------- |
    | Email        | `email_sha256`        | Yes        | Trim whitespace, lowercase before hashing                                   | `5d41402abc4b2a76b9719d911017c592` |
    | Phone        | `phone_sha256`        | Yes        | Remove symbols/letters, prefix with country code if `country_code` not used | `98f6bcd4621d373cade4e832627b4f6`  |
    | First Name   | `fname_sha256`        | Yes        | Lowercase, remove punctuation, convert to Roman characters                  | `6dcd4ce23d88e2ee9568ba546c007c63` |
    | Last Name    | `lname_sha256`        | Yes        | Same as First Name                                                          | `7c6a180b36896a0a8c02787eeafb0e4c` |
    | Zip Code     | `postal_code_sha256`  | Yes        | US: First 5 digits only. UK: Area/District/Sector format                    | –                                  |
    | Country Code | `country_code_sha256` | Yes        | ISO 3166-1 alpha-2 format (e.g., `us`), lowercase before hashing            | `us` → hashed                      |
    | Birth Month  | `dobm_sha256`         | Yes        | Format MM (01–12), hash the result                                          | `01` → hashed                      |
    | Birth Day    | `dobd_sha256`         | Yes        | Format DD (01–31), hash the result                                          | `15` → hashed                      |
    | Birth Year   | `doby_sha256`         | Yes        | Format YYYY (1900–current), hash the result                                 | `1990` → hashed                    |
    | Mobile Ad ID | `maid`                | No         | Not hashed; standard device ID                                              | `cdda802e-fb9c-47ad-0794d394c912`  |

💡 _For a complete list of supported match identifiers and multi-key matching guidelines, review the official_ [_Meta Custom Audiences Documentation._](https://developers.facebook.com/docs/marketing-api/audiences/guides/custom-audiences/#external_identifiers)

2. **Segment ID Field**
   * **Field Name:** `segment_id`
   * **Data Type:** String
   * **Description:** The unique ID assigned by Meta for the specific audience segment.
   * **Example:** `987654321`
3. **Segment Name Field**
   * **Field Name:** `segment_name`
   * **Data Type:** String
   * **Description:** The name of the audience to be created in Meta. If a `segment_id` is not provided, MadConnect will use the `segment_name` to create a new audience in Meta and return the assigned `segment_id`.
   * **Example:** `Winter Campaign Audience`
4. **Action Field**
   * **Field Name:** `action`
   * **Data Type:** String
   * **Description:** Specifies whether to **add** or **remove** the user from the audience.
   * **Accepted Values:** `add`, `remove`
   * **Example:** `add`

***

#### **💡 How the Schema Works in MadConnect:**

1. **Audience Creation:**
   * If a **`segment_name`** is provided and **`segment_id`** is not, MadConnect will **create a new audience** in Meta using the provided name.
   * The **`action`** field must be set to `add` during audience creation.
2. **Managing Existing Audiences:**
   * If a **`segment_id`** exists, IDs will be **added or removed** based on the **`action`** field.
   * **Any valid ID field sent to Meta that the platform supports will be used for matching.** This allows flexibility for matching on a wide range of identifiers, from hashed emails and phone numbers to non-hashed Mobile Advertiser IDs (MADID).
3. **Hashing Guidelines:**
   * **SHA-256 hashing is required** for all personal identifiers except for **External Identifiers**, **App User IDs**, **Page Scoped User IDs**, and **MAID**.
   * Normalize data before hashing (e.g., lowercase, remove whitespace/special characters).
   * Names (first/last) support special characters and non-Roman alphabets but should include Romanized versions for better match rates.
4. **UI Enhancements:**
   * Users can configure metadata (e.g., **audience source**, **duration**) using dropdowns in the **MadConnect UI**.
   * These settings can be modified at the **connection level** as needed.



For more details on Meta Custom Audiences, please refer to the[ Meta Custom Audiences documentation](https://www.facebook.com/business/help/744354708981227).
