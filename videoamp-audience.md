# VideoAmp - Audience

## VideoAmp Audience Connector

### Connector Overview

| Field                 | Value                                                                                                                                                                                                                                   |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Connector Type**    | Destination                                                                                                                                                                                                                             |
| **Data Type**         | Audience                                                                                                                                                                                                                                |
| **Description**       | Enables delivery of audience data to VideoAmp via Snowflake Direct Share. Connector writes audience data into a Snowflake table, which is shared with VideoAmp. VideoAmp then ingests and processes the audience within their platform. |
| **Supported Actions** | Send                                                                                                                                                                                                                                    |

***

### Prerequisites

* Active **VideoAmp account with API access**
* VideoAmp-provided credentials:
  * Client ID
  * Username (email)
  * Password
  * Account ID
* Snowflake environment configured with:
  * Data share created for VideoAmp
  * Share access granted to VideoAmp
* Snowflake role with permissions to:
  * Create tables
  * Write to tables
  * Grant access to shares
* Table creation must run in a **non-MFA execution context**

***

### Authentication Requirements

#### VideoAmp Account Credentials

Used to configure and authorize access to the VideoAmp platform.

**Required fields:**

* Username
* Password
* Client ID
* Account ID

These are provided by VideoAmp during onboarding

***

#### Snowflake Authentication

Used for writing audience data and enabling VideoAmp access via share.

**Option 1: Username / Password**

```
"user": "<username>"
"password": "<password>"
"account": "<account_identifier>"
"snowflakeRole": "<role>"
```

**Option 2: Key Pair Authentication**

```
"account": "<account_identifier>"
"snowflakeUser": "<username>"
"snowflakeRole": "<role>"
"snowflakePrivateKey": "<private_key>"
"snowflakeHost": "<hostname>"
```

***

### Configuration Fields

#### Required Fields

| Field                   | Description                        |
| ----------------------- | ---------------------------------- |
| **Snowflake Warehouse** | Warehouse used for processing      |
| **Snowflake Database**  | Database containing audience table |
| **Snowflake Schema**    | Schema for audience table          |
| **Snowflake Table**     | Table used for VideoAmp ingestion  |
| **Snowflake Share**     | Share granted to VideoAmp          |
| **Currency of Record**  |                                    |

***

#### Optional Fields

| Field                                  | Description                                |
| -------------------------------------- | ------------------------------------------ |
| **Permissioned Agency Advertiser IDs** | Advertisers allowed to access audience     |
| **Permissioned Agency IDs**            | Agencies allowed to access audience        |
| **Validate Only**                      | Validate request without creating audience |

***

### Data Schema

If the target table does not exist, MadConnect will create the following schema:

```
CREATE TABLE VIDEOAMP_AUDIENCE_TABLE (
    AUDIENCE_ID STRING,
    AUDIENCE_NAME STRING,
    OPERATION_ID STRING,
    OPEN_ID STRING,
    BLOCKGRAPH_ID STRING,
    CIRCANA_SYNTHETIC_ID STRING,
    LIVE_RAMP STRING,
    TU_HH_ID STRING,
    TU_IND_ID STRING,
    DEVICE_ID STRING,
    DEVICE_ID_WITH_TYPE STRING,
    IP STRING,
    SHA256_EMAIL STRING,
    SHA256_IP STRING,
    CREATED_AT TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

**Key Notes:**

* At least **one identifier column must be populated**
* Supported identifiers include:
  * SHA256 email
  * Device IDs
  * IP
  * LiveRamp IDs
  * TransUnion IDs
* Multiple identifier types can exist in the table
* This table serves as a **staging layer for VideoAmp ingestion**

***

### Running a Transfer in MadConnect

1. Navigate to **Create Connection**
2. Select a **Source** (e.g., Snowflake, S3, etc.)
3. Select **VideoAmp** as the **Destination**
4. Configure:
   * Snowflake connection details
   * Table and share
   * Currency of record
5. Map fields to VideoAmp schema (if needed)
6. Run or schedule the transfer

**What happens next:**

* Data is written to the Snowflake table
* The table is shared with VideoAmp
* VideoAmp reads the data and processes audience creation asynchronously

***

### Important Notes

* This connector uses a **Snowflake-based integration (no direct API push)**
* VideoAmp supports:
  * Snowflake Direct Share
  * AWS S3 (not used in this flow)
* Audience creation is:
  * **Asynchronous**
  * Triggered by VideoAmp after data is available
* Each audience is defined by:
  * `AUDIENCE_ID` and/or `AUDIENCE_NAME`
* Ensure identifiers are:
  * Properly formatted
  * Hashed where required (e.g., SHA256 email)
* Multiple audiences can be supported within the same table

***

### Additional Resources

* VideoAmp API Documentation\
  [https://docs.videoamp.dev/](https://docs.videoamp.dev/)
* VideoAmp Audience API\
  [https://api.videoamp.dev/v1/audiences](https://api.videoamp.dev/v1/audiences)
* Internal Setup Guide\
  VideoAmp Destination Connector – Customer Setup Guide
