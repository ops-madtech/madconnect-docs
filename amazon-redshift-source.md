# Amazon Redshift - Source

MadConnect connects directly to a client's Amazon Redshift data warehouse, letting you pull data out of a table, a custom SQL query, or an entire large table via Redshift's native UNLOAD-to-S3 fast path — directly into your destination of choice. Use it to extract warehouse data for blending with other sources, migrating it elsewhere, or feeding it into downstream reporting.

## Connector Overview

| Field             | Description                                                                                                                                                         |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Connector Type    | Source                                                                                                                                                              |
| Data Type         | Data Store                                                                                                                                                          |
| Description       | Retrieve rows from a table, a custom SQL query, or a full-table UNLOAD-to-S3 export in a client's Amazon Redshift database                                          |
| Primary Use Case  | Extracting warehouse data into other systems; large-table extraction via the UNLOAD fast path; incremental syncs on tables with a reliably increasing marker column |
| Supported Actions | Read                                                                                                                                                                |

## Prerequisites

### Authentication

This connector authenticates with standard database credentials: hostname, port, database name, username, and password.

For large tables, this connector can also use Redshift's own UNLOAD command to export data to S3 in parallel before reading it — this needs an IAM Role ARN that the Redshift cluster is allowed to assume, plus an S3 staging location.

**AWS cloud only:** IAM-role-based staging access (the default task-role session, or an assumed awsRoleARN) only works when MadConnect's own execution environment runs on AWS.

### Supported Connection Methods

Choose a Connection Method when configuring authentication:

* **Direct** — MadConnect connects straight to the Redshift endpoint. Use this when the cluster or serverless workgroup is reachable from MadConnect's network.
* **SSH Tunnel** — MadConnect connects through an SSH bastion host and tunnels the Redshift connection over it. Use this when Redshift sits in a private network only reachable via a bastion. Requires the bastion's host, port, username, and private key (with an optional passphrase).

MadConnect's execution environment runs inside your own AWS account, so nothing here is a cross-account grant to an outside party. You're setting up IAM permissions between two roles you control: the role MadConnect's own execution environment runs as, and the role Redshift itself assumes when it runs UNLOAD.

### Access Requirements

* A Redshift database user with SELECT access on the target table(s) or schema
* If using an SSH tunnel: network access to the bastion host, and a valid SSH private key
* If using the UNLOAD fast path: an IAM role attached to the Redshift cluster/workgroup with write access to the S3 staging bucket
* The IAM role MadConnect's execution environment runs as needs read access to that same S3 staging bucket, to retrieve the unloaded files
* Outbound network access from MadConnect to the Redshift endpoint (and to the SSH bastion, if used)

### Know These Before Configuring

* Database name, schema, and table name — or the custom SQL query you want to run instead
* Whether you want a one-time Full Load or an Incremental sync, and if incremental, which column to use as the marker/cursor (it must only ever increase, e.g. an auto-incrementing id or an updated\_at timestamp)
* Whether the table is large enough (roughly 10M+ rows) to benefit from the UNLOAD fast path instead of the default read path
* If using UNLOAD: the S3 staging bucket URI and the IAM role ARN Redshift will assume

## Steps to Set Up Redshift Access for MadConnect

**AWS-hosted MadConnect only:** steps 2 and 3 below (the IAM role and the execution-role grant) apply only when MadConnect's own execution environment runs on AWS.

**1. Create an S3 Staging Bucket**

* In AWS, create a new S3 bucket (or choose an existing one) dedicated to UNLOAD staging
* Note the bucket name — you'll enter it as the S3 Staging URI in MadConnect

**2. Create and Attach an IAM Role (AWS-hosted MadConnect only)**

* In IAM, create a role that the Redshift service can assume
* Attach a policy granting s3:PutObject, s3:GetObject, s3:DeleteObject, and s3:ListBucket on the staging bucket
* In the Redshift console, attach this role to the cluster or serverless workgroup under Associated IAM roles
* Copy the role's ARN — this is the IAM Role ARN you'll enter in MadConnect

**3. Grant MadConnect's Execution Role Access to the Staging Bucket (AWS-hosted only)**

* Grant the IAM role MadConnect's execution environment runs as read access (GetObject, ListBucket) to the same staging bucket, so it can retrieve the files Redshift unloads

**4. Create a Database User**

* Create (or identify) a Redshift user with SELECT access on the schema(s) you want to sync
* Share the username and password with MadConnect — or, if the cluster isn't publicly reachable, gather the SSH bastion host/port/username and private key instead

## Setting Up the Connector in MadConnect

**Add Platform**

* Go to **My Platforms** → **Add Platform**
* Select **Amazon Redshift (Source)** and click **Configure**

**Authenticate**

* Enter Hostname, Port, Database, Username, and Password
* If needed, enable **SSH Tunnel** and enter the bastion host, port, username, and private key
* Click **Save** — status shows **Configured**

**Create Connection**

* Choose **Amazon Redshift** as the source
* Choose the data source type: **Table** or **Query**
* Enter the schema and table name, or paste a custom SQL query
* To sync only new/changed rows, enable **Incremental** and choose the marker column
* For large tables, enable the **UNLOAD fast path** and enter the S3 Staging URI and IAM Role ARN
* Choose your destination (S3, Snowflake, BigQuery, another Redshift database, etc.)

**Run or Schedule**

* Run a **Manual Pull** for one-time data retrieval
* Or set up a **Scheduled Sync** for recurring pulls (e.g. daily)

**Review and Save Connection**

* Review the configuration and click **Save**

## Data Dictionary

Sample Output (Table source, e.g. an events table):

| Column      | Example Value       | Notes        |
| ----------- | ------------------- | ------------ |
| id          | 8452                | Whole number |
| updated\_at | 2026-09-18 14:53:25 | Datetime     |
| label       | row\_8452           | Text         |
| amount      | 128.50              | Decimal      |
| region      | AU                  | Text         |

Each column in the source table (or each column returned by your custom query) becomes a properly typed column in your destination — Redshift's own column types map directly (e.g. BIGINT → whole number, TIMESTAMP → datetime, VARCHAR → text, DOUBLE PRECISION/DECIMAL → decimal).

## Additional Considerations

* Large tables: for tables in the tens or hundreds of millions of rows, enable the UNLOAD fast path — Redshift exports the data to S3 in parallel first, which is dramatically faster than reading it row by row
* IAM-role staging access is AWS-only: the default task-role session and the awsRoleARN assume-role option both require MadConnect's own execution environment to run on AWS
* Incremental syncs only pick up rows where the marker column's value increased since the last run — the column must be genuinely monotonic (an id that only grows, or a timestamp that's never backdated)
* Read-only: this connector never writes to or modifies the source Redshift database
* No new data: if no rows match the current marker/incremental window, the run completes successfully with an empty result
* SSH tunnel: adds a small amount of connection setup time per run, but is required whenever the Redshift cluster or workgroup isn't reachable directly
