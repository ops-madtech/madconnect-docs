# Google Campaign Manager 360 - Conversions

## **Campaign Manager 360 – Conversions API Connector Overview**

MadConnect's Campaign Manager 360 – Conversions API Connector enables advertisers to upload offline conversion data directly to Campaign Manager 360. This integration bridges online and offline activity, providing a comprehensive view of campaign performance and enabling better optimization of advertising efforts.

***

### **Connector Overview**

* **Source / Destination**: Destination
* **Data Type**: Conversions
* **Description**: Integrate offline conversion data into Campaign Manager 360, allowing for enhanced tracking and measurement of user actions that begin online and conclude offline.
* **Supported Actions**: Add / Update

***

### **Prerequisites**

To activate this connector, ensure the following:

1. **Active Campaign Manager 360 Account**
   * Maintain an active Campaign Manager 360 account with permissions to manage conversion events.
2. **User Role Permissions**
   * Verify that your Campaign Manager 360 user profile has the "Insert offline conversions" and "Update offline conversions" permissions enabled. Contact your account administrator if adjustments are needed.
3. **Floodlight Configuration**
   * Ensure that offline conversions are matched to a corresponding online activity in Campaign Manager 360. You'll refer to these activities by their `floodlightActivityId` and corresponding `floodlightConfigurationId`. If you don't have an activity for tracking offline conversions, you can use the API's FloodlightActivities service to create one.
4. **Conversion Data Preparation**
   * Prepare your conversion data, ensuring each record includes the necessary identifiers and is formatted according to API specifications.

***

### **Configure Connector**

1. **Add Platform:**
   * Navigate to **My Platforms** in the MadConnect UI.
   * Click on **Add Platform** and select **Google** **Campaign Manager 360 – Conversions** from the available platform tiles.
2. **Destination Configuration**:
   * Once Google CM360 platform is added, click **Configure** on the platform tile under **My Platforms**.
3. **Authenticate with Google**
   * Click the "Sign in with Google" button.
   * You will be redirected to Google's login page. Sign in using your Google account credentials associated with Campaign Manager 360.
4. **Authorize Access**
   * Grant the platform permission to access and manage your Campaign Manager 360 conversions.
5. **Verify Configuration**
   * Ensure the connector status is marked as "Configured" under your integrations.

***

### **Conversion Schema Requirements**

To send conversion data to Campaign Manager 360, adhere to the following schema:

1. **Floodlight Activity ID**
   * **Field Name**: `floodlightActivityId`
   * **Data Type**: Integer
   * **Description**: The ID of the Floodlight activity associated with this conversion.
   * **Example**: `1234567`
2. **Floodlight Configuration ID**
   * **Field Name**: `floodlightConfigurationId`
   * **Data Type**: Integer
   * **Description**: The ID of the Floodlight configuration used by the specified activity.
   * **Example**: `7654321`
3. **Timestamp**
   * **Field Name**: `timestampMicros`
   * **Data Type**: Integer (microseconds since Unix epoch)
   * **Description**: The timestamp of the conversion event.
   * **Example**: `1705508777000000`
4. **User Identifier**
   * **Field Name**: `gclid`, `dclid`, `encryptedUserId`, `mobileDeviceId`, or `matchId`
   * **Data Type**: String
   * **Description**: A unique identifier for the user, such as Google Click ID, Display Click ID, encrypted user ID, mobile device ID, or a custom match ID.
   * **Example**: `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`
5. **Ordinal**
   * **Field Name**: `ordinal`
   * **Data Type**: Integer
   * **Description**: A value used to control how conversions from the same user and day are deduplicated.
   * **Example**: `1`
6. **Conversion Value**
   * **Field Name**: `value`
   * **Data Type**: Double
   * **Description**: The monetary value associated with the conversion.
   * **Example**: `29.99`
7. **Conversion Quantity**
   * **Field Name**: `quantity`
   * **Data Type**: Integer
   * **Description**: The number of items associated with the conversion.
   * **Example**: `1`
