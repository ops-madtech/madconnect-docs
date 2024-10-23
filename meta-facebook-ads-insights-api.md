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

1. date\_start
2. date\_stop
3. account\_id
4. account\_name
5. ad\_id
6. ad\_name
7. adset\_id
8. adset\_name
9. buying\_type
10. campaign\_id
11. campaign\_name
12. conversion\_values
13. device\_platform
14. media\_asset
15. objective
16. publisher\_platform
17. user\_segment\_key

**Metrics**

1. actions
2. clicks
3. conversions
4. impressions
5. inline\_link\_clicks
6. reach
7. spend
8. video\_continuous\_2\_sec\_watched\_actions
9. video\_p100\_watched\_actions
10. video\_p25\_watched\_actions
11. video\_p50\_watched\_actions
12. video\_p75\_watched\_actions

***

For more information on the Meta Ads Reporting API, please review the [Meta Ads documentation](https://developers.facebook.com/docs/marketing-api/insights/).

