# Reddit Ads - Reporting

### Reddit Ads - Reporting&#x20;

MadConnect enables seamless integration with Reddit Ads Reporting, allowing you to pull detailed insights and analytics on ad performance, audience engagement, and other relevant metrics directly from Reddit Ads into your analytics or reporting tools. Gain valuable insights to optimize your advertising strategies and improve campaign effectiveness with automated data synchronization.

***

### **Connector Overview**

* **Source/Destination**: Source
* **Connector Type**: Reporting
* **Description**: Retrieve campaign performance Reddit Ads.
* **Supported Actions**: Get

***

### **Prerequisites**

1. **Authenticate Reddit Ads Account**:
   * **OAuth Authentication**: Authenticate your Reddit Ads account using OAuth to allow data access.
   *   **Manual Credentials**: Reddit Ads primarily uses OAuth. In limited cases, token-based authentication may be supported depending on account setup.

       Required fields:

       * Access Token
       * Refresh Token
       * Client ID / Client Secret

       These must align with an Reddit Ads developer application with reporting permissions.
2. **Data Access Permissions**:
   * Ensure the authenticating account has the appropriate roles or permissions.
3. **Test Report in UI**:
   * We recommend first building the desired report in the UI as a test before proceeding with configuring the reporting connection.

***

### **Configure Connector**

1. **Navigate to My Platforms**
   * Go to "My Platforms" in the MadConnect UI.
2. **Add a New Platform**
   * Click on "Add Platform."
3. **Select Reddit Ads Reporting Connector**
   * Choose the **"Reddit Ads Reporting"** tile and click **"Configure."**
4. **Go to Configuration**
   * Open the **"Configuration"** tab.
5. **Sign in with Reddit Ads**
   * Click **"Sign in with Reddit Ads"** and log in using your Reddit account credentials.
6. **Authorize MadConnect**
   * Grant MadConnect permission to access and pull campaign performance data.
7. **Verify Configuration**
   * Ensure the platform status is marked as **"Configured"** under **My Platforms**.

***

For more information on the Reddit Ads Reporting API, please review the [Reddit Ads API documentation](https://ads-api.reddit.com/docs/v3/api/get-a-report).
