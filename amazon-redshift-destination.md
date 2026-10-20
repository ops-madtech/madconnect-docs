# Amazon Redshift - Destination

MadConnect loads data from any of its sources into a client's Amazon Redshift data warehouse. Data is staged to S3 and loaded with Redshift's native COPY command, so even very large volumes load quickly in a single parallel operation. The target table is created automatically if it doesn't already exist.

## Connector Overview

| Field             | Description                                                                                |
| ----------------- | ------------------------------------------------------------------------------------------ |
| Connector Type    | Destination                                                                                |
| Data Type         | Data Store                                                                                 |
| Description       | Load data into a table in a client's Amazon Redshift database via a staged S3 COPY         |
| Primary Use Case  | Centralizing data from other platforms and sources into a client's Redshift data warehouse |
| Supported Actions | Write (Append or Delsert)                                                                  |

## Prerequisites

### Authentication

This connector authenticates with standard database credentials: hostname, port, database name, username, and password.

Loading also needs an S3 staging location that MadConnect can write to, and an IAM Role ARN that Redshift assumes to read the staged files back out during COPY.

**AWS cloud only:** IAM-role-based staging access (the default task-role session, or an assumed awsRoleARN) only works when MadConnect's own execution environment runs on AWS.

### Supported Connection Types

The network path between MadConnect and your Redshift cluster/workgroup generally falls into one of three topologies:

**1. Same VPC Connection**

* Best for: Applications running in AWS (ECS, EC2, Lambda)
* How it works: MadConnect connects directly to Redshift through a private VPC endpoint.
* Required Credentials: Host (VPC Endpoint DNS, e.g. `vpce-xxxxx.vpce-svc-xxxxx.us-west-2.vpce.amazonaws.com`), Port (5439), Database Name, Username, Password, SSL Mode (Required) \[confirm: credentials here are listed as "Admin username/password" in the destination draft — confirm whether this should instead reference a database user scoped to CREATE TABLE + INSERT/DELETE only, consistent with the Access Requirements below]
* Setup Guide: [Managing Redshift cluster cross-VPC access](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-cluster-cross-vpc.html)

**2. SSH Bastion Host**

* Best for: Remote access from laptops or office networks
* How it works: SSH tunnel through a bastion host to access Redshift securely.
* Required Credentials: Bastion Host IP, SSH Key (.pem or .ssh private key file), Redshift Host, Port (5439), Database Name, Username, Password \[confirm: same least-privilege question as above applies to this credential set]
* Setup Guide: [Connect to your Linux instance through a bastion host](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/tutorial-connect-bastion-host.html)

**3. VPC Peering**

* Best for: Applications in a different VPC or AWS account
* How it works: Creates a network connection between two VPCs.
* Required Credentials: Host (Redshift endpoint, e.g. `workgroup.123456.us-west-2.redshift-serverless.amazonaws.com`), Port (5439), Database Name, Username, Password, SSL Mode (Required) \[confirm: same least-privilege question as above applies to this credential set]
* Setup Guide: [What is VPC peering?](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html)

All three paths use port 5439 and should run over SSL.

**Legacy connection methods:** Earlier configurations also support two simpler methods, still available when configuring authentication:

* **Direct** — MadConnect connects straight to the Redshift endpoint. Use this when the cluster or serverless workgroup is reachable from MadConnect's network.
* **SSH Tunnel** — MadConnect connects through an SSH bastion host and tunnels the Redshift connection over it. Requires the bastion's host, port, username, and private key (with an optional passphrase).

MadConnect's execution environment runs inside your own AWS account, so nothing here is a cross-account grant to an outside party. You're setting up IAM permissions between two roles you control: the role MadConnect's own execution environment runs as, and the role Redshift itself assumes when it runs COPY.

### Access Requirements

* A Redshift database user with CREATE TABLE and INSERT/DELETE privileges on the target schema
* The IAM role MadConnect's execution environment runs as needs write access (PutObject) to the S3 staging bucket
* The IAM role attached to the Redshift cluster/workgroup needs read access (GetObject, ListBucket) to that same staging bucket
* Outbound network access from MadConnect to the Redshift endpoint (and to the SSH bastion, if used)

### Know These Before Configuring

* Database name, schema, and target table name — the table is created automatically if it doesn't exist, matching the incoming data's structure
* Update Mode: **Append** (always add new rows) or **Delsert** (delete existing rows in a date/datetime window, then insert the new batch for that window)
* If using Delsert: which column to key the delete window on, its type (Date, DateTime, or Epoch), and the window's start and end
* The S3 staging bucket URI and IAM Role ARN Redshift will assume during COPY

## S3 Integration (Required)

Unlike the Source connector, S3 staging is required for the Destination connector — every load is staged to S3 before Redshift's COPY command runs.

**Required Information:**

* S3 Bucket Name: S3 bucket for data staging
* Redshift IAM Role ARN: role that allows Redshift to read the staged files from S3 during COPY

**Example:**

```
S3 Bucket: my-company-redshift-data
Redshift IAM Role: arn:aws:iam::123456789012:role/RedshiftS3AccessRole
```

**Note:** Both Redshift and MadConnect's execution environment need S3 access:

* Redshift uses its IAM role to read the staged files during COPY
* MadConnect's execution environment uses the role it runs as to write files to S3 \[confirm: the draft also named an internal "Application IAM Role" with an example internal role-name pattern — removed here as an internal implementation detail; confirm no customer-facing role name/ARN needs to be published in its place]

