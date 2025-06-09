# Azure Blob Storage – Destination

### Azure Blob Storage – **Destination** Tile Overview

Write CSV outputs from MadConnect to an Azure Blob Storage container for downstream analytics, archival, or cross-cloud hand-off.

#### **Connector Overview**

| Field                  | Value                                                                  |
| ---------------------- | ---------------------------------------------------------------------- |
| **Source/Destination** | **Destination**                                                        |
| **Connector Type**     | File-Based                                                             |
| **Data Type**          | CSV / CSV.GZ                                                           |
| **Description**        | Write MadConnect-generated files into an Azure Blob Storage container. |
| **Supported Actions**  | **Put**                                                                |

***

#### **Authentication**

| Method                         | When to Use                                               | Permissions Needed                                                        |
| ------------------------------ | --------------------------------------------------------- | ------------------------------------------------------------------------- |
| **SAS URL**                    | Recommended for bucket-level writes with built-in expiry. | _Create_ (`c`), _Write_ (`w`), _Add_ (`a`), and _List_ (`l`) permissions. |
| **Storage Account Access Key** | For permanent integrations.                               | **Storage Blob Data Contributor** role at the container scope.            |

**Create a Write-Enabled SAS Token**

1. **Generate SAS** (same path as above).
2. **Permissions:** `c` `w` `a` `l` (and `r` if post-write verification is needed).
3. Copy the full SAS URL.

***

#### **Configure Connector in MadConnect**

1. **My Platforms → Add Platform → Azure Blob Storage (Destination)**
2. Provide **SAS URL** **or** **Account Name + Access Key**.
3. **Default Path / Prefix** – e.g., `https://analytics.blob.core.windows.net/madconnect/`.
4. Save → status should read **Configured**.

***

#### **Writing Files in a Connection**

| Option                   | Example                                                                       | Notes                                                                |
| ------------------------ | ----------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| **Prefix (recommended)** | `https://analytics.blob.core.windows.net/madconnect/`                         | MadConnect auto-generates file names with timestamp + connection ID. |
| **Explicit Blob Name**   | `https://analytics.blob.core.windows.net/madconnect/daily_users_{{date}}.csv` | Supports placeholders such as `{{date}}` (YYYY-MM-DD).               |

* **Compression:** Enable “GZIP output” to upload `.csv.gz`.
* **Overwrite Policy:** Existing blobs with the same name are overwritten (versioning retained if **Blob Versioning** is enabled).

***

#### **Best Practices**

* **Tiering & Costs:** Store long-term data in **Cool** or **Archive** to reduce cost.
* **Atomic Loads:** Write to a _staging_ prefix, then copy/rename to a final location when complete.
* **Secure Transfer:** Enforce **HTTPS only** and, if feasible, use **Private Endpoints**.
* **Governance:** Turn on **Blob Soft Delete** or **Versioning** to guard against accidental overwrites or deletions.
