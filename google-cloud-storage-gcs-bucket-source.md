# Google Cloud Storage (GCS) Bucket - Source

## Google Cloud Storage (GCS) – **Source** Tile Overview

Google Cloud Storage (GCS) is GCP’s durable, infinitely scalable object store. Use MadConnect to ingest CSV (or GZIP-compressed CSV) files from a bucket into your data pipelines.

### **Connector Overview**

| Field                  | Value                                                               |
| ---------------------- | ------------------------------------------------------------------- |
| **Source/Destination** | **Source**                                                          |
| **Connector Type**     | File-Based                                                          |
| **Data Type**          | CSV / CSV.GZ                                                        |
| **Description**        | Extract files stored in a GCS bucket and load them into MadConnect. |
| **Supported Actions**  | **Get**                                                             |

### **Authentication**

MadConnect authenticates with a **Service Account JSON key** that has **read-only** scope on the bucket.

**Create Read-Only Service Account**

1. **GCP Console → IAM & Admin → Service Accounts → Create Service Account**
   * _Example Name:_ `madconnect-gcs-source`
2. **Grant Roles**
   * **Storage Object Viewer** (project-wide _or_ bucket-level)
3. **Create Key → JSON** and download the file.
4. (Optional) **Restrict to a single bucket**
   * Storage → Buckets → _bucket name_ → Permissions → **Add principal** (service-account email) → Role **Storage Object Viewer**.

***

### **Configure Connector in MadConnect**

1. **My Platforms → Add Platform → Google Cloud Storage (Source)**
2. **Upload JSON key** or paste its contents.
3. Save → ensure platform status shows **Configured**.

***

### **Using a GCS URI in a Connection**

| Scenario          | URI Example                                      | Behavior                                                       |
| ----------------- | ------------------------------------------------ | -------------------------------------------------------------- |
| **Folder Prefix** | `gs://data-lake/exports/`                        | Loads every `.csv`/`.csv.gz` at the top level (non-recursive). |
| **Single File**   | `gs://data-lake/exports/users_2025-04-01.csv.gz` | Retrieves only that object.                                    |

1. MadConnect ignores sub-folders under a prefix.
2. File names must end in `.csv` or `.csv.gz`.

***

#### **Best Practices**

* **Regional Buckets:** Place buckets in the same GCP region as downstream workloads to lower latency and egress fees.
* **Lifecycle Policies:** Create rules to transition or delete aged files automatically.
* **Security:** Rotate keys regularly or adopt **Workload Identity Federation** to eliminate long-lived keys.
