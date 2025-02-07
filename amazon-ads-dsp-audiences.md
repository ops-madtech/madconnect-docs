# Amazon Ads - DSP Audiences

![](<.gitbook/assets/image (30).png>)

## **Amazon DSP – Advertiser Audiences Connector Overview**

### **Connector Overview**

* **Source / Destination**: Destination
* **Data Type**: Audience
* **Description**: Sync your first-party audience data to Amazon DSP for precise targeting in programmatic advertising campaigns. This connector enables audience creation, audience membership updates, and seamless activation within Amazon DSP and Amazon Marketing Cloud (AMC).
* **Supported Actions**: Create / Add / Remove

> **Note**: If you want to send audiences to both **Amazon DSP & AMC**, use a **connectionId**. If you want to send audiences to **Amazon DSP only**, use an **advertiserId**.

***

### **Prerequisites**

To activate the connector, ensure the following:

1. **Active Amazon DSP Account**
   * Ensure you have an active Amazon DSP account with permissions to manage audiences.
2. **Authentication Requirements**
   * **OAuth Authentication**: Authenticate using your Amazon Ads account credentials within MadConnect.
   * If OAuth is not available, connect using **Client ID** and **Client Secret** obtained from your Amazon Ads Developer account.

***

### **Configure Connector**

To configure the **Amazon DSP – Advertiser Audiences** API connector in MadConnect, follow these steps:

1. **Navigate to the  Platforms Section**
   * In the **MadConnect UI**, go to **"My Platforms"**.
2. **Add a New Platform**
   * Click on **"Add Platform"**.
3. **Select Amazon DSP – Advertiser Audiences**
   * Choose the **"Amazon DSP – Advertiser Audiences"** tile and click **"Configure"**.
4. **Go to Configuration**
   * Navigate to the **"Configuration"** tab.
5. **Sign in with Amazon**
   * Click the **"Sign in with Amazon"** button.
6. **Authenticate with Amazon**
   * You will be redirected to **Amazon’s login page**. Sign in using your Amazon Ads account credentials.
7. **Authorize MadConnect**
   * Grant MadConnect permission to manage audiences in Amazon DSP.
8. **Verify Configuration**
   * Ensure the connector status is marked as **"Configured"** under **My Platforms**.

***

### **Audience Schema Requirements for Amazon DSP – Advertiser Audiences**

To successfully send audience data to Amazon DSP via MadConnect, ensure your data follows the required schema:

#### **ID Field**

* **Field Name**: `hashed_email`, `hashed_phone`, `hashed_address`, `hashed_first_name`, `hashed_last_name`, `hashed_city`, `hashed_state`, `hashed_zip`
* **Data Type**: String (Hashed using SHA-256)
* **Description**: Contains user identifiers that Amazon DSP will use for audience matching.
* **Example**:
  * Email: `c67a544ff3ec2feafb9b22…`
  * Phone: `f0e4c2f76c58916ec258f2…`
  * Zip Code: `3130329451cc782b7dfda79…`

#### **Segment Name Field**

* **Field Name**: `segment_name`
* **Data Type**: String
* **Description**: The name of the audience being created or updated in Amazon DSP.
* **Example**: `Holiday_Shoppers_Q4`

#### **Segment ID Field**

* **Field Name**: `audience_id`
* **Data Type**: String
* **Description**: Unique identifier for the audience segment within Amazon DSP.
* **Example**: `347891`

#### **Country Code Field**

* **Field Name**: `country_code`
* **Data Type**: String (ISO 3166 format)
* **Description**: Indicates the country of the uploaded audience data.
* **Example**: `US`

#### **Action Field**

* **Field Name**: `action`
* **Data Type**: String
* **Description**: Specifies whether to add or remove users from the audience.
* **Accepted Values**: `add`, `remove`
* **Example**: `add`

***

### **Important Notes**

* **All PII data must be hashed using SHA-256** before sending.
* **Country code is highly recommended** to improve audience match rates.
* **Amazon DSP requires at least 2,000 matched users per audience** for activation.
* **Audience updates are processed within 15 minutes** but may take up to **36 hours to appear in DSP**.

For more details on the **Amazon DSP Advertiser Audiences API**, refer to the [Amazon API Documentation](https://advertising.amazon.com/API/docs/en-us/guides/amazon-marketing-cloud/audiences/audience-management-service).

***

Let me know if you need any modifications or additional details! 🚀
