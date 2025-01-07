# The Trade Desk – CRM Audience Activation

**The Trade Desk – CRM Audience Activation Overview**

MadConnect integrates with The Trade Desk, enabling advertisers to activate CRM audiences by syncing customer data directly to The Trade Desk. This connector allows businesses to leverage first-party or third-party data segments to create, target, and manage audiences for their advertising campaigns.

### **Connector Overview**

* **Connector Type**: Destination
* **Data Type**: Audience
* **Description**: Activate CRM audiences in The Trade Desk by syncing customer data to target or exclude specific groups within your campaigns.
* **Supported Actions**: Create / Add / Replace

> **Note**: If you are activating audiences with UID2s, use the **First Party Data Connector** instead.

***

### **Prerequisites**

1. **Active The Trade Desk Account**
   * Ensure you have an active The Trade Desk account with permissions to manage CRM audiences.
2. **Contact TTD Representative**
   * Connect with your The Trade Desk representative for any paperwork needed for audience activation.
   * Paperwork includes an **Access Letter** giving MadConnect permission to transfer data and **UID2 Agreement**.
3. **API Credentials**
   * Generate API credentials in The Trade Desk Developer Portal to authenticate and manage integrations (steps provided below).

***

### **Getting The Trade Desk API Credentials**

1. **Log in to The Trade Desk Platform**
   * Use your account credentials to access The Trade Desk Developer Portal.
2. **Generate an API Token**
   * Click on the **User Profile icon** in the top-right corner and select **Manage API Tokens**.
   * Click **Generate Token** to open the dialog box.
   * Provide a descriptive name for the token (e.g., "MadConnect Integration").
   * Set the token lifetime (recommendation: at least 6 months).
   * Click **Generate Token**, copy the displayed token, and store it securely. If you do not save your token, you will need to generate a new one.

***

### **Configure Connector**

1. **Navigate to the My Platforms Section**
   * Go to "My Platforms" in the MadConnect UI.
2. **Add a New Platform**
   * Click on "Add Platform."
3. **Select The Trade Desk – CRM Audience Activation**
   * Choose the "The Trade Desk – CRM Audience Activation" tile and click "Configure."
4. **Go to Destination Configuration**
   * Open the "Destination Configuration" tab.
5. **Enter API Token**
   * Paste the API token generated from The Trade Desk into the **Token** field.
6. **Authorize MadConnect**
   * Grant MadConnect permission to manage your audiences in The Trade Desk.
7. **Save Configuration**
   * Click **Save** to store the credentials and finalize the setup.

***

### **Audience Schema Requirements for The Trade Desk – CRM Audience Activation**

To successfully sync CRM data to The Trade Desk via MadConnect, ensure your data adheres to the following schema requirements:

1. **\<ID> Field**
   * **Field Name**: `Email`, `Email_SHA256`, `Phone`, `Phone_SHA256`, `UID2Token`,`IDL`
   * **Description**: Specifies the type of Personally Identifiable Information (PII) used for audience matching.
   * **Accepted Values**: `Email`, `Email_SHA256`, `Phone`, `Phone_SHA256`, `UID2Token`,`IDL`
   * **Example**: `"PiiType": "Email_SHA256"`
2. **Action Type**
   * **Field Name**: `action`
   * **Description**: Determines how the new data integrates with existing audience data.
   * **Accepted Values**:
     * `Add`: Adds new records to the existing audience.
     * `Replace`: Replaces the existing audience with the new data.
   * **Example**: `add`
3. **TtlInMinutes**
   * **Description**: Time-to-Live (TTL) in minutes, indicating how long the data remains active in the audience segment.
   * **Example**: `129600`  // 90 days

**Important Notes**:

* **Data Normalization and Hashing**: Ensure that email and phone numbers are normalized and hashed using SHA-256 and base64-encoded before uploading. For detailed guidelines, refer to [The Trade Desk's PII Normalization and Hash Encoding](https://partner.thetradedesk.com/v3/portal/data/doc/DataPiiNormalization).

***

For comprehensive details on API parameters and audience management, consult The Trade Desk's [API Reference Documentation](https://partner.thetradedesk.com/v3/portal/data/doc/DataIntegrateCRMData#crm-data-send).
