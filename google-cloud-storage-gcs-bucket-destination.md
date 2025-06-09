# Google Cloud Storage (GCS) Bucket - Destination

## Google Cloud Storage (GCS) – **Destination** Tile Overview

Push CSV payloads from MadConnect into a designated location within a GCS bucket for downstream analytics, archiving, or hand-off to other systems.

### **Connector Overview**

| Field                  | Value                                                   |
| ---------------------- | ------------------------------------------------------- |
| **Source/Destination** | **Destination**                                         |
| **Connector Type**     | File-Based                                              |
| **Data Type**          | CSV / CSV.GZ                                            |
| **Description**        | Write MadConnect-generated CSV files into a GCS bucket. |
| **Supported Actions**  | **Put**                                                 |

***

### **Authentication**

Requires a **Service Account JSON key** with **write** permissions.

**Create Write-Enabled Service Account**

1. **IAM & Admin → Service Accounts → Create Service Account**
   * _Example Name:_ `madconnect-gcs-dest`
2. **Grant Roles**
   * **Storage Object Creator** (write objects)
   * (Optional) **Storage Object Viewer** for verification/listing.
3. **Create Key → JSON** and download.
4. Limit access at the bucket level when possible.

***

### **Configure Connector in MadConnect**

1. **My Platforms → Add Platform → Google Cloud Storage (Destination)**
2. **Input JSON key**.
3. Save → confirm platform shows **Configured**.

***

### **Writing Files in a Connection**

| Option            | Example                                                          | Notes                                                               |
| ----------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------- |
| **Prefix**        | `gs://analytics-exports/madconnect_out/`                         | MadConnect auto-generates filenames with timestamp + connection ID. |
| **Explicit Path** | `gs://analytics-exports/madconnect_out/daily_users_{{date}}.csv` | Supports template tokens such as `{{date}}` (YYYY-MM-DD).           |

1. **Compression:** Enable “GZIP output” to write `.csv.gz` files.
2. **Overwrite Policy:** If an object name already exists, MadConnect overwrites it (GCS versioning preserves previous versions if enabled).

***

#### **Best Practices**

* **Transactional Loads:** Write to a staging prefix first, then move/rename if your downstream tools require atomic updates.
* **Bucket Versioning:** Enable to keep historical copies of overwritten files.
* **Security & Governance:**
  * Restrict service-account roles to the destination bucket only.
  * Set IAM Conditions to enforce network or time-based restrictions.
* **Cost Control:** Use lifecycle rules to transition old objects to Nearline/Coldline or delete after retention.
