# META / Facebook Ads - Audience

![](<.gitbook/assets/image (16).png>)

#### Meta Ads -Custom  Audiences Overview

MadConnect integrates seamlessly with Meta’s Custom Audiences API, enabling advertisers to effectively manage and target their audiences on Meta platforms, including Facebook and Instagram. With this connector, businesses can sync customer data directly from their data sources to Meta, ensuring more precise and impactful audience targeting.

***

**Connector Overview**

* **Connector Type**: Destination
* **Data Type**: Custom Audiences
* **Description**: Custom audiences allow advertisers to target their ads to a specific set of people with whom they have already established a relationship.
* **Supported Actions**: Create / Add / Remove

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

**ID Field**

* **Field Name:** `email_hash`, `phone_hash`, `maid`, `fname_hash`, `lnname_hash`, `zip_hash`, `country_code_hash`, `dobm`, `dobd_hash`, `doby_hash`
* **Data Type:** String (Hashed where required)
* **Description:**\
  Contains audience member identifiers used for matching in Meta. The following ID types are supported:
  * **`EMAIL`** – SHA-256 hashed email addresses.
    * _Before hashing:_ Trim whitespace and convert to lowercase.
    * _Example:_ `5d41402abc4b2a76b9719d911017c592`
  * **`PHONE`** – SHA-256 hashed phone numbers.
    * _Before hashing:_ Remove symbols/letters and prefix with country code if not using the `country` field.
    * _Example:_ `98f6bcd4621d373cade4e832627b4f6`
  * **`FN` / `LN`** – SHA-256 hashed first and last names.
    * _Before hashing:_ Convert to lowercase, remove punctuation, and use Roman characters where possible.
    * _Example:_
      * First Name (`FN`): `6dcd4ce23d88e2ee9568ba546c007c63`
      * Last Name (`LN`): `7c6a180b36896a0a8c02787eeafb0e4c`
  * **`ZIP`** – SHA-256 hashed zip code.
    * _For US:_ First 5 digits only.
    * _For UK:_ Area/District/Sector format.
  * **`COUNTRY`** – SHA-256 hashed two-letter country code (ISO 3166-1 alpha-2).
    * _Example:_ `us`
  * **`DOBM`, `DOBD`, `DOBY`** – SHA-256 hashed birth month, day, and year.
    * _Format:_
      * **DOBM (MM):** `01-12`
      * **DOBD (DD):** `01-31`
      * **DOBY (YYYY):** `1900` to current year
  * **`MADID`** – Mobile Advertiser ID (**not hashed**)
    * _Example:_ `cdda802e-fb9c-47ad-0794d394c912`

💡 _For a complete list of supported match identifiers and multi-key matching guidelines, review the official_ [_Meta Custom Audiences Documentation._](https://developers.facebook.com/docs/marketing-api/audiences/guides/custom-audiences/#external_identifiers)

**Segment ID Field**

* **Field Name:** `segment_id`
* **Data Type:** String
* **Description:** The unique ID assigned by Meta for the specific audience segment.
* **Example:** `987654321`

**Segment Name Field**

* **Field Name:** `segment_name`
* **Data Type:** String
* **Description:** The name of the audience to be created in Meta. If a `segment_id` is not provided, MadConnect will use the `segment_name` to create a new audience in Meta and return the assigned `segment_id`.
* **Example:** `Winter Campaign Audience`

**Action Field**

* **Field Name:** `action`
* **Data Type:** String
* **Description:** Specifies whether to **add** or **remove** the user from the audience.
* **Accepted Values:** `add`, `remove`
* **Example:** `add`

***

#### **💡 How the Schema Works in MadConnect:**

* **Audience Creation:**
  * If a **`segment_name`** is provided and **`segment_id`** is not, MadConnect will **create a new audience** in Meta using the provided name.
  * The **`action`** field must be set to `add` during audience creation.
* **Managing Existing Audiences:**
  * If a **`segment_id`** exists, IDs will be **added or removed** based on the **`action`** field.
  * **Any valid ID field sent to Meta that the platform supports will be used for matching.** This allows flexibility for matching on a wide range of identifiers, from hashed emails and phone numbers to non-hashed Mobile Advertiser IDs (MADID).
* **Hashing Guidelines:**
  * **SHA-256 hashing is required** for all personal identifiers except for **External Identifiers**, **App User IDs**, **Page Scoped User IDs**, and **MAID**.
  * Normalize data before hashing (e.g., lowercase, remove whitespace/special characters).
  * Names (first/last) support special characters and non-Roman alphabets but should include Romanized versions for better match rates.
* **UI Enhancements:**
  * Users can configure metadata (e.g., **audience source**, **duration**) using dropdowns in the **MadConnect UI**.
  * These settings can be modified at the **connection level** as needed.



For more details on Meta Custom Audiences, please refer to the[ Meta Custom Audiences documentation](https://www.facebook.com/business/help/744354708981227).
