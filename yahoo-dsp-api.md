# Yahoo - DSP API

MadConnect enables seamless integration with Yahoo DSP Reporting, allowing you to pull campaign performance and exposure data directly from Yahoo DSP into your analytics or reporting tools. Gain valuable insights to optimize your advertising strategies and improve campaign effectiveness with real-time data synchronization.

***

**Connector Overview**

**Connector Type**: Source\
**Data Type**: Reporting\
**Description**: Retrieve campaign performance and exposure data from Yahoo DSP.\
**Supported Actions**: Get

***

**Prerequisites**

**Authenticate Yahoo DSP Account**:

* **Client ID** and **Client Secret**: Obtain your Yahoo DSP API credentials (Client ID and Client Secret) from your Yahoo account manager or the Yahoo DSP developer portal.
* Use these credentials to authenticate your Yahoo DSP account within MadConnect.

**User Role and Permissions**:

* Ensure that the authenticating account has the necessary permissions to access reporting data in Yahoo DSP.

**API Access**:

* Ensure that API access is enabled for your Yahoo DSP account. This might involve setting up API access through your Yahoo DSP account settings or contacting your Yahoo representative.

***

**Steps to Obtain Credentials:**

**Get Client ID and Secret**:

* Open the DSP UI and select your name in the upper-right corner.
* Select **My Account** and then the **Activate** button.
* Agree to the terms of service to receive a success message with your Client ID and Secret.
* Copy and store the Client ID and Secret safely.

***

**Configure Connector**

1. Navigate to the **My Platforms** Section:\
   In the MadConnect UI, go to the **My Platforms** section.
2. **Add a New Platform**:\
   Click on **Add Platform**.
3. Select **Yahoo DSP - Reporting**:\
   Choose the **Yahoo DSP - Reporting** tile and click on **Configure**.
4. Go to **Source Configuration**:\
   Navigate to the **Destination Configuration** tab.
5. Enter **Client ID** and **Client Secret**:\
   Input your Yahoo DSP **Client ID** and **Client Secret** into the provided fields.
6. **Verify Configuration**:\
   Ensure the connector status is marked as **Configured** under My Platforms..

***

**Additional Considerations**

* **Data Retrieval Limits**: Be aware of any data retrieval limits or rate limits that may apply when using the Yahoo DSP API for reporting.

For more detailed information on using the [Yahoo DSP Reporting API](https://developer.yahooinc.com/dsp/api/docs/reporting/dimensions.html), please review the Yahoo DSP API documentation or contact your Yahoo representative.

<br>
