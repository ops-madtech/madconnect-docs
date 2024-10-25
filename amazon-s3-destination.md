# Amazon S3 - Destination

<div align="left">

<figure><img src=".gitbook/assets/image (73).png" alt="" width="290"><figcaption></figcaption></figure>

</div>

## **Amazon S3 - Destination Connector Overview**

MadConnect integrates with **Amazon S3** to securely transfer and store data within your AWS cloud environment. This connector enables seamless exports of processed data, backups of critical information, or sharing data between platforms within your organization.

***

### **Connector Overview**

* **Connector Type:** Destination
* **Data Type:** Cloud Storage
* **Description:** Export and back up data directly to an Amazon S3 bucket.
* **Supported Actions:** Put

***

### **Prerequisites**

To set up Amazon S3 as a destination, the following must be provided:

1. **AWS Account ID**
   * Identifies the AWS account that owns the S3 bucket.
2. **AWS Role ARN**
   * Provide the IAM role ARN with permissions to allow data uploads to the S3 bucket.
3. **AWS External ID**
   * Use the external ID provided by MadConnect to configure secure cross-account access in the AWS IAM role.
4. **Bucket URI**
   * Specify the URI of the destination S3 bucket (e.g., `s3://your-bucket-name`).

***

### **Configure Connector**

1. **Navigate to the My Platforms Section:**
   * In the MadConnect UI, go to **My Platforms**.
2. **Add a New Platform:**
   * Click on **Add Platform** and select **Amazon S3** from the list.
3. **Go to Destination Configuration:**
   * In the **Configuration** tab, enter the required credentials:
     * AWS Account ID
     * AWS Role ARN
     * AWS External ID
     * Bucket URI
4. **Test and Save Configuration:**
   * Verify the connection and save the configuration. Ensure the status is marked as **Configured** under **My Platforms**.

***

### **Usage Example**

Once configured, you can seamlessly push data to the specified S3 bucket for storage or backup purposes. The connector ensures secure data transfers by leveraging IAM roles and cross-account access policies.

For more detailed information on configuring IAM roles and policies for S3 access, refer to the [**AWS S3 Documentation**](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Integrating.Authorizing.IAM.S3CreatePolicy.html).
