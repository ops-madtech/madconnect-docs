# SnapChat- Conversions API

![](<.gitbook/assets/image (5).png>)

#### Snapchat Marketing API for Customer Lists - Connection Overview

MadConnect integrates seamlessly with the Snapchat Marketing API, enabling you to manage customer lists by adding or removing records for targeted advertising. This connector allows you to efficiently sync your customer data directly to Snapchat, ensuring your campaigns reach the right audience.

***

**Connector Overview**

* **Source / Destination**: Destination
* **Data Type**: Audience
* **Description**: Manage Snapchat customer lists by adding or removing records for targeted campaigns.
* **Supported Actions**: Add, Remove

***

**Prerequisites**

To set up the Snapchat Marketing API for Customer Lists in MadConnect, ensure the following prerequisites are met:

1. **Authenticate using OAuth:**
   * Navigate to the connector configuration and authenticate your Snapchat account using OAuth.
2. **Ensure Existing Audience(s):**
   * Verify that the audience(s) you plan to update already exist in the Snapchat Ads Manager. MadConnect can update existing audiences but does not create new ones at this time.

***

**Configure Connector**

1. **Navigate to the My Platforms Section:**
   * In the MadConnect UI, go to the "My Platforms" section.
2. **Add a New Platform:**
   * Click on "Add Platform."
3. **Select Snapchat - Customer Lists:**
   * Choose the "Snapchat - Customer Lists" tile and click on "Configure."
4. **Go to Configuration:**
   * Navigate to the "Configuration" tab.
5. **Sign in with Snapchat:**
   * Click the "Sign in with Snapchat" button.
6. **Authenticate with Snapchat:**
   * You will be redirected to Snapchat’s login page. Sign in using your Snapchat account credentials.
7. **Authorize MadConnect:**
   * Grant MadConnect permission to access and manage your Snapchat account.
8. **Verify Configuration:**
   * Ensure the connector status is marked as "Configured" under My Platforms.

***

**Create a Customer List Audience**

To create an empty Customer List Audience in Snapchat:

1. **Log in to Ads Manager:**
   * Click the menu in the top left corner and select "Audiences."
2. **Click New Audience:**
   * Select "Custom Audience" and choose "Customer List."
3. **Create a Name:**
   * Provide a name for the Audience.
4. **Select the Data Type:**
   * Choose the data type for the Audience – Email, Phone Number, or Mobile Ad ID.
5. **Create the Audience:**
   * Save the empty audience.
6. **Obtain the Snapchat Segment ID:**
   * Provide the Segment ID to your representative.

***

**Where Can I Find My Snapchat Segment ID?**

To obtain your Snapchat Segment ID, follow these steps:

1. Log in to your Snapchat Ads Manager: Go to your dashboard.
2. Go to Assets > Audiences: Under the Audience Library, select the required audience.
3. Copy the Segment ID: The ID appears after /audiences/custom-audience/ in the URL of the resulting page.

***

**Audience Schema Requirements**

To successfully send data to Snapchat Ads as audiences via MadConnect, the following minimum schema must be used:

1. \<ID> Field
   * **Field Name**: EMAIL\_HASH, PHONE\_HASH
   * **Data Type**: String (Hashed)
   * **Description**: This field contains the hashed emails or phone numbers or unhashed MAIDs of the audience members
   * **Accepted Hashing Algorithms**: SHA-256
   * **Example**: A hashed email such as 5d41402abc4b2a76b9719d911017c592.
2. Segment ID Field
   * **Field Name**: segment\_id
   * **Data Type**: String
   * **Description**: The unique ID assigned by Snapchat Ads for the specific audience segment.
   * **Example**: 123456
3. Action Field
   * **Field Name**: action
   * **Data Type**: String
   * **Description**: Specifies whether to add or remove the user from the audience.
   * **Accepted Values**: add, remove
   * **Example**: add



For more detailed instructions on the Snapchat Marketing API and managing customer lists, refer to the [Snapchat Marketing API](https://developers.snap.com/api/marketing-api/Ads-API/customer-lists) documentation.
