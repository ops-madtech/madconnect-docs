# META/Facebook Ads - Insights API

![](<.gitbook/assets/image (13).png>)

## **Meta Ads Reporting Connector Overview**

MadConnect enables seamless integration with Meta Ads Reporting, allowing you to pull campaign performance and exposure data directly from Meta Ads into your analytics or reporting tools. Gain valuable insights to optimize your advertising strategies and improve campaign effectiveness with real-time data synchronization.

***

### **Connector Overview**

* **Source/Destination**: Source
* **Connector Type**: Reporting
* **Description**: Retrieve campaign performance and exposure data from Meta Ads.
* **Supported Actions**: Get

***

### **Prerequisites**

To use the Meta Ads Reporting connector, ensure the following requirements are met:

1. **Authenticate Meta Account**:
   * **OAuth Authentication**: Authenticate your Meta account using OAuth to allow data access.
2. **Meta Ads User Role**:
   * Ensure the authenticating Meta account has the necessary permissions (Read Only or higher).
3. **Meta Ads Account Credentials**:
   * Obtain credentials from your Meta Business Suite account settings. Ensure your account complies with Meta’s policy and has appropriate roles configured.
   * If you encounter issues, contact your Meta representative.

***

### **Configure Connector**

1. **Navigate to My Platforms**
   * Go to "My Platforms" in the MadConnect UI.
2. **Add a New Platform**
   * Click on "Add Platform."
3. **Select Meta Ads Reporting Connector**
   * Choose the **"Meta Ads Reporting"** tile and click **"Configure."**
4. **Go to Configuration**
   * Open the **"Configuration"** tab.
5. **Sign in with Meta**
   * Click **"Sign in with Meta"** and log in using your Meta account credentials.
6. **Authorize MadConnect**
   * Grant MadConnect permission to access and pull campaign performance data.
7. **Verify Configuration**
   * Ensure the platform status is marked as **"Configured"** under **My Platforms**.

***

### **Standard Dimensions and Metrics Available**

Below are the standard dimensions and metrics available for reporting. This list is continually updated and may not reflect all available fields. Please connect with your MadConnect representative to request additional fields or to obtain the most up-to-date list of fields.

**Dimensions**

* date\_start
* date\_stop
* account\_id
* account\_name
* ad\_id
* ad\_name
* adset\_id
* adset\_name
* buying\_type
* campaign\_id
* campaign\_name
* conversion\_values
* device\_platform
* media\_asset
* objective
* publisher\_platform
* user\_segment\_key

**Metrics**

* actions
* clicks
* conversions
* impressions
* inline\_link\_clicks
* reach
* spend
* video\_continuous\_2\_sec\_watched\_actions
* video\_p100\_watched\_actions
* video\_p25\_watched\_actions
* video\_p50\_watched\_actions
* video\_p75\_watched\_actions

***

For more information on the Meta Ads Reporting API, please review the [Meta Ads documentation](https://developers.facebook.com/docs/marketing-api/insights/).

