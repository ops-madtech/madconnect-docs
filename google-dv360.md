# Google DV360

![](<.gitbook/assets/image (24).png>)

#### Google Display & Video 360 Customer Match Connector Overview

MadConnect enables seamless integration with DV360 Customer Match, allowing you to upload and manage customer data for targeted advertising. Leverage your first-party data to create personalized ad campaigns, ensuring optimal reach and engagement. Maximize your advertising impact with efficient data synchronization directly from your CRM or data sources to DV360.

***

**Connector Overview**

* **Source / Destination**: Destination
* **Data Type**: Audience
* **Description**: Activate your customer match audiences on Google DV360 by syncing your customer data.
* **Supported Actions**: Add / Remove

***

**Prerequisites**

To activate audiences on Google DV360 using MadConnect, ensure the following prerequisites are met:

**Meet Google's Requirements for Using Customer Match**:

* **Policy Compliance**: The advertiser account must have a good history of policy compliance.
* **Payment History**: The advertiser account must have a good payment history.
* **Allowlisting**: Customers with compliant accounts are automatically allowlisted by Google.

**Account Requirements**:

* **OAuth Authentication**: Authenticate Google DV360 Account destination using OAuth.
* **Compliant Advertiser Account**: Ensure the account adheres to Google's Customer Match requirements.

**Credentials**:

* You can obtain your Google DV360 Account Credentials from your account settings in the Google Marketing Platform.

**Ensure the Audience(s) exist in the DV360 account.**



Warning: Verify that data upload features have been approved for the proposed audience's parent partner by first trying to create a Customer Match audience under a relevant advertiser in the UI. If data upload has not been enabled, attempts to create or update Customer Match audiences will return an error.

***

**Configure Connector**

To configure the DV360 Customer Match connector in MadConnect, follow these steps for authentication:

Follow these steps to configure the Google DV360 Customer Match connector in MadConnect:

1. **Navigate to the My Platforms Section**:
   * In the MadConnect UI, go to the **My Platforms** section.
2. **Add a New Platform**:&#x20;
   * Click on **Add Platform**.
3. **Select Google DV360 - Customer Match**:&#x20;
   * Choose the **Google DV360 - Customer Match** tile and click on **Configure**.
4. **Go to Destination Configuration**:&#x20;
   * Navigate to the **Configuration** tab.
5. **Sign in with Google**:&#x20;
   * Click the **Sign in with Google** button to authenticate your Google DV360 account.
6. **Authenticate with Google**:&#x20;
   * You will be redirected to Google’s OAuth login page. Sign in using your Google account credentials.
7. **Authorize MadConnect:**&#x20;
   * Grant MadConnect permission to access and manage your DV360 Customer Match audiences.
8. **Redirect to MadConnect**:&#x20;
   * After successful authentication, you will be redirected back to MadConnect, where the configuration will be completed.

***

**Creating Empty Customer Lists in DV360 for Audience Activation**

To successfully send an audience to DV360, it's necessary to create an empty customer list in DV360 and obtain the unique ID assigned by the system. Follow these steps:

1. **Login to DV360**:
   * Access the DV360 login page and sign in with your credentials.
2. **Navigate to Audiences**:
   * From the main menu, select the "Audiences" tab.
3. **Create a New Audience**:
   * Click on the "New Audience" button.
4. **Choose Customer Match**:
   * Select the "Customer Match" option from the audience type options.
5. **Enter Audience Details**:
   * **Name**: Provide a name for your audience list.
   * **Description**: Optionally, add a description for better clarity.
   * **Customer List Type**: Choose the type of customer data you will upload (e.g., emails, phone numbers).
6. **Save the Audience**:
   * Click the "Save" button to create the empty customer list. Uploading data and setting membership duration are not required at this step for an empty audience list.
7. **Obtain the Unique Audience ID**:
   * After saving, the system will generate a unique ID for the audience. Locate this ID in the audience details section.
8. **Share the Unique Audience ID**:
   * Provide the unique ID to the appropriate team or entity responsible for activating the audience.

For more information on Google DV360 Audience Activation, please review the[ Google Customer Match documentation](https://developers.google.com/display-video/api/guides/audiences/upload-customer-match).

***

_**Disclosure**_**:** _MadConnect's use and transfer of information received from Google APIs to any other app will adhere to_ [_Google API Services User Data Policy_](https://developers.google.com/terms/api-services-user-data-policy#additional\_requirements\_for\_specific\_api\_scopes)_, including the Limited Use requirements._
