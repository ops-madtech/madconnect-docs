# META / Facebook Ads - Conversions

![](<.gitbook/assets/image (15).png>)

## **Meta Ads – Conversions API Connector Overview**

MadConnect’s **Meta Ads – Conversions API Connector** lets you send offline or server‑generated conversion events to Meta for attribution and optimisation. The connector posts events to the **Offline Conversions endpoint** (`/{dataset_id}/events`). Under Meta’s new _unified dataset_ model, this **Dataset ID may be identical to your Pixel ID** when a pixel has been promoted to a dataset. Regardless, the API call always uses the numeric **Dataset ID**.

***

## Connector Overview

|                          |                                                                                                                                                                                                        |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Source / Destination** | Destination                                                                                                                                                                                            |
| **Data Type**            | Conversions                                                                                                                                                                                            |
| **Description**          | Sync offline and server‑side conversion events to Meta Ads using the Conversions API. Events are routed via a **Dataset ID** which may correspond to your existing Pixel ID under Meta’s unified view. |
| **Supported Actions**    | Add / Update / Delete                                                                                                                                                                                  |

***

### Prerequisites

1. **Meta Business Manager Access** – Access to the Business Manager that owns the dataset.
2. **Dataset ID (Offline Event Set)** – Create or locate the dataset in **Events Manager → Add Data Source → Offline Event Set**. If your pixel has been automatically converted, the dataset ID will be the **same number** as the pixel ID.
3. **Ad‑Account Permissions** – The target ad account must be assigned to the dataset.
4. **System User Access Token** – OAuth token with the `ads_management` scope to post events.

***

### Configure Connector

1. **Add Platform** → Select **Meta Ads – Conversions**.
2. **Authenticate** → Sign in with Meta (OAuth) and approve the required scopes.
3. **Verify** → Connector status should show **Configured** under _My Platforms_.
4. **Enter Dataset ID** → Paste the numeric Dataset ID (can equal Pixel ID if unified).
   1. This step is done within the **Create Connection** screen.

***

### Conversion Schema Requirements

| Field           | Required?   | Notes                                                  |
| --------------- | ----------- | ------------------------------------------------------ |
| `event_name`    | Yes         | e.g. `Purchase`, `Lead`                                |
| `event_time`    | Yes         | Unix epoch **seconds**                                 |
| `action_source` | Yes         | `physical_store`, `call_center`, or `system_generated` |
| `user_data`     | Yes         | At least one hashed identifier (`em`, `ph`, etc.)      |
| `custom_data`   | Optional    | `value`, `currency`, product info, etc.                |
| `event_id`      | Recommended | Deduplication key                                      |

The **Dataset ID** is used in the request URL, not in the JSON payload.

***

### Important Notes

1. If a pixel has been promoted to a dataset, its numeric ID can be used directly as the Dataset ID.
2. Ensure all user identifiers are **SHA‑256 hashed** before sending.
3. Events must be uploaded within Meta’s attribution window (28 days default).
4. Use `event_id` to deduplicate events sent from multiple sources.

For additional guidance see Meta documentation:

1. Unified Datasets: [https://www.facebook.com/business/help/5270377362999582](https://www.facebook.com/business/help/5270377362999582)
2. Offline Conversions API: [https://developers.facebook.com/docs/marketing-api/conversions-api/offline-events](https://developers.facebook.com/docs/marketing-api/conversions-api/offline-events)



