# Snowflake - Audience

## Snowflake Source Connector – Universal Data Transfer Overview

MadConnect supports Snowflake as a first-class data source for any data extraction, including but not limited to:

* Audience activation
* Conversion/event pipelines
* Enrichment datasets
* Measurement or reporting exports
* Custom partner-specific payloads

The Snowflake source connector is configured externally and requires authentication credentials. Once authenticated, any table or view the configured role can read is available as a data source across MadConnect destinations.

***

### Connector Overview

| Field             | Description                                                                                                                                                                                                                            |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Connector Type    | Source                                                                                                                                                                                                                                 |
| Data Type         | Data Store                                                                                                                                                                                                                             |
| Description       | Reads structured data from Snowflake tables or views and delivers it to any supported MadConnect destination. Supports audience activation, conversion pipelines, enrichment datasets, reporting exports, and custom partner payloads. |
| Supported Actions | Read                                                                                                                                                                                                                                   |
| Authentication    | Key-Pair Authentication, Username and Password                                                                                                                                                                                         |
| Data Source Types | Table Reference (DB / Schema / Table), SQL Query                                                                                                                                                                                       |
| Sync Modes        | Full, Incremental                                                                                                                                                                                                                      |

***

### 1. Prerequisites

Before configuring a Snowflake source connection in MadConnect, ensure the following:

**Snowflake Access Requirements**

1. A Snowflake account is provisioned and accessible
2. A Snowflake user and role are available for MadConnect to use
3. The configured role has read access to the target database, schema, and table or view
4. A Snowflake warehouse is available for query execution

**Granting the Required Access**

Regardless of which authentication method is used, the Snowflake Role entered during connector configuration must have read access to the target database, schema, and table or view. The user or key pair authenticates the connection, but all queries execute under the specified role. If the role lacks the required grants, the connector will authenticate successfully but fail when a transfer is initiated.

Run the following commands to grant the required access, replacing `<ROLE_NAME>` with the exact role entered in the Snowflake Role field during connector setup:

```sql
GRANT USAGE ON DATABASE <DATABASE_NAME> TO ROLE <ROLE_NAME>;
GRANT USAGE ON SCHEMA <DATABASE_NAME>.<SCHEMA_NAME> TO ROLE <ROLE_NAME>;
GRANT SELECT ON TABLE <DATABASE_NAME>.<SCHEMA_NAME>.<TABLE_NAME> TO ROLE <ROLE_NAME>;
```

These grants apply to both authentication methods. The role must also be assigned to the Snowflake user specified in the connector configuration, or Snowflake will not allow the role to be activated during the session.

***

### 2. How the Snowflake Source Connector Works

The Snowflake source connector allows MadConnect to read from a Snowflake table or view and transport the data to any downstream destination that supports structured ingestion (audience APIs, conversions APIs, S3 buckets, SFTP endpoints, etc.).

**Key behaviors:**

* Data is read using the credentials and role supplied during connector configuration
* Table or view selection defines the input dataset
* MadConnect applies schema validation, normalization, hashing (when required), and formatting
* Supports both full and incremental sync patterns
* Output formatting is determined by the chosen destination

This makes Snowflake a universal source for all MadConnect connectors.

***

### 3. Authentication

The Snowflake source connector supports two authentication methods. The Auth Type is selected during connector configuration under My Connectors → Snowflake (data store) → Configuration.

#### Option 1 – Username and Password

| Field             | Required | Description                                    |
| ----------------- | -------- | ---------------------------------------------- |
| Auth Type         | Yes      | Select: Username and Password                  |
| Username          | Yes      | Snowflake user login                           |
| Password          | Yes      | Snowflake user password                        |
| Snowflake Account | Yes      | Account identifier (e.g., `xy12345.us-east-1`) |
| Snowflake Role    | Yes      | Role used for query execution and data access  |

This method is straightforward to set up. Password rotation must be managed independently.

#### Option 2 – Key-Pair Authentication

| Field             | Required | Description                                    |
| ----------------- | -------- | ---------------------------------------------- |
| Auth Type         | Yes      | Select: Key-Pair Authentication                |
| Private Key       | Yes      | RSA private key in PEM format                  |
| Snowflake Account | Yes      | Account identifier (e.g., `xy12345.us-east-1`) |
| Snowflake Host    | Yes      | Full account host URL                          |
| Snowflake User    | Yes      | Snowflake user associated with the key pair    |
| Snowflake Role    | Yes      | Role used for query execution and data access  |

Key-pair authentication eliminates password rotation overhead, aligns with enterprise security standards, and is the preferred method for production environments. The private key must not be passphrase-protected.

***

### 4. Source Configuration Fields (Create Connection Flow)

When selecting Snowflake as a source during connection creation, two Data Source Type options are available. The type determines what additional fields are required.

#### Data Source Type: Table Reference (DB / Schema / Table)

Use this option when reading from a specific Snowflake table or view.

