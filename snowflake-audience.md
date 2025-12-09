# Snowflake - Audience

### **Snowflake Source Connector – Universal Data Transfer Overview**

MadConnect supports Snowflake as a first-class data source for **any data extraction**, including but not limited to:

1. Audience activation
2. Conversion/event pipelines
3. Enrichment datasets
4. Measurement or reporting exports
5. Custom partner-specific payloads

When used inside the **Snowflake Native App**, Snowflake acts as a secure, fully authorized data provider. All reading, validation, and transformation flows execute directly within the customer’s Snowflake environment, and no additional authentication configuration is required.

***

#### **1. Prerequisites**

Before configuring a Snowflake source connection in MadConnect, ensure the following:

#### **Snowflake Access Requirements**

The Snowflake Native App must have read access to the table or view from which data will be extracted.

To grant the required access to the **MADCONNECT** application, run the following commands:

```sql
GRANT USAGE ON DATABASE <DATABASE_NAME> TO APPLICATION MADCONNECT;
GRANT USAGE ON SCHEMA <DATABASE_NAME>.<SCHEMA_NAME> TO APPLICATION MADCONNECT;
GRANT SELECT ON TABLE <DATABASE_NAME>.<SCHEMA_NAME>.<TABLE_NAME> TO APPLICATION MADCONNECT;
```

These grants enable the Snowflake source connector to read data for:

1. Audiences
2. Conversions
3. Reporting/analytics datasets
4. Custom partner integrations
5. Any use case that extracts data from Snowflake

No additional authentication or credential exchange is required because permissions are handled natively within Snowflake.

***

#### **2. How the Snowflake Source Connector Works**

The Snowflake source connector allows MadConnect to read from a **Snowflake table or view** and transport the data to any downstream destination that supports structured ingestion (audience APIs, conversions APIs, S3 buckets, HTTP endpoints, etc.).

Key behaviors:

1. Data is read directly from Snowflake using the app’s authorized roles
2. No keys, tokens, or external authentication
3. Table/view selection defines the input dataset
4. MadConnect applies schema validation, normalization, hashing (when required), and formatting
5. Supports both **full** and **incremental** sync patterns
6. Output formatting is determined by the chosen **destination**

This makes Snowflake a **universal source for all MadConnect connectors**, not only activation.

***

#### **3. Authentication**

#### **Snowflake Native App = Zero Configuration**

No credentials, OAuth, secrets, or token exchange is required.

1. The app is installed inside the customer’s Snowflake account
2. Access is controlled via Snowflake grants (see prerequisites above)
3. Any readable table or view can be used as the source

***

#### **4. Source Configuration Fields (UI Overview)**

When selecting Snowflake as a source, the following fields appear:

| Field               | Required                 | Description                            |
| ------------------- | ------------------------ | -------------------------------------- |
| **Connection Name** | Yes                      | Friendly label for the connection      |
| **Table Name**      | Yes                      | Snowflake table or view                |
| **Sync Mode**       | Yes                      | Full or Incremental                    |
| **Timezone**        | Optional                 | Interprets timestamps                  |
| **Start Time**      | Optional                 | Start time for scheduled syncs         |
| **Sync Frequency**  | Optional                 | How often data is pulled               |
| **Unit**            | Optional                 | Minutes / Hours / Days                 |
| **Marker Column**   | Required for Incremental | Column used to detect new/updated rows |
| **Marker Type**     | Required for Incremental | Datetime or Numeric                    |
| **Marker Position** | Auto-managed             | Last processed marker value            |

These fields define the **extraction behavior**, independent of the downstream use case.

***

#### **5. General Data Handling (Applies to All Use Cases)**

MadConnect performs the following transformations automatically:

**Normalization**

1. Lowercasing of identifier-like strings
2. Trimming whitespace
3. Removing unsupported characters
4. Standardizing timestamps

**Hashing (Destination-Dependent)**

1. Raw identifiers → hashed to **SHA-256** when required
2. Pre-hashed identifiers → validated but not re-hashed
3. Normalization applied before hashing

**Incremental Handling**

1. Uses the marker column to detect new or updated rows
2. Updates marker position after each successful run

***

#### **6. Audience Activation**&#x20;

#### **Most Commonly Supported Audience Identifiers**

| Identifier Type | Raw Field   | Hashed Field       |
| --------------- | ----------- | ------------------ |
| Email           | `email`     | `email_sha256`     |
| Phone           | `phone`     | `phone_sha256`     |
| MAID            | `maid`      | `maid_sha256`      |
| IDFA            | `idfa`      | `idfa_sha256`      |
| External ID     | `extern_id` | `extern_id_sha256` |

**Audience-Specific Behavior**

MadConnect:

1. Normalizes and hashes identifiers if needed
2. Supports multiple identifier types in the same row
3. Supports `action` values (add/remove), depending on the destination

***

#### **7. Conversion/Event Uploads**&#x20;

**Common Conversion Fields**

| Field                                 | Required | Notes                                    |
| ------------------------------------- | -------- | ---------------------------------------- |
| `event_time`                          | Yes      | Must be epoch seconds or valid timestamp |
| `event_name` / destination equivalent | Yes      | Destination-specific                     |
| ID fields                             | Optional | Same handling as audience IDs            |
| `properties.*`                        | Optional | Additional metadata                      |
| `event_number`                        | Optional | Unique event ID                          |

MadConnect performs timestamp, identifier, and payload validation depending on the chosen destination.

***

#### **8. Flexible Data Transfer**

Snowflake can be used to transfer:

1. Reporting/analytics exports
2. Business logic datasets
3. Enrichment tables
4. Custom API payloads
5. Look-alike seeds
6. Offline measurement data
7. Any curated table the customer wants to deliver downstream

Snowflake is treated as a **general-purpose structured data source**.

***

#### **9. Choosing Full vs Incremental Sync**

| Use Case                | Mode        | Reason                       |
| ----------------------- | ----------- | ---------------------------- |
| Large fact tables       | Incremental | Avoid full scans             |
| Event streams           | Incremental | Append-only data             |
| Stable dimension tables | Full        | Simple refresh               |
| Audiences               | Either      | Based on customer preference |
| Custom feeds            | Either      | Destination-defined          |

More information on Full & Incremental transfers can be found [**here**](https://docs.madconnect.io/connections#incremental-loads-and-marker-columns-in-madconnect).&#x20;

***

#### **10. Important Notes**

1. Table or view must be accessible to **MADCONNECT** via Snowflake grants
2. All data remains inside the customer’s Snowflake account until transferred
3. Case sensitivity is handled via MadConnect normalization
4. Marker strategy is critical for large datasets
5. Snowflake→MadConnect→Destination is secure, controlled, and fully auditable
