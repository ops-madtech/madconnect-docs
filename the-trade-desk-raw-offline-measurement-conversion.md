# The Trade Desk - Raw Offline Measurement Conversion

![](https://lh7-us.googleusercontent.com/wup3xBIBtaYy7UVLc1ABg55E98QIuY7WHthBrZInsQP40gI5Z_ilROMdvobYgmdtPHQ66X9_urGMGsZRmHGeSUTmxtGqq1pxo2fqCq16kBz762EbRkDHB5OFLOk58xvVDiuZL6NG2c9SL9xCyTcuvQ)

## The Trade Desk – Offline Conversions Connector Overview

MadConnect's The Trade Desk – Offline Conversions API Connector enables advertisers to upload user-level offline conversions directly to The Trade Desk. This integration supports use cases such as attributing in-store sales, visits, custom events, and more, ensuring comprehensive tracking and optimization of campaigns.

### Connector Overview

* **Source / Destination**: Destination
* **Data Type**: Conversions
* **Description**: Transmit offline conversion data, including events such as purchases or visits, directly to The Trade Desk to enhance measurement and optimize campaign performance.
* **Supported Actions**: Add

***

### Prerequisites

To activate this connector, ensure the following:

1. **Active The Trade Desk Account**
   * Maintain an active The Trade Desk account with permissions to manage offline conversion events.
2. **Data Provider ID**
   * Obtain a valid `DataProviderId` from your The Trade Desk Technical Account Manager.
   * Ensure the `DataProviderId` matches the `OfflineDataProviderId` field in the associated tracking tag.
3. **Tracking Tags**
   * Set up offline tracking tags using The Trade Desk platform or tracking tag endpoints.
4. **Authentication Credentials**
   * Obtain the following credentials:
     * **Advertiser ID**
     * **Advertiser Secret Key**
     * **Offline Data Provider ID**
5. **Conversion Data Preparation**
   * Prepare your offline conversion data following The Trade Desk schema, ensuring all required fields are included and formatted correctly.

***

### Configure Connector

1. **Add Platform**
   * Navigate to **My Platforms** in the MadConnect UI.
   * Click on **Add Platform** and select **The Trade Desk – Offline Conversions** from the available platform tiles.
2. **Destination Configuration**
   * Once the platform is added, click **Configure** on the platform tile under **My Platforms**.
3. **Authenticate with The Trade Desk**
   * Enter the following credentials into the configuration:
     * **Advertiser Secret Key**
   * MadConnect will validate these credentials to ensure secure data uploads.
4. **Verify Configuration**
   * Ensure the connector status is marked as **Configured** under **My Platforms**.

***

### Conversion Schema Requirements

To send offline conversion data to The Trade Desk, adhere to the following schema:

#### Data Provider ID

* **Field Name**: `DataProviderId`
* **Data Type**: String
* **Description**: The provider ID supplied by your The Trade Desk Technical Account Manager.
* **Example**: `dataproviderABC`

#### Items (Event Details)

* **Field Name**: `Items`
* **Data Type**: Array of objects
* **Description**: Contains event details for offline conversions.

Each object in the `Items` array must include:

1. **Impression ID (Optional)**
   * **Field Name**: `ImpressionId`
   * **Data Type**: GUID
   * **Description**: A unique identifier linking the conversion to an impression. Required if no other ID is provided.
   * **Example**: `49gCBlwj-9d33-0927-s9g7-sd7nF3D248d5`
2. **Tracking Tag ID**
   * **Field Name**: `TrackingTagId`
   * **Data Type**: String
   * **Description**: The ID of the offline tracking tag.
   * **Example**: `bjsks98`
3. **Timestamp**
   * **Field Name**: `TimestampUtc`
   * **Data Type**: String (ISO 8601 format)
   * **Description**: The time of the conversion event in UTC.
   * **Example**: `2023-03-23T22:11:13.2311903Z`
4. **Event Name (Required for Merchant Catalog)**
   * **Field Name**: `EventName`
   * **Data Type**: String
   * **Description**: A user-defined event name, such as `purchase`, `addtocart`, or `viewitem`.
   * **Example**: `purchase`
5. **Value (Optional)**
   * **Field Name**: `Value`
   * **Data Type**: Decimal
   * **Description**: The monetary value of the conversion.
   * **Example**: `19.98`
6. **Value Currency (Optional)**
   * **Field Name**: `ValueCurrency`
   * **Data Type**: String
   * **Description**: The currency of the conversion value, in ISO 4217 format.
   * **Example**: `USD`
7. **Country (Optional)**
   * **Field Name**: `Country`
   * **Data Type**: String
   * **Description**: The country where the conversion occurred, in ISO 3 format.
   * **Example**: `USA`
8. **Region (Required if Country is USA)**
   * **Field Name**: `Region`
   * **Data Type**: String
   * **Description**: The region where the conversion occurred.
   * **Example**: `NY`
9. **Order ID (Optional)**
   * **Field Name**: `OrderId`
   * **Data Type**: String
   * **Description**: A unique identifier for the transaction or conversion event.
   * **Example**: `abc123XyZ`

***

### Important Notes

* **Data Privacy**: Ensure compliance with GDPR, CCPA, and other regional privacy regulations by providing accurate privacy settings when applicable.
* **Batch Uploads**: Consolidate multiple conversions into fewer API calls to improve efficiency and reduce potential errors.
* **Timeliness**: Submit conversion data within 1–3 days of the transaction date. Data older than 25 days may not be processed.

For more details, refer to [The Trade Desk Offline Conversion API Documentation](https://partner.thetradedesk.com/v3/portal/data/doc/post-providerapi-offlineconversion).
