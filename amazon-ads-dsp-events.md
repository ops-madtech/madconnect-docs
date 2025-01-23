# Amazon Ads -  DSP Events

![](<.gitbook/assets/image (30).png>)

## **Amazon Ads – Events API Connector Overview**

MadConnect's Amazon Ads – Events API Connector enables advertisers to upload conversion event data directly to Amazon Ads. This integration provides a seamless way to transmit offline, CRM, or server-side conversions, ensuring accurate campaign measurement and optimization.

***

### **Connector Overview**

* **Source / Destination**: Destination
* **Data Type**: Conversions
* **Description**: Sync conversion event data with Amazon Ads enhancing campaign tracking and performance analysis.
* **Supported Actions**: Add&#x20;

***

### **Prerequisites**

To activate this connector, ensure the following:

1. **Active Amazon Ads Account**
   * Maintain an active Amazon Ads account with permissions to manage DSP conversion events.
2. **User Role Permissions**
   * Ensure your account has the necessary permissions to access and manage conversion event data.
3. **Conversion Data Preparation**
   * Prepare your conversion event data in line with Amazon Ads API requirements. Each record should include necessary identifiers such as advertiserId, eventType, eventTime, and other relevant fields.

***

### **Configure Connector**

1. **Add Platform**
   * Navigate to **My Platforms** in the MadConnect UI.
   * Click on **Add Platform** and select **Amazon Ads – Events** from the available platform tiles.
2. **Destination Configuration**
   * Once the Amazon Ads platform is added, click **Configure** on the platform tile under **My Platforms**.
3. **Authenticate with Amazon**
   * Click the **Sign in with Amazon** button.
   * You will be redirected to Amazon’s OAuth login page. Sign in using your Amazon Ads account credentials.
4. **Authorize Access**
   * Grant MadConnect permission to access and manage your Amazon Ads conversions.
5. **Verify Configuration**
   * Ensure the connector status is marked as **Configured** under **My Platforms**.

***

### **Conversion Events Schema Requirements**

To send event data to Amazon Ads, adhere to the following schema:

1. **Advertiser ID**
   * **Field Name**: `advertiserId`
   * **Data Type**: String
   * **Description**: The unique identifier for the advertiser.
   * **Example**: `1234567890`
2. **Event Type**
   * **Field Name**: `eventType`
   * **Data Type**: String
   * **Description**: Specifies the type of conversion event (e.g., purchase, signup).
   * **Example**: `purchase`
3. **Event Time**
   * **Field Name**: `eventTime`
   * **Data Type**: String (ISO 8601 format)
   * **Description**: The timestamp of the conversion event.
   * **Example**: `2023-12-25T12:34:56Z`
4. **Event Value**
   * **Field Name**: `eventValue`
   * **Data Type**: Float
   * **Description**: The monetary value associated with the conversion event.
   * **Example**: `29.99`
5. **Currency**
   * **Field Name**: `currency`
   * **Data Type**: String (ISO 4217 format)
   * **Description**: The currency code of the event value.
   * **Example**: `USD`
6. **User Identifier**
   * **Field Name**: `userId`
   * **Data Type**: String
   * **Description**: A unique identifier for the user (e.g., hashed email, hashed phone, or mobile advertising ID).
   * **Example**: `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`
7. **Event Attribution Source (Optional)**
   * **Field Name**: `eventAttributionSource`
   * **Data Type**: String
   * **Description**: Source of the event data (e.g., AMAZON\_AD\_TAG or SERVER\_TO\_SERVER).
   * **Example**: `SERVER_TO_SERVER`

***

For more details on Amazon Ads’ Conversions API, refer to the [Amazon DSP Conversion Event API Documentation](https://advertising.amazon.com/API/docs/en-us/dsp-conversion-builder#tag/Conversion-Event-Data).

\
