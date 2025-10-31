# Amazon S3 - Destination

<div align="left"><figure><img src=".gitbook/assets/image (73).png" alt="" width="290"><figcaption></figcaption></figure></div>

### Amazon S3 – Destination Overview

Amazon Simple Storage Service (Amazon S3) is a secure, highly scalable object-storage platform for storing any amount of data.\
With MadConnect, you can **deliver output data**—including activated audiences, reporting exports, and connector payloads—directly into your own S3 buckets for downstream use in analytics, marketing, or enterprise workflows.

***

#### Connector Overview

| Field                    | Description                                                                                      |
| ------------------------ | ------------------------------------------------------------------------------------------------ |
| **Source / Destination** | Destination                                                                                      |
| **Connector Type**       | File-Based                                                                                       |
| **Data Type**            | CSV / Parquet / GZIP compressed CSV                                                              |
| **Description**          | Write data from MadConnect to your Amazon S3 bucket for long-term storage or further processing. |
| **Supported Actions**    | Put / Upload                                                                                     |

***

#### Authentication

MadConnect uses AWS programmatic-access credentials to write to your S3 bucket.\
You’ll need to supply:

1. **AWS Access Key ID**
2. **AWS Secret Access Key**

These credentials should belong to an **IAM user or role with limited write access** to the specific bucket or prefix.

***

#### Steps to Create IAM Credentials for MadConnect (Write Access)

1. **Log in** to the AWS Management Console
2. **Create an IAM User**
   * Navigate to **IAM › Users › Add Users**
   * Example Username: `MadConnect-S3-Writer`
   * Access type: **Programmatic access**
3. **Attach Permissions**
   * Choose _Attach policies directly › Create Policy_
   *   Select the **JSON** tab and paste:

       ```json
       {
         "Version": "2012-10-17",
         "Statement": [
           {
             "Effect": "Allow",
             "Action": [
               "s3:PutObject",
               "s3:PutObjectAcl",
               "s3:ListBucket"
             ],
             "Resource": [
               "arn:aws:s3:::<BUCKET_NAME>",
               "arn:aws:s3:::<BUCKET_NAME>/*"
             ]
           }
         ]
       }
       ```


   * Save and name the policy (e.g., `MadConnectS3WritePolicy`)
     * Attach the policy to the user and finish creation
4. **Download Credentials**
   * Securely save the `.csv` file containing the Access Key ID and Secret Access Key

***

#### Configuring the Connector in MadConnect

1. **Log in** to MadConnect
2. Navigate to **My Platforms**
3. Locate the **Amazon S3** tile and click **Configure**
4. Enter:
   * Access Key ID
   * Secret Access Key
5. Click **Save Configuration**
6. Verify the platform status is marked **Configured**

***

#### Using S3 as a Destination in Connections

When creating a connection where **Amazon S3 is the destination**:

1. **Specify Output Path**
   * Provide the Bucket URI and prefix where MadConnect should write files.
     * Example (prefix): `s3://my-bucket/madconnect/outputs/`
       * Example (file): `s3://my-bucket/madconnect/outputs/audience_export.csv`
2. **Select File Format**
   * Choose CSV (default) or Parquet as the output type.
3. **Run Transfer**
   * On manual or scheduled runs, MadConnect creates a new object in the defined path.
4. **Verify Delivery**
   * Confirm objects appear in your S3 console with the expected prefix and timestamp.

***

#### Best Practices & Notes

1. Use a dedicated prefix for MadConnect outputs to simplify data management.
2. Ensure the IAM user has `PutObjectAcl` if cross-account access is needed.
3. Large datasets are streamed and chunked automatically by MadConnect.
4. Configure lifecycle rules in S3 to expire or archive old objects if needed.
5. The same credentials can be reused across workspaces unless stricter bucket-level segmentation is required.

***

#### Reference Documentation

* [AWS IAM Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)
* [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
* [MadConnect Platform Configuration Guide](https://docs.madconnect.io)

