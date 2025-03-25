# Amazon - S3

<div align="left"><figure><img src=".gitbook/assets/image (73).png" alt="" width="290"><figcaption></figcaption></figure></div>

Amazon Simple Storage Service (Amazon S3) is a highly scalable object storage service that allows organizations to store and retrieve any amount of data. With support for secure access controls and various storage classes, S3 enables efficient data lakes, cloud-native applications, and enterprise workflows. Use MadConnect to extract CSV files from your S3 buckets for seamless integration with your data and activation pipelines.

### **Connector Overview**

* **Source/Destination**: Source
* **Connector Type**: File-Based
* **Data Type**: CSV
* **Description**: Extract CSV or compressed CSV (.csv.gz) files stored in Amazon S3 buckets and ingest them into MadConnect.
* **Supported Actions**: Get

### **Authentication**

MadConnect uses programmatic access credentials to connect to your S3 bucket. You will need to provide:

* **AWS Access Key**
* **AWS Secret Key**

These credentials must be generated from an IAM user with limited read-only access to the specific bucket.

#### **Steps to Create IAM Credentials for MadConnect**

1. **Log in to the AWS Management Console**
2. **Create an IAM User**
   * Go to **IAM > Users > Add Users**
   * Username: `MadConnect-S3-ReadOnlyUser`
   * Access type: Select **Programmatic access**
3. **Attach Permissions**
   * Select **Attach policies directly** > **Create Policy**
   * Choose the **JSON tab** and paste the following:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::<BUCKET_NAME>"
    },
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::<BUCKET_NAME>/*"
    }
  ]
}
```

* Save and name the policy (e.g., `MadConnectS3ReadOnlyPolicy`)
* Attach it to the user and complete user creation

4. **Download Credentials**
   * Securely download the `.csv` file containing the **Access Key ID** and **Secret Access Key**

### **Configure Connector**

1. **Log in to MadConnect**
2. **Navigate to My Platforms**
3. **Locate and Click on the Amazon S3 Tile**
4. **Enter Configuration Details**:
   * **Access Key**
   * **Secret Key**
5. **Save Configuration**
6. **Verify Platform is Marked as Configured**

### **Using S3 Bucket URI in a Connection**

When setting up a connection with Amazon S3 as the source:

* Enter the **Bucket URI** where your files are located.
  * This can be a **folder prefix** (e.g., `s3://my-bucket/data/input/`) or a **specific file** path (e.g., `s3://my-bucket/data/input/file1.csv`).
* When initiating a manual transfer:
  * If a **folder prefix** is specified, all **first-level** `.csv` and `.csv.gz` files in that folder will be fetched.
  * Subfolders under the prefix are **not scanned recursively**.
  * If a **specific file** is provided, only that file will be pulled.

### **Additional Notes**

* Ensure file schemas are consistent across all files in the target prefix.
* Reuse the same IAM credentials for all accounts unless stricter bucket-level access is required.

For any questions or troubleshooting, contact your MadConnect representative.

