# The Trade Desk - Audience Activation

#### TheTradeDesk First-Party Data Connector Overview

MadConnect supports adding your data to first-party data segments in The Trade Desk. Enhance your The Trade Desk platform with real-time data directly from your data sources. Keeping your data current empowers your team to deliver exceptional advertising experiences. Leverage the full potential of The Trade Desk for audience activation through the First-Party Data API, ensuring your campaigns are always optimized and effectively targeted.

***

**Connector Overview**

* **Source / Destination**: Destination
* **Data Type**: Audience
* **Description**: Sync your first-party IDs (UID2, RampID) from any source to The Trade Desk.
* **Supported Actions**: Add / Remove

***

**Prerequisites**

To activate the connector, ensure the following:

1. **Contact TTD Representative**:
   * Connect with your The Trade Desk representative for any paperwork needed for audience activation.
   * Paperwork includes an **Access Letter** giving MadConnect permission to transfer data and a **UID2 Agreement**.
2. **Authentication Requirements**:
   * **Advertiser ID**: Obtain the Advertiser ID for the TTD account.
   * **Advertiser Secret Key**: Retrieve the secret key from The Trade Desk UI under _**Advertiser Preferences**_ > _**Seat Identifiers & Keys**_. If the secret key is unavailable, reach out to your TTD Account Manager for assistance.

***

**Configure Connector**

To configure the The Trade Desk First-Party Data API connector in MadConnect, follow these steps:

1. **Add Platform**:
   * Navigate to **My Platforms** in the MadConnect UI.
   * Click on **Add Platform** and select **The Trade Desk - First-Party Data** from the available platform tiles.
2. **Destination Configuration**:
   * Once The Trade Desk platform is added, click **Configure** on the platform tile under **My Platforms**.
3. **Authentication Details**:
   * In the **Configuration** tab, input the required **Advertiser ID** and **Advertiser Secret Key**.
   * The Advertiser Secret Key can be retrieved from _**Advertiser Preferences > Seat Identifiers & Keys**_ in the TTD UI.
   * Ensure the secret key is cross-checked with the one stored by The Trade Desk for the corresponding Advertiser ID.
4. **Verify**:
   * Once authentication is complete, you can verify that the connection is properly configured under **My Platforms**.

**Audience Schema Requirements**

To successfully send data to The Trade Desk First Party API via MadConnect, the following minimum schema must be used:

1. \<ID> Field
   * **Field Name**: TDID, UID2, DAID&#x20;
   * **Data Type**: String (Hashed)
   * **Description**: This field contains the accepted First Party ID as listed by The Trade Desk [documentation](https://partner.thetradedesk.com/v3/portal/data/doc/post-data-advertiser-firstparty#supported-ids).
   * **Example**: A raw UID2 value such as 48MjlfIUZpOKNAm9nod7/jCLAXUYsnE1tpVHQSDS0uo=.
2. Segment Name Field
   * **Field Name**: segment\_name
   * **Data Type**: String
   * **Description**: The name of the new audience you are creating OR name of existing audience to update as it appears in TTD UI.
   * **Example**: q1\_campaign\_signups
3. Action Field
   * **Field Name**: action
   * **Data Type**: String
   * **Description**: Specifies whether to add or remove the user from the audience.
   * **Accepted Values**: add, remove
   * **Example**: add

For more information on the First-Party Data API, please review the[ TTD documentation.](https://partner.thetradedesk.com/v3/portal/data/doc/post-data-advertiser-firstparty#supported-ids)

***

\
