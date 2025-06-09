# Azure Blob Storage – Source

### Azure Blob Storage – **Source** Tile Overview

Azure Blob Storage is Microsoft Azure’s massively scalable object store for unstructured data. Use MadConnect to ingest CSV (or GZIP-compressed CSV) objects from a container into your pipelines.

#### **Connector Overview**

| Field                  | Value                                                                               |
| ---------------------- | ----------------------------------------------------------------------------------- |
| **Source/Destination** | **Source**                                                                          |
| **Connector Type**     | File-Based                                                                          |
| **Data Type**          | CSV / CSV.GZ                                                                        |
| **Description**        | Read files stored in an Azure Blob Storage container and load them into MadConnect. |
| **Supported Actions**  | **Get**                                                                             |

***

#### **Authentication**

MadConnect supports two secure methods (choose one):

| Method                                | When to Use                                                           | Permissions Needed                                                        |
| ------------------------------------- | --------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| **Shared Access Signature (SAS) URL** | Preferred—granular, time-bound, no key rotation needed in MadConnect. | _Read_ (`r`) and _List_ (`l`) on the target container.                    |
| **Storage Account Access Key**        | For long-lived service accounts or when SAS isn’t feasible.           | Assign built-in **Storage Blob Data Reader** role at the container scope. |

**Create a Read-Only SAS Token (portal)**

1. **Storage Accounts →&#x20;**_**account**_**&#x20;→ Containers →&#x20;**_**container**_**&#x20;→ … → Generate SAS**.
2. **Allowed permissions:** Check **Read** and **List** _(optionally **Read ACL** for ADLS Gen2)_.
3. **Expiry:** Set a reasonable window (e.g., 1 year or rotate quarterly).
4. **Copy the full SAS URL** (`https://<account>.blob.core.windows.net/<container>?sv=...&sig=...`).

***

#### **Configure Connector in MadConnect**

1. **My Platforms → Add Platform → Azure Blob Storage (Source)**
2. **Choose Credential Type**
   * _SAS URL_: paste the container-level SAS.
   * _Account Key_: enter **Account Name** + **Access Key**.
3. **Save** → ensure the platform shows **Configured**.

***

#### **URI Format in Connections**

Azure doesn’t use an `s3://`-style scheme. Supply the standard HTTPS path:

| Scenario          | Example URI                                                              | Behaviour                                                              |
| ----------------- | ------------------------------------------------------------------------ | ---------------------------------------------------------------------- |
| **Folder Prefix** | `https://datalake.blob.core.windows.net/exports/`                        | MadConnect lists top-level `.csv` / `.csv.gz` objects (non-recursive). |
| **Single File**   | `https://datalake.blob.core.windows.net/exports/users_2025-06-01.csv.gz` | Fetches only that blob.                                                |

#### **Best Practices**

* **Least Privilege:** Prefer SAS over account keys. Scope it to the container; rotate regularly.
* **Lifecycle Management:** Configure rules to move aged data to Cool/Archive tiers.
* **Performance:** If pulling large files, use the same Azure region as your MadConnect runtime for minimal egress latency.
* **Security:** Enable **Private Endpoints** or **Service Endpoints** to keep traffic on Azure’s backbone when possible.
