# The Trade Desk - Raw Offline Measurement Conversion

![](https://lh7-us.googleusercontent.com/wup3xBIBtaYy7UVLc1ABg55E98QIuY7WHthBrZInsQP40gI5Z\_ilROMdvobYgmdtPHQ66X9\_urGMGsZRmHGeSUTmxtGqq1pxo2fqCq16kBz762EbRkDHB5OFLOk58xvVDiuZL6NG2c9SL9xCyTcuvQ)

The [Trade Desk](https://www.thetradedesk.com/) is an online, demand-side platform that provides a self-service platform enabling ad buyers to manage data-driven digital advertising campaigns using their own teams across various advertising formats, including display, video, and social, and on a multitude of devices, including computers,mobile devices, and connected TV.

Offline conversions can include use cases such as attributing in-store sales, place visits, or custom events. To facilitate insights into conversions in verticals where purchases predominantly occur offline, The Trade Desk enables partners and trusted marketplace providers to send offline measurement data to the platform and then use it for attribution and targeting.

[https://partner.thetradedesk.com/v3/portal/data/doc/DataOfflineMeasurement](https://partner.thetradedesk.com/v3/portal/data/doc/DataOfflineMeasurement)

**Connection Type:** Destination.

**Data Type:** Events.

#### Getting The Trade Desk API Credentials

1. &#x20;Log in to the TTD platform using your API credentials.
2. &#x20;At the top-right of the Developer Portal, click the User Profile icon, and select Manage API Tokens. The API Tokens page appears.
3. &#x20;Click GENERATE TOKEN. The Generate API Token dialog appears.
4. &#x20;Enter a descriptive name for your token. For example, include the tool name for which it be used or any other details that will help you distinguish between multiple tokens later.
5. &#x20;Select the token lifetime based on your key rotation strategy and integration needs. Recommended: at least 6 months.
6. &#x20;Click GENERATE TOKEN. A confirmation message appears with an API token.
7. &#x20;Copy the displayed API token, save it to your secrets management system for future reference, and then close the message.

#### Configuring Trade Desk - Raw Offline Measurement Conversion as Destination

To configure Trade Desk Raw Offline Conversions as destination,

1. &#x20;Goto to the The Trade Desk Raw Offline Conversions destination configuration page.
2. &#x20;Copy the TTD token in the token field.
3. &#x20;Click on the save button to save the credentials.
4. &#x20;Connector is now configured. Connection can be created using TTD Raw Offline Conversion as destination

#### Data Attributes for Raw Offline Measurement Conversion Data

Destination

Offline tag creation

| <p><code>{</code></p><p>    <code>"AdvertiserId": "advert12",</code></p><p>    <code>"TrackingTagName": "2022 Widget Phone Sales",</code></p><p>    <code>"OfflineDataProviderId": "provid34"</code></p><p><code>}</code></p> |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Upload offline conversion events:

| <p><code>{</code></p><p>    <code>"OfflineDataProviderId": "provid34",</code></p><p>    <code>"AdvertiserId": "advert12",</code></p><p>    <code>"PageStartIndex": 0,</code></p><p>    <code>"PageSize": 10</code></p><p><code>}</code></p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