| Field              | Required | Description                                    |
| ------------------ | -------- | ---------------------------------------------- |
| Data Source Type   | Yes      | Select: Table Reference (DB / Schema / Table)  |
| Warehouse          | Yes      | Snowflake compute warehouse used for the query |
| Snowflake Database | Yes      | Database containing the source table or view   |
| Snowflake Schema   | Yes      | Schema containing the source table or view     |
| Snowflake Table    | Yes      | Table or view name to read from                |

#### Data Source Type: SQL Query

Use this option when the dataset requires filtering, joining, or transformation at the source before extraction.

| Field            | Required | Description                                    |
| ---------------- | -------- | ---------------------------------------------- |
| Data Source Type | Yes      | Select: SQL Query                              |
| Warehouse        | Yes      | Snowflake compute warehouse used for the query |
| SQL Query        | Yes      | A valid Snowflake SQL SELECT statement         |

The SQL Query option supports any valid `SELECT` statement. The configured role must have access to all tables or views referenced in the query.

#### Additional Run Configuration Fields

The following fields control sync behavior and are configured in the Run Configuration step:

| Field           | Required                 | Description                                         |
| --------------- | ------------------------ | --------------------------------------------------- |
| Sync Mode       | Yes                      | Full or Incremental                                 |
| Timezone        | Optional                 | Interprets timestamp values                         |
| Start Time      | Optional                 | Start time for scheduled syncs                      |
| Sync Frequency  | Optional                 | How often data is pulled                            |
| Unit            | Optional                 | Minutes / Hours / Days                              |
| Marker Column   | Required for Incremental | Column used to detect new or updated rows           |
| Marker Type     | Required for Incremental | Datetime or Numeric                                 |
| Marker Position | Auto-managed             | Last processed marker value, updated after each run |

These fields define the extraction behavior independent of the downstream use case.

***

### 5. General Data Handling&#x20;

MadConnect performs the following transformations automatically:

**Normalization**

* Lowercasing of identifier-like strings
* Trimming whitespace
* Removing unsupported characters
* Standardizing timestamps

**Hashing (Destination-Dependent)**

* Raw identifiers are hashed  when required by the destination
* Pre-hashed identifiers are validated but not re-hashed
* Normalization is applied before hashing

**Incremental Handling**

* Uses the marker column to detect new or updated rows
* Updates marker position after each successful run

***

### 6. Audience Activation

**Most Commonly Supported Audience Identifiers**

| Identifier Type | Raw Field   | Hashed Field       |
| --------------- | ----------- | ------------------ |
| Email           | `email`     | `email_sha256`     |
| Phone           | `phone`     | `phone_sha256`     |
| MAID            | `maid`      | `maid_sha256`      |
| IDFA            | `idfa`      | `idfa_sha256`      |
| External ID     | `extern_id` | `extern_id_sha256` |

**Audience-Specific Behavior**

MadConnect normalizes and hashes identifiers if needed, supports multiple identifier types in the same row, and supports action values (`add`/`remove`) depending on the destination.

***

### 7. Conversion/Event Uploads

**Common Conversion Fields**

| Field                                 | Required | Notes                                            |
| ------------------------------------- | -------- | ------------------------------------------------ |
| `event_time`                          | Yes      | Must be epoch seconds or valid timestamp         |
| `event_name` / destination equivalent | Yes      | Destination-specific                             |
| `user.ids.email_sha256`               | Optional | Hashed email, same normalization as audience IDs |
| `user.ids.phone_sha256`               | Optional | Hashed phone, same normalization as audience IDs |
| `user.ids.maid`                       | Optional | Mobile advertising ID                            |
| `user.ids.extern_id`                  | Optional | External identifier                              |
| `properties.*`                        | Optional | Additional metadata                              |
| `event_id`                            | Optional | Unique event ID                                  |

MadConnect performs timestamp, identifier, and payload validation depending on the chosen destination.

***

### 8. Flexible Data Transfer

Snowflake can be used to transfer:

* Reporting and analytics exports
* Business logic datasets
* Enrichment tables
* Custom API payloads
* Look-alike seeds
* Offline measurement data
* Any curated table the customer wants to deliver downstream

Snowflake is treated as a general-purpose structured data source, not limited to audience activation.

***

### 9. Choosing Full vs Incremental Sync

| Use Case                | Mode        | Reason                       |
| ----------------------- | ----------- | ---------------------------- |
| Large fact tables       | Incremental | Avoid full scans             |
| Event streams           | Incremental | Append-only data             |
| Stable dimension tables | Full        | Simple refresh               |
| Audiences               | Either      | Based on customer preference |
| Custom feeds            | Either      | Destination-defined          |

More information on Full and Incremental transfers can be found in the MadConnect documentation.

***

### 10. Important Notes

1. The configured Snowflake role must have USAGE on the database and schema, and SELECT on the target table or view
2. The warehouse must be active or configured to auto-resume
3. SQL Query mode supports any valid SELECT statement accessible to the configured role
4. Key-pair private keys must not be passphrase-protected
5. Case sensitivity in table and column names is handled via MadConnect normalization
6. Marker strategy is critical for large or high-frequency datasets
