# Google BigQuery - Source

Use **Google BigQuery** as a source to query and extract data directly from your GCP data warehouse into MadConnect workflows. This connector supports secure, service-account–based authentication and flexible SQL queries, making it ideal for audience activation, conversions, and downstream reporting pipelines.

BigQuery sources are commonly used when customer data already lives in GCP and needs to be activated or shared without exporting files.

***

#### Authentication & Connector Setup

To configure the BigQuery connector, you’ll provide a **Google Cloud service account JSON key** with the appropriate permissions to access the target dataset and tables.

**Required permissions (recommended):**

* BigQuery Data Viewer
* BigQuery Job User

The JSON key is securely stored and used only for executing queries defined in your connection.

***

#### Connector Configuration (Add Source)

When creating the BigQuery connector, you’ll be asked to provide:

* **JSON Key**\
  Paste the full contents of the Google service account JSON key.\
  This enables secure authentication to your GCP project.

***

#### Source Configuration (Connection Setup)

Once the connector is configured, each connection using BigQuery as a source requires the following inputs:

* **Google Project ID**\
  The GCP project that contains your BigQuery dataset.
* **BigQuery Dataset**\
  The dataset where your source table or view lives.
* **BigQuery Table**\
  The primary table or view to query from.
* **SQL Query**\
  A valid BigQuery SQL query used to select and shape the data.\
  This can reference the table directly or include joins, filters, and transformations as needed.

Queries should return only the fields required by the destination schema to optimize performance.

***

#### Common Use Cases

* Activating first-party audiences from BigQuery
* Sending hashed PII for paid media onboarding
* Powering recurring syncs from curated analytical tables
* Using views to abstract complex transformations upstream
