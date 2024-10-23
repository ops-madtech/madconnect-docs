# TikTok Marketing - Audience Management API

![](https://lh7-us.googleusercontent.com/-kPbeFBJpZqW\_8FCf85212FqYj6RITRO0ELqraJIMnLF-u1Zl0e3g0MJR-EgLsw9Nc1gxEkLmNhEm7qKR5KW16i9RoKbtNcZ9do4YzygoU8P7UBAhNUX8iCPqmXVqnzzQBqqp0YYXZcGN7r0ovybOA)\
\
**TikTok Audiences Connector Overview (Streaming API)**

MadConnect integrates seamlessly with TikTok’s Streaming API to manage audience segments efficiently. This connector allows bulk addition and removal of audience members, facilitating precise targeting for advertising campaigns.

***

#### **Connector Overview**

* **Connector Type:** Destination
* **Data Type:** Audiences
* **Description:** Manage TikTok audience segments by adding or removing user/device IDs using the Streaming API.
* **Supported Actions:** Add, Remove

***

#### **Prerequisites**

1. **TikTok Business Account:**
   * Ensure you have an active TikTok Business Account, with access associated with desired advertisers.
2. **OAuth Authentication**
   * Authenticate your TikTok account using OAuth within MadConnect.
3. **Existing Audience Segments:**
   * Confirm that the audience segments you wish to manage are already created within your TikTok account.

***

#### **Configure Connector**

1. **Navigate to the My Platforms Section:**
   * In the MadConnect UI, go to the "My Platforms" section.
2. **Add a New Platform:**
   * Click on "Add Platform."
3. **Select TikTok - Audiences:**
   * Choose the "TikTok - Audiences" tile and click on "Configure."
4. **Go to Destination Configuration:**
   * Navigate to the "Destination Configuration" tab.
5. **Authenticate with TikTok:**
   * Click on "Sign in with TikTok" to initiate the OAuth flow.
   * Enter your credentials and authorize MadConnect to access your TikTok account.
6. **Authorization:**
   * After logging in, you will be redirected to MadConnect with a green check mark.
7. **Verify Configuration**
   * Ensure the platform status is marked as "Configured" under My Platforms.

***

#### **Audience Schema Requirements**

To successfully send data to TikTok via the Streaming API, the following schema must be used:

1. **\<ID> Field**
   * **Field Name:** `email_hash`, `phone_hash`, `maid`
   * **Data Type:** String (Hashed for emails/phones; plain for MAID)
   * **Description:** Contains the hashed emails, phone numbers, or non-hashed MAIDs of audience members.
   * **Accepted Hashing Algorithm:** SHA-256 for emails/phone numbers.
   * **Example:**
     * **Email:** `5d41402abc4b2a76b9719d911017c592`
     * **MAID:** `cdda802e-fb9c-47ad-0794d394c912`
2. **Segment ID Field**
   * **Field Name:** `segment_id`
   * **Data Type:** String
   * **Description:** The unique ID assigned by TikTok for the specific audience segment.
   * **Example:** `123456`
3. **Action Field**
   * **Field Name:** `action`
   * **Data Type:** String
   * **Description:** Specifies the action to be performed on the audience member.
   * **Accepted Values:** `add`, `remove`
   * **Example:** `add`

***

For more detailed instructions, refer to the [TikTok Business API](https://business-api.tiktok.com/portal/docs?id=1739940585975809) documentation.

\