Setup Guide: [Creating an IAM role for Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/dg/c-getting-started-using-spectrum-create-role.html)

## Official AWS Documentation

**Redshift Serverless**

* [Main Documentation](https://docs.aws.amazon.com/redshift/latest/mgmt/serverless.html)
* [Getting Started](https://docs.aws.amazon.com/redshift/latest/gsg/new-user-serverless.html)
* [Connecting to Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/connecting-to-cluster.html)

**Security & Networking**

* [VPC Endpoints](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-cluster-cross-vpc.html)
* [Security Groups](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-security-groups.html)
* [IAM Roles](https://docs.aws.amazon.com/redshift/latest/dg/c-getting-started-using-spectrum-create-role.html)

**Data Loading**

* [COPY Command](https://docs.aws.amazon.com/redshift/latest/dg/r_COPY.html)
* [UNLOAD Command](https://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html)
* [Loading Data from S3](https://docs.aws.amazon.com/redshift/latest/dg/t_Loading_tables_with_the_COPY_command.html)

## Quick Setup Steps

**Step 1: Create a Redshift Cluster**

1. Go to AWS Console → Redshift Serverless
2. Create a namespace (database)
3. Create a workgroup (compute)
4. Save your credentials

Official Guide: [Getting started with Redshift Serverless](https://docs.aws.amazon.com/redshift/latest/gsg/new-user-serverless.html)

**Step 2: Configure Network Access**

Choose your connection method:

* Same VPC: Update security groups
* Bastion: Create an EC2 bastion and SSH tunnel
* VPC Peering: Create a peering connection

**Step 3: Connect**

Use the credentials from Step 1 and the connection details from Step 2.

**Step 4: Set Up S3 Integration**

1. Create an S3 bucket
2. Create an IAM role
3. Attach the role to Redshift

Official Guide: [Creating an IAM role for Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/dg/c-getting-started-using-spectrum-create-role.html)

## Steps to Set Up Redshift Access for MadConnect

**AWS-hosted MadConnect only:** steps 2 and 3 below (the IAM role and the execution-role grant) apply only when MadConnect's own execution environment runs on AWS.

**1. Create an S3 Staging Bucket**

* In AWS, create a new S3 bucket (or choose an existing one) dedicated to load staging
* Note the bucket name — you'll enter it as the S3 Staging URI in MadConnect

**2. Create and Attach an IAM Role (AWS-hosted MadConnect only)**

* In IAM, create a role that the Redshift service can assume
* Attach a policy granting s3:GetObject and s3:ListBucket on the staging bucket, so COPY can read the staged files
* In the Redshift console, attach this role to the cluster or serverless workgroup under Associated IAM roles
* Copy the role's ARN — this is the IAM Role ARN you'll enter in MadConnect

**3. Grant MadConnect's Execution Role Write Access to the Staging Bucket (AWS-hosted MadConnect only)**

* Grant the IAM role MadConnect's execution environment runs as write access (PutObject, AbortMultipartUpload) to the same staging bucket, so it can upload files for Redshift to load

**4. Create a Database User**

* Create (or identify) a Redshift user with CREATE TABLE and INSERT/DELETE privileges on the target schema
* Share the username and password with MadConnect — or, if the cluster isn't publicly reachable, gather the SSH bastion host/port/username and private key instead

## Setting Up the Connector in MadConnect

**Add Platform**

* Go to **My Platforms** → **Add Platform**
* Select **Amazon Redshift (Destination)** and click **Configure**

**Authenticate**

* Enter Hostname, Port, Database, Username, and Password
* If needed, enable **SSH Tunnel** and enter the bastion host, port, username, and private key
* Click **Save** — status shows **Configured**

**Create Connection**

* Choose your source
* Choose **Amazon Redshift** as the destination
* Enter the target Database, Schema, and Table
* Choose the **Update Mode**: **Append** or **Delsert** (with window column, type, and dates, if Delsert)
* Enter the S3 Staging URI and IAM Role ARN

**Run or Schedule**

* Run a **Manual Pull** for one-time loading
* Or set up a **Scheduled Sync** for recurring loads (e.g. daily)

**Review and Save Connection**

* Review the configuration and click **Save**

## Data Dictionary

Sample Loaded Row:

| Column      | Example Value       | Notes                               |
| ----------- | ------------------- | ----------------------------------- |
| id          | 8452                | Whole number → Redshift BIGINT      |
| updated\_at | 2026-09-18 14:53:25 | Datetime → Redshift TIMESTAMP       |
| label       | row\_8452           | Text → Redshift VARCHAR(65535)      |
| amount      | 128.50              | Decimal → Redshift DOUBLE PRECISION |
| region      | AU                  | Text → Redshift VARCHAR(65535)      |

If the target table doesn't already exist, MadConnect creates it automatically, mapping each incoming column to a matching Redshift type.

## Additional Considerations

* Auto-create: if the target table doesn't exist, MadConnect creates it automatically based on the incoming data's structure
* IAM-role staging access is AWS-only: the default task-role session and the awsRoleARN assume-role option both require MadConnect's own execution environment to run on AWS
* Update Mode – Append: every run's data is added as new rows; no deduplication is performed
* Update Mode – Delsert: existing rows in the configured date/datetime window are deleted before the new batch is inserted — use this to safely reprocess a period without creating duplicates
* Large loads: files are staged to S3 and loaded with Redshift's native COPY command, which loads every staged file for a run in a single parallel operation regardless of total size
* Write-only: this connector only adds or replaces data in the target table; it never reads from or modifies any other table
