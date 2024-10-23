# Snapchat - Reporting

![](<.gitbook/assets/image (5).png>)

## **Snapchat Ads Reporting Connector Overview**

MadConnect enables seamless integration with Snapchat Ads Reporting, allowing you to pull campaign performance and exposure data directly from Snapchat Ads into your analytics or reporting tools. Gain valuable insights to optimize your advertising strategies and improve campaign effectiveness with real-time data synchronization.

***

### **Connector Overview**

* **Source/Destination**: Source
* **Connector Type**: Reporting
* **Description**: Retrieve campaign performance and exposure data from Snapchat Ads.
* **Supported Actions**: Get

***

### **Prerequisites**

1. **Authenticate Snapchat Ads Account**:
   * **OAuth Authentication**: Authenticate your Snapchat Ads account using OAuth to allow data access.
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
3. **Select Snapchat Ads Reporting Connector**
   * Choose the **"Snapchat Ads Reporting"** tile and click **"Configure."**
4. **Go to Configuration**
   * Open the **"Configuration"** tab.
5. **Sign in with Snapchat Ads**
   * Click **"Sign in with Snapchat Ads"** and log in using your Snapchat account credentials.
6. **Authorize MadConnect**
   * Grant MadConnect permission to access and pull campaign performance data.
7. **Verify Configuration**
   * Ensure the platform status is marked as **"Configured"** under **My Platforms**.

***

### **Core Dimensions and Metrics Available**

Below are the standard dimensions and metrics available for reporting. This list is continually updated and may not reflect all available fields. Please connect with your MadConnect representative to request additional fields or to obtain the most up-to-date list of fields.

#### **Recommended Dimensions**

1. day
2. campaign\_id
3. campaign
4. ad\_squad\_id
5. ad\_squad
6. ad\_id
7. ad\_name

#### **Recommended Metrics**

1. impressions
2. swipes
3. view\_time\_millis
4. screen\_time\_millis
5. quartile\_1
6. quartile\_2
7. quartile\_3
8. view\_completion
9. spend
10. video\_views
11. uniques
12. frequency

***

For more information on the Snapchat Ads Reporting API, please review the [Snapchat Ads documentation](https://developers.snap.com/api/marketing-api/Ads-API/measurement).
