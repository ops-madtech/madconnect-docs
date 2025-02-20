# Amazon DSP - Conversions

## **Amazon DSP - Conversions API Overview**

MadConnect enables seamless integration with Amazon DSP, allowing advertisers to import conversion event data. This API supports importing one source per request and allows partial imports. Conversion event data without a matching conversion source will be rejected.

### **Connector Overview**

* **Source/Destination**: Destination
* **Connector Type**: Conversion
* **Description**: Import conversion event data into Amazon DSP for enhanced campaign tracking and optimization.
* **Supported Actions**: Add

### **Prerequisites**

1. **Account ID**:
   * Your **DSP Advertiser ID** is required. Note: DSP Entity IDs are not supported.
2. **API Authentication**:
   * **Amazon-Advertising-API-ClientId**: The identifier for a client associated with a Login with Amazon account.
   * **Amazon-Advertising-API-Scope**: Profile ID associated with the advertiser account. Use the GET method on the Profiles resource to obtain this.
   * **Amazon-Ads-AccountId**: Your **DSP Advertiser ID** is required for access to DSP data.
3. **Supported Conversion Sources**:
   * Ensure your event data aligns with the following supported sources:
     * **AMAZON\_AD\_TAG**
     * **SERVER\_TO\_SERVER**

***

### **Configure Connector**

1. **Navigate to the My Platforms Section**
   * Go to **"My Platforms"** in the MadConnect UI.
2. **Add a New Platform**
   * Click on **"Add Platform"** in the UI.
3. **Select Amazon DSP - Conversions API**
   * Choose the **Amazon DSP - Conversions API** tile and click **"Configure."**
4. **Go to Configuration**
   * Open the **"Configuration"** tab for the selected platform.
5. **Authenticate with Amazon DSP**
   * Enter the required authentication credentials:
     * **Account ID**: Your DSP Advertiser ID.
     * **Amazon-Advertising-API-ClientId**: Client identifier linked to your Login with Amazon account.
     * **Amazon-Advertising-API-Scope**: Profile ID linked to your DSP account.
6. **Grant Permissions**
   * Ensure that MadConnect has permission to upload conversion event data for your account.
7. **Verify Configuration**
   * Confirm that the platform status is marked as **"Configured"** under **My Platforms**.

***

### **Conversion Event Schema**

Below is the schema required for importing conversion event data:

1. **eventData** (Array of objects):
   * **Description**: Array containing individual conversion events.
   * **Items**: Minimum 1 and maximum 100 conversion events per request.
2. **source** (String):
   * **Description**: Defines the source of the conversion data.
   * **Accepted Values**:
     * **AMAZON\_AD\_TAG**
     * **SERVER\_TO\_SERVER**

#### **Conversion Event Object Requirements**

Each object in the **eventData** array should include the following:

* **eventTime** (String): Timestamp of the conversion event in ISO-8601 format.
* **conversionValue** (Number): The monetary value attributed to the conversion.
* **userIdentifier** (String): Unique identifier for the user associated with the conversion.
* **conversionType** (String): Type of conversion event (e.g., purchase, signup).

For more information on the required fields, consult the [Amazon DSP API Documentation](https://advertising.amazon.com/API/docs/en-us/dsp-conversion-builder#tag/Conversion-Event-Data/operation/dspAmazonIngestConversionData).
