# The Trade Desk - Raw Real Time Conversion Events

![](https://lh7-us.googleusercontent.com/wup3xBIBtaYy7UVLc1ABg55E98QIuY7WHthBrZInsQP40gI5Z\_ilROMdvobYgmdtPHQ66X9\_urGMGsZRmHGeSUTmxtGqq1pxo2fqCq16kBz762EbRkDHB5OFLOk58xvVDiuZL6NG2c9SL9xCyTcuvQ)

The [Trade Desk](https://www.thetradedesk.com/) is an online, demand-side platform that provides a self-service platform enabling ad buyers to manage data-driven digital advertising campaigns using their own teams across various advertising formats, including display, video, and social, and on a multitude of devices, including computers, mobile devices, and connected TV.

&#x20;The Trade Desk Real-Time Conversion Events API supports receiving events from partners so that clients can leverage them for retargeting and attribution. The API is accessible via HTTPS requests using the JSON format, which facilitates the process of connecting to the platform and recording user events.

The Real-Time Conversion Events API offers the following capabilities:

* Real-time conversion events.
* URL mapping.
* Multiple items in an event. For example, cart-level data.
* Support for different types of IDs, such as TDID, RampID (formerly IDL), Mobile Advertising IDs (IDFA, AAID, NAID, DAID), and Unified IDs such as UID2 and EUID.
* Custom fields TD1-TD10 for conversion event metadata, such as promo codes

[https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi)

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

#### Configuring Trade Desk - Raw Real-Time Conversion Event Data as Destination

To configure Trade Desk Raw Real-Time Conversions as destination,

1. &#x20;Goto to the The Trade Desk Raw Real-Time Conversions destination configuration page.
2. &#x20;Copy the TTD token in the token field.
3. &#x20;Click on the save button to save the credentials.
4. &#x20;Connector is now configured. Connection can be created using TTD Raw Real-Time  Conversion as destination

**Data Attributes for Raw Real-Time Conversion Event Data**

Sample request to add Raw Real-Time Conversion&#x20;

| <p>  <code>{</code></p><p>   <code>"data":[</code></p><p>      <code>{</code></p><p>         <code>"merchant_id":"a7b3sd",</code></p><p>         <code>"tracker_id":"hc7ihke",</code></p><p>         <code>"adv":"4w1ba8e",</code></p><p>         <code>"value":25.97,</code></p><p>         <code>"currency":"USD",</code></p><p>         <code>"event_name":"First Purchase",</code></p><p>         <code>"client_ip":"7.7.7.7",</code></p><p>         <code>"referrer_url":"site.com",</code></p><p>         <code>"adid":"w/0jDIIFc1N/8E5ASncaMCwi0XyI7uz+ONDVxBZKjj8=",</code></p><p>         <code>"adid_type":"UID2",</code></p><p>         <code>"imp":"49gCBlwj-9d33-0927-s9g7-sd7nF3D248d5",</code></p><p>         <code>"order_id":"abc123XyZ",</code></p><p>         <code>"items":[</code></p><p>            <code>{</code></p><p>               <code>"item_code":"203319203",</code></p><p>               <code>"name":"Deluxe Twin Room",</code></p><p>               <code>"qty":1,</code></p><p>               <code>"price":264.00,</code></p><p>               <code>"cat":"23"</code></p><p>            <code>},</code></p><p>            <code>{</code></p><p>               <code>"item_code":"#2234567892",</code></p><p>               <code>"name":"King Terrace Suite",</code></p><p>               <code>"qty":1,</code></p><p>               <code>"price":399.00,</code></p><p>               <code>"cat":"1110"</code></p><p>            <code>}</code></p><p>         <code>],</code></p><p>         <code>"td1":"Spring 2023 Promo",</code></p><p>         <code>"data_processing_option":{</code></p><p>            <code>"policies":[</code></p><p>               <code>"LDU"</code></p><p>            <code>],</code></p><p>            <code>"region":"US-CO"</code></p><p>         <code>}</code></p><p>      <code>}</code></p><p>   <code>]</code></p><p><code>}</code></p><p></p> |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
