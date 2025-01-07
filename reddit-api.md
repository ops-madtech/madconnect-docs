# Reddit API

![](<.gitbook/assets/image (46).png>)

## **Reddit Ads – Audience Connector Overview**

MadConnect integrates with Reddit Ads, enabling advertisers to seamlessly sync and manage their audiences. With this connector, businesses can efficiently target specific user segments on Reddit by leveraging existing customer data, ensuring relevant ad delivery and higher engagement.

### **Connector Overview**

* **Connector Type**: Destination
* **Data Type**: Audience
* **Description**: Audiences in Reddit Ads allow advertisers to target specific groups of users based on hashed emails or Mobile Advertising IDs (MAIDs).
* **Supported Actions**: Add / Remove

### **Prerequisites**

1. **Active Reddit Ads Account**
   * Ensure you have an active Reddit Ads account with the necessary permissions to manage audiences.
2. **OAuth Authentication**
   * Authenticate your Reddit Ads account using OAuth within MadConnect.
3. **Existing Audiences in Reddit Ads**
   * Verify the audience exists in Reddit Ads Manager; if not, create it before syncing data.

### **Configure Connector**

1. **Navigate to the My Platforms Section**
   * Go to "My Platforms" in the MadConnect UI.
2. **Add a New Platform**
   * Click on "Add Platform."
3. **Select Reddit Ads – Audience Connector**
   * Choose the "Reddit Ads – Audience" tile and click "Configure."
4. **Go to Destination Configuration**
   * Open the "Destination Configuration" tab.
5. **Sign in with Reddit**
   * Click "Sign in with Reddit" and log in with your account credentials.
6. **Authorize MadConnect**
   * Grant MadConnect permission to access and manage your Reddit audiences.
7. **Verify Configuration**
   * Ensure the platform status is marked as "Configured" under My Platforms.

### **Steps to Create an Empty Audience in Reddit Ads**

1. **Log in to Reddit Ads Manager**
   * Navigate to Reddit Ads Manager.
2. **Click on Audiences**
   * Select “Audiences” from the main menu.
3. **Create a New Audience**
   * Click on “Create Audience” and select the appropriate type.
4. **Name the Audience**
   * Provide a meaningful name for the audience.
5. **Select Data Type**
   * Choose whether to use emails or MAIDs for the audience data.
6. **Create the Audience**
   * Save the audience and make note of the audience ID for syncing purposes.

### **Audience Schema Requirements for Reddit Ads – Audience Connector**

To successfully sync data with Reddit Ads via MadConnect, the following schema must be used:

1. **ID Field**
   * **Field Name**: email\_hash, maid
   * **Data Type**: String (Hashed for emails; plain for MAIDs)
   * **Description**: Contains the hashed emails or Mobile Advertising IDs (MAIDs) of the audience members.
   * **Accepted Hashing Algorithms**: SHA-256
   * **Example**:
     * Email: `5d41402abc4b2a76b9719d911017c592`
     * MAID: `cdda802e-fb9c-47ad-0794d394c912`
2. **Segment ID Field**
   * **Field Name**: segment\_id
   * **Data Type**: String
   * **Description**: The unique identifier for the specific audience segment assigned by Reddit.
   * **Example**: `123456789`
3. **Action Field**
   * **Field Name**: action
   * **Data Type**: String
   * **Description**: Specifies the intended action for the audience member—either adding or removing the user from the segment.
   * **Accepted Values**: add, remove
   * **Example**: `add`

For more details on Reddit Ads audiences, please refer to the [Reddit Ads documentation](https://madtech.atlassian.net/jira/core/projects/MCM/board?selectedIssue=MCM-169).
