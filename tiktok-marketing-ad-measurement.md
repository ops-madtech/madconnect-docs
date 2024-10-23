# Tiktok Marketing - Ad Measurement

![](https://lh7-us.googleusercontent.com/-kPbeFBJpZqW\_8FCf85212FqYj6RITRO0ELqraJIMnLF-u1Zl0e3g0MJR-EgLsw9Nc1gxEkLmNhEm7qKR5KW16i9RoKbtNcZ9do4YzygoU8P7UBAhNUX8iCPqmXVqnzzQBqqp0YYXZcGN7r0ovybOA)

## **TikTok Ads Reporting Connector Overview**

MadConnect enables seamless integration with TikTok Ads Reporting, allowing you to pull campaign performance and exposure data directly from TikTok Ads into your analytics or reporting tools. Gain valuable insights to optimize your advertising strategies and improve campaign effectiveness with real-time data synchronization.

***

### **Connector Overview**

* **Source/Destination**: Source
* **Connector Type**: Reporting
* **Description**: Retrieve campaign performance and exposure data from TikTok Ads.
* **Supported Actions**: Get

***

### **Prerequisites**

1. **Authenticate TikTok Ads Account**:
   * **OAuth Authentication**: Authenticate your TikTok Ads account using OAuth to allow data access.
2. **Data Access Permissions**:
   * Ensure the authenticating account has the appropriate roles or permissions.
3. **Test Report in UI**:
   * We recommend first building the desired report in the TikTok Ads UI as a test before proceeding with configuring the reporting connection.

***

### **Configure Connector**

1. **Navigate to My Platforms**
   * Go to "My Platforms" in the MadConnect UI.
2. **Add a New Platform**
   * Click on "Add Platform."
3. **Select TikTok Ads Reporting Connector**
   * Choose the **"TikTok Ads Reporting"** tile and click **"Configure."**
4. **Go to Configuration**
   * Open the **"Configuration"** tab.
5. **Sign in with TikTok Ads**
   * Click **"Sign in with TikTok Ads"** and log in using your TikTok account credentials.
6. **Authorize MadConnect**
   * Grant MadConnect permission to access and pull campaign performance data.
7. **Verify Configuration**
   * Ensure the platform status is marked as **"Configured"** under **My Platforms**.

***

### **Core Dimensions and Metrics Available**

Below are the standard dimensions and metrics available for reporting. This list is continually updated and may not reflect all available fields. Please connect with your MadConnect representative to request additional fields or to verify the most up-to-date list of fields.

#### **Dimensions**

1. advertiser\_id: Advertiser account ID
2. advertiser\_name: Advertiser account name
3. ad\_account\_id: The ID of the ad account
4. ad\_account\_name: The name of the ad account
5. campaign\_id: The ID of the campaign
6. campaign\_name: The name of the campaign
7. adgroup\_id: The ID of the ad group
8. adgroup\_name: The name of the ad group
9. ad\_id: The ID of the specific ad
10. ad\_name: The name of the specific ad
11. date: The time period (daily, weekly) for time-based reports

#### **Metrics**

1. impressions: The number of times the ad was shown
2. clicks: The number of clicks on the ad
3. spend: The total amount spent on the ad
4. conversion: The number of conversions from the ad
5. ctr: Clicks as a percentage of impressions
6. cpc: The average cost per click
7. cpm: Average amount spent per 1,000 impressions
8. conversion\_rate\_v2: Percentage of clicks resulting in a conversion
9. reach: The number of unique users who saw the ad
10. frequency: The average number of times each person saw the ad
11. video\_play\_actions: The number of video views for video ads
12. video\_watched\_2s: 2-second video views
13. video\_watched\_6s: 6-second video views
14. video\_views\_p25: Video views at 25%
15. video\_views\_p50: Video views at 50%
16. video\_views\_p75: Video views at 75%
17. video\_views\_p100: Video views at 100%
18. engagements: The number of engagements (likes, shares, etc.)

***

For more information on the TikTok Ads Reporting API, please review the [TikTok Ads documentation](https://business-api.tiktok.com/portal/docs?id=1738864915188737).

