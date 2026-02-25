# Snowflake - Data store

## Snowflake Inc. – Data Store (Destination)

#### **Snowflake Destination Connector – Structured Data Ingestion Overview**

MadConnect supports Snowflake as a **destination data store**, enabling structured datasets to be written directly into a customer-controlled Snowflake environment.

Snowflake can receive:

1. Audience exports
2. Conversion / event datasets
3. Reporting data from ad platforms
4. Enrichment outputs
5. Partner-delivered payloads
6. Custom structured API responses

This connector is configured externally and requires authentication credentials.

***

### **Connector Overview**

| Field              | Description                                                                                       |
| ------------------ | ------------------------------------------------------------------------------------------------- |
| **Connector Type** | Destination                                                                                       |
| **Data Type**      | Structured Tables (Warehouse Storage)                                                             |
| **Description**    | Writes processed datasets into a Snowflake table for storage, analytics, or downstream processing |
| **Write Modes**    | Append / Replace                                                                                  |

***

### **Prerequisites**

Before configuring Snowflake as a destination, ensure:

1. A Snowflake account is provisioned
2. Target **Database** and **Schema** exist
3. A table exists (or role has permission to create one)
4. A Warehouse is available
5. The configured user/role has:
   * USAGE on Database
   * USAGE on Schema
   * INSERT on Table
   * CREATE TABLE (optional, if allowing auto-create)

Example grants:

```
GRANT USAGE ON DATABASE <DATABASE_NAME> TO ROLE <ROLE_NAME>;
GRANT USAGE ON SCHEMA <DATABASE_NAME>.<SCHEMA_NAME> TO ROLE <ROLE_NAME>;
GRANT INSERT ON TABLE <DATABASE_NAME>.<SCHEMA_NAME>.<TABLE_NAME> TO ROLE <ROLE_NAME>;
```

***

## Authentication Configuration (My Connectors → Snowflake Data Store → Configure)

Snowflake supports **two authentication methods**:

***

### **Option 1 – Username & Password**

**Auth Type:** Username and Password

Required fields:

| Field        | Description        |
| ------------ | ------------------ |
| **Username** | Snowflake user     |
| **Password** | Snowflake password |

This method is simple but requires password rotation management.

***

### **Option 2 – Key Pair Authentication (Recommended for Enterprise)**

**Auth Type:** Key-Pair Authentication

Required fields:

| Field                 | Description                                   |
| --------------------- | --------------------------------------------- |
| **Private Key**       | RSA private key (PEM format)                  |
| **Snowflake Account** | Account identifier (e.g. `xy12345.us-east-1`) |
| **Snowflake Host**    | Full account host URL                         |
| **Snowflake User**    | Snowflake user                                |
| **Snowflake Role**    | Role used for execution                       |

Key pair authentication:

* Eliminates password rotation issues
* Aligns with enterprise security standards
* Is preferred for production environments

***

## Connection Configuration (Create Connection Flow)

After the connector is authenticated, the following fields are required when creating a Snowflake destination connection:

| Field                 | Required | Description                       |
| --------------------- | -------- | --------------------------------- |
| **Warehouse**         | Yes      | Compute warehouse used for writes |
| **Database**          | Yes      | Target database                   |
| **Schema**            | Yes      | Target schema                     |
| **Table / View Name** | Yes      | Destination table for data writes |
| **Write Mode**        | Yes      | Append or Replace                 |

These fields define the ingestion target and write behavior.

***

## Common Destination Use Cases

#### **Audience Archival**

Store:

* `segment_id`
* `segment_name`
* Identifier fields
* Action flags
* Transfer metadata

Used for QA, compliance, and reconciliation.

***

#### **Conversion/Event Storage**

Typical fields:

| Field              | Notes             |
| ------------------ | ----------------- |
| `event_time`       | ISO or epoch      |
| `event_name`       | Logical event     |
| Identifier columns | Optional          |
| `properties.*`     | Optional metadata |

***

#### **Reporting & Analytics**

Snowflake can receive:

1. Platform performance reports
2. Match rate summaries
3. API response logs
4. Enrichment outputs

***

## Important Notes

1. Credentials are required
2. Warehouse must be active or auto-resume enabled
3. Replace mode performs a full table truncate before write
4. Role permissions must include INSERT
