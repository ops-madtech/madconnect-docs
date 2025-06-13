# TikTok - Events

## **TikTok Events API 2.0 Connector Overview**

MadConnect’s **TikTok Events API 2.0 Connector** delivers server‑side event data straight to TikTok Ads Manager, enabling more accurate attribution, optimization, and measurement without relying on browser pixels.

***

### Connector Overview

|                          |                                                                                                                   |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------- |
| **Source / Destination** | Destination                                                                                                       |
| **Data Type**            | Events                                                                                                            |
| **Description**          | Sync server‑generated events (e.g., purchases, adds‑to‑cart, sign‑ups) to TikTok via the Events API 2.0 endpoint. |
| **Supported Actions**    | Add                                                                                                               |

***

### Prerequisites

1. **Active TikTok Ads Account** – User must have permissions to manage events.
2. **Access Token** – Generate in TikTok Business Center (Events API access). MadConnect stores and refreshes this token.
3. **Event Schema Preparation** – Event payloads must contain parameters that can be mapped to TikTok’s 2.0 schema.

***

### Configure Connector

1. **Add Platform** → select **TikTok – Events API 2.0**.
2. **Authenticate** → paste or generate the **Access Token**; MadConnect validates it.
3. **Verify** → connector shows **Configured** once token and mapping tests pass.

***

### Event Schema Requirements

| Field          | Required?   | Description                                                                                |
| -------------- | ----------- | ------------------------------------------------------------------------------------------ |
| `event`        | Yes         | Event name e.g. `CompletePayment`, `AddToCart`.                                            |
| `event_time`   | Yes         | ISO 8601 timestamp (`2024‑06‑13T12:34:56Z`).                                               |
| `event_source` | Yes         | `web`, `app`, or `offline`.                                                                |
| `context`      | Yes         | Wrapper for identity: `ip`, `user_agent`, `ttclid`, etc. At least one identifier required. |
| `properties`   | Conditional | Event‑specific data (`value`, `currency`, `content_id`, etc.).                             |

***

For complete field definitions, authentication steps, and deduplication best‑practices, please refer to TikTok’s official documentation: [https://business-api.tiktok.com/portal/docs?id=1771101303285761](https://business-api.tiktok.com/portal/docs?id=1771101303285761)
