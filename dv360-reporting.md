# DV360 - Reporting

## **Google Display & Video 360 Reporting Connector Overview**

MadConnect enables seamless integration with DV360 Reporting, allowing you to pull campaign performance and exposure data directly from Google DV360 into your analytics or reporting tools. Gain valuable insights to optimize your advertising strategies and improve campaign effectiveness with real-time data synchronization.

***

### **Connector Overview**

* **Source/Destination**: Source
* **Connector Type**: Reporting
* **Description**: Retrieve campaign performance and exposure data from Google DV360.
* **Supported Actions**: Get

***

### **Prerequisites**

To use the DV360 Reporting connector, ensure the following requirements are met:

1. **Authenticate Google DV360 Account**:
   * **OAuth Authentication**: Authenticate your Google DV360 account using OAuth to allow data access.
2. **DV360 User Role**:
   * Ensure the authenticating Google Account is a Display & Video 360 user with the necessary permissions (Read Only or higher).
3. **Google DV360 Account Credentials**:
   * Obtain credentials from your Google Marketing Platform account settings. Make sure your account complies with Google’s policy and has appropriate roles configured.
   * If you encounter issues, contact your Google representative.

***

### **Configure Connector**

1. **Navigate to My Platforms**
   * Go to "My Platforms" in the MadConnect UI.
2. **Add a New Platform**
   * Click on "Add Platform."
3. **Select Google DV360 Reporting Connector**
   * Choose the **"Google DV360 Reporting"** tile and click **"Configure."**
4. **Go to Configuration**
   * Open the **"Configuration"** tab.
5. **Sign in with Google DV360**
   * Click **"Sign in with Google DV360"** and log in using your Google account credentials.
6. **Authorize MadConnect**
   * Grant MadConnect permission to access and pull campaign performance data.
7. **Verify Configuration**
   * Ensure the platform status is marked as **"Configured"** under **My Platforms**.

***

For more information on the DV360 Reporting API, please review the [Google DV360 documentation](https://developers.google.com/bid-manager/reference/rest/v2/queries).
