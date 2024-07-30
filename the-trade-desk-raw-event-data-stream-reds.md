# The Trade Desk - Raw Event Data Stream (REDS)

![](https://lh7-us.googleusercontent.com/wup3xBIBtaYy7UVLc1ABg55E98QIuY7WHthBrZInsQP40gI5Z\_ilROMdvobYgmdtPHQ66X9\_urGMGsZRmHGeSUTmxtGqq1pxo2fqCq16kBz762EbRkDHB5OFLOk58xvVDiuZL6NG2c9SL9xCyTcuvQ)

The [Trade Desk](https://www.thetradedesk.com/) is an online, demand-side platform that provides a self-service platform enabling ad buyers to manage data-driven digital advertising campaigns using their own teams across various advertising formats, including display, video, and social, and on a multitude of devices, including computers, mobile devices, and connected TV.

Raw Event Data Stream (REDS) is one of the three [reporting solutions](https://partner.thetradedesk.com/v3/portal/reds/overview) in The Trade Desk platform. It provides partners with log-level data collected from bidding partners. REDS collects raw, log-level bidding events from won auctions and pixels on an hourly basis.

Events are separated into four streams by type:

* Impressions
* Clicks
* Video Events
* Clicks

[https://partner.thetradedesk.com/v3/portal/reds/doc/REDSGeneralInfo](https://partner.thetradedesk.com/v3/portal/reds/doc/REDSGeneralInfo)

[https://partner.thetradedesk.com/v3/portal/reds/doc/REDSGetStarted](https://partner.thetradedesk.com/v3/portal/reds/doc/REDSGetStarted)

**Connection Type:** Source.

**Data Type:** Reporting.

#### Getting The Trade Desk API Credentials

1. &#x20;Log in to the TTD platform using your API credentials.
2. &#x20;At the top-right of the Developer Portal, click the User Profile icon, and select Manage API Tokens. The API Tokens page appears.
3. &#x20;Click GENERATE TOKEN. The Generate API Token dialog appears.
4. &#x20;Enter a descriptive name for your token. For example, include the tool name for which it be used or any other details that will help you distinguish between multiple tokens later.
5. &#x20;Select the token lifetime based on your key rotation strategy and integration needs. Recommended: at least 6 months.
6. &#x20;Click GENERATE TOKEN. A confirmation message appears with an API token.
7. &#x20;Copy the displayed API token, save it to your secrets management system for future reference, and then close the message.

#### Configuring Trade Desk - REDS as Source

To configure Trade Desk REDS as source,

1. &#x20;Goto to the The Trade Desk REDS source configuration page.
2. &#x20;Copy the TTD token in the token field.
3. &#x20;Click on the save button to save the credentials.
4. &#x20;Connector is now configured. Connection can be created using TTD Raw Real-Time  Conversion as destination.

