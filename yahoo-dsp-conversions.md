# Yahoo DSP - Conversions

## **Yahoo DSP – Conversions API Connector Overview**

MadConnect’s Yahoo DSP – Conversions API Connector lets advertisers push privacy‑safe web, app, or offline conversion events directly into the Yahoo DSP. The connector replaces legacy Dot pixels, works with cookieless identifiers, and feeds the same attribution and optimisation rules configured in your seat.

***

### Connector Overview

| Field                    | Value                                                                                                                                                                              |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Source / Destination** | Destination                                                                                                                                                                        |
| **Data Type**            | Conversions                                                                                                                                                                        |
| **Description**          | Send conversion events to Yahoo’s Conversions API (CAPI) using a **Pixel ID (yp ID)** created in the DSP. Choose _streaming_ (multiple runs per day) or _batch_ (daily) ingestion. |
| **Supported Actions**    | Add ▸ Update ▸ Delete                                                                                                                                                              |

***

### Prerequisites

1. **Yahoo DSP Advertiser Seat** – Ability to create pixels and conversion rules.
2. **Pixel ID (yp ID)** – In _Tracking → Create Pixel_ generate a new pixel; copy its numeric ID.
3. **OAuth 2.0 Credentials** – Client ID and secret provided by Yahoo (public‑key exchange & allow‑listing).
4. **Access Token** – MadConnect builds a signed JWT and refreshes a 60‑minute access token automatically.

***

### Configure Connector

1. **Add Platform** → select **Yahoo DSP – Conversions API**.
2. **Authenticate** → paste client credentials; MadConnect handles JWT → token workflow.
3. **Pixel ID** → enter the numeric yp ID.
4. **Mode** → choose _Streaming_ or _Batch_ processing.
5. **Verify** → connector status shows **Configured** when the token test succeeds.

***

### Conversion Schema Requirements

| Field                           | Required?   | Notes                                                                                                                       |
| ------------------------------- | ----------- | --------------------------------------------------------------------------------------------------------------------------- |
| `eventTs`                       | Yes         | Unix timestamp (sec or ms).                                                                                                 |
| `actionSource`                  | Yes         | `web`, `app`, `phone`, `email`, `online`, `physical_store`.                                                                 |
| `userData`                      | Conditional | At least one identifier (`email`, `phone`, `gpsaid`, `idfa`, `pxid`) **or** `clickData`. Hash email/phone with **SHA‑256**. |
| `clickData.vmcid`               | Conditional | Yahoo Click ID if no userData identifiers present.                                                                          |
| `eventName`                     | Optional    | Label used in DSP rules (`Purchase`, `AddToCart`, etc.).                                                                    |
| `eventData.price`               | Optional    | Numeric value (assumed USD if no currency field).                                                                           |
| `eventData.products[].category` | Optional    | Maps to Event‑Type Category rule in DSP.                                                                                    |
| `privacy.optOut`                | Optional    | `true` excludes the event from optimisation.                                                                                |

***

For detailed CAPI setup, authentication instructions, and complete field lists, please refer to Yahoo’s documentation: [https://help.yahooinc.com/dsp-api/docs/standard-yahoo-conversion-api](https://help.yahooinc.com/dsp-api/docs/standard-yahoo-conversion-api)
