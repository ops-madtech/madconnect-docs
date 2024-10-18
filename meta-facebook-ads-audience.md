# META / Facebook Ads - Audience

![](<.gitbook/assets/image (16).png>)

#### Meta Ads -Custom  Audiences Overview

MadConnect integrates seamlessly with Meta’s Custom Audiences API, enabling advertisers to effectively manage and target their audiences on Meta platforms, including Facebook and Instagram. With this connector, businesses can sync customer data directly from their data sources to Meta, ensuring more precise and impactful audience targeting.

***

**Connector Overview**

* **Connector Type**: Destination
* **Data Type**: Custom Audiences
* **Description**: Custom audiences allow advertisers to target their ads to a specific set of people with whom they have already established a relationship.
* **Supported Actions**: Add / Remove

***

#### Prerequisites

1. **Active Meta Ads Account**
   * Ensure you have an active Meta Ads account with permissions to manage Custom Audiences.
2. **OAuth Authentication**
   * Authenticate your Meta Ads account using OAuth within MadConnect.
3. **Existing Audiences in Meta Ads**
   * Verify the audience exists in Meta; if not, create it within Meta Ads Manager before syncing.

***

#### Configure Connector

1. **Navigate to the My Platforms Section**
   * Go to "My Platforms" in the MadConnect UI.
2. **Add a New Platform**
   * Click on "Add Platform."
3. **Select Meta - Custom Audiences**
   * Choose the "Meta - Custom Audiences" tile and click "Configure."
4. **Go to Destination Configuration**
   * Open the "Destination Configuration" tab.
5. **Sign in with Meta**
   * Click "Sign in with Meta" and log in using your Meta account credentials.
6. **Authorize MadConnect**
   * Grant MadConnect permission to access and manage your Meta Custom Audiences.
7. **Verify Configuration**
   * Ensure the platform status is marked as "Configured" under My Platforms.

***

#### Steps to Create an Empty Custom Audience in Meta

1. **Log in to Ads Manager**
   * Go to Meta Ads Manager and click on "Audiences" in the menu.
2. **Click New Audience**
   * Select "Create Audience" > "Custom Audience" > "Customer List."
3. **Create a Name**
   * Provide a name for the new audience.
4. **Select Data Type**
   * Choose the type of data used: Email, Phone Number, or Mobile Ad ID.
5. **Create the Audience**
   * Save the empty audience.
6. **Obtain the Audience ID**
   * Locate the audience ID in the Ads Manager and provide it to your representative for syncing.

***

#### **Audience Schema Requirements for Meta Ads – Custom Audiences**

To successfully send data to Meta Custom Audiences via MadConnect, the following minimum schema must be used:

1. **\<ID> Field**
   * **Field Name:** `email_hash`, `phone_hash`, `maid`
   * **Data Type:** String (Hashed for emails and phone numbers; plain for MAID)
   * **Description:** Contains the hashed emails, phone numbers, or plain Mobile Advertising IDs (MAIDs) of the audience members.
   * **Accepted Hashing Algorithms:** SHA-256
   * **Example:**
     * **Email:** `5d41402abc4b2a76b9719d911017c592`
     * **Phone:** `98f6bcd4621d373cade4e832627b4f6`
     * **MAID:** `cdda802e-fb9c-47ad-0794d394c912`
2. **Segment ID Field**
   * **Field Name:** `segment_id`
   * **Data Type:** String
   * **Description:** The unique identifier for the specific audience segment assigned by Meta.
   * **Example:** `987654321`
3. **Action Field**
   * **Field Name:** `action`
   * **Data Type:** String
   * **Description:** Specifies the intended action for the audience member—either adding or removing the user from the segment.
   * **Accepted Values:** `add`, `remove`
   * **Example:** `add`

For more details on Meta Custom Audiences, please refer to the[ Meta Custom Audiences documentation](https://www.facebook.com/business/help/744354708981227).
