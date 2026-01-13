# Salesforce Marketing Cloud - REST API Bulk Data Extensions

**Salesforce Marketing Cloud - Data Extensions Connector Overview**

### **Introduction**

The Salesforce Marketing Cloud - Data Extensions connector allows you to securely sync customer data from your systems into Salesforce Marketing Cloud. Use this connector to populate Data Extensions with contact information for targeted messaging, automation workflows, and campaign personalization.

### **Connector Overview**

| Attribute                | Value                                                                                                                                                                                                                                                        |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Source / Destination** | Destination                                                                                                                                                                                                                                                  |
| **Connector Type**       | Audience                                                                                                                                                                                                                                                     |
| **Data Type**            | Customer Data                                                                                                                                                                                                                                                |
| **Description**          | Sync customer data to Salesforce Marketing Cloud Data Extensions. This connector allows you to create/update/delete data extension and insert/upsert customer records into your pre-configured Data Extensions for marketing personalization and automation. |
| **Supported Actions**    | Add                                                                                                                                                                                                                                                          |

***

### **Prerequisites**

Before activating this connector, ensure you have the following:

1. **Active Salesforce Marketing Cloud Account** with access to the REST API.
2. Before you connect MadConnect to Salesforce Marketing Cloud (SFMC), you need to c**reate an installed package** in SFMC for MadConnect to use.
3. Locate the **Client ID**, **Client Secret**, **Subdomain**, and **External Key**.



**Creating an Installed Package in SFMC**

1. In SFMC, navigate to **Setup → Apps → Installed Packages** and select **New**.
2. Enter a package name (e.g., “MadConnect Integration”) and choose **Create with enhanced functionality**.
3. Select **Add Component → API Integration → Server-to-Server**.
4. Under **Permissions**, enable at minimum:
   * DataExtensionRead
   * DataExtensionWrite
5. Click **Save**. The **Client ID,** **Client Secret,** and **Subdomain** appear in the component details.
   * Use this information when configuring the connector & connection within MadConnect.
   * The subdomain is the part of the Authentication base URI after `https://` and before `.auth.marketingcloudapis.com/`.&#x20;
     1. For example, if your Authentication base URI is `https://bd5xjh-acgt3456xc7o.auth.marketingcloudapis.com/`, your subdomain is `bd5xjh-acgt3456xc7o`&#x20;



**Locating the an External Key (Required for adding data to an existing data extension)**

1. **Log in to Marketing Cloud** and choose the business unit you want.
2. Open **Contact Builder** from the App Switcher (square grid icon).
3. Click the **Data Extensions** tab in the Contact Builder menu bar.
4. In the left-hand folder tree, click the folder that holds your data extension (or click **Data Extensions** to see everything).
5. In the list on the right, find the **External Key** column—each row shows the key.

<figure><img src=".gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

***

### **Configure Connector**

1. **Go to My Platforms**
   * Navigate to the **"My Platforms"** section of the MadConnect UI.
2. **Add New Platform**
   * Select **Salesforce Marketing Cloud - Data Extensions** and click **"Configure."**
3. **Sign in with Salesforce**
   * **Enter Client ID** and **Client Secret**
   * **Click Save**
4. **Verify Configuration**
   * Once validated, your connector will display as **Configured** in the platform.
5. Enter in Additional Details when creating connection
   * Enter **Subdomain** and **External Key** values during the Create Connection step.&#x20;

***

### Connection Configuration Requirements

<table><thead><tr><th width="192" valign="middle">Field Name</th><th width="116" valign="middle">Option</th><th>Description</th></tr></thead><tbody><tr><td valign="middle">Operation Type</td><td valign="middle">Create </td><td>Creating and uploading data to a new Data Extension. At-least one valid data row is required for successful execution.</td></tr><tr><td valign="middle">Operation Type</td><td valign="middle">Update</td><td>Updating and uploading data to an existing Data Extension. At-least one valid data row is required for successful execution.</td></tr><tr><td valign="middle">Operation Type</td><td valign="middle">Delete</td><td>Deleting an existing Data Extension. No data upload will be performed, but at least one valid data row is required for successful execution.</td></tr><tr><td valign="middle">Operation Type</td><td valign="middle">Add Data</td><td>Uploading data to an existing Data Extension. At-least one valid data row is required for successful execution.</td></tr><tr><td valign="middle">API Type</td><td valign="middle">Insert</td><td>Insert API is used to push records to an existing Data Extension.</td></tr><tr><td valign="middle">API Type</td><td valign="middle">Upsert</td><td>Upsert API is used to push records to an existing Data Extension.</td></tr><tr><td valign="middle">Subdomain</td><td valign="middle">-</td><td>Subdomain of the base URI generated in Pre-requisites step.</td></tr><tr><td valign="middle">Data Extension Name</td><td valign="middle">-</td><td>Name of the Data Extension. Required for Operation Type options - Create and Update. </td></tr><tr><td valign="middle">Data Extension Key</td><td valign="middle">-</td><td>ID of the Data Extension. Required for Operation Type options - Update, Delete and Add Data.</td></tr><tr><td valign="middle">Category ID</td><td valign="middle">-</td><td>ID of the folder which contains the Data Extension. Required for Operation Type options - Create and Update.</td></tr><tr><td valign="middle">Schema Json</td><td valign="middle">-</td><td><p>Fields Json array without the "fields" key. </p><p>Example: </p><p>[<br> {<br>  "description": "The mobile phone number of the contact.",<br>  "isActive": true,<br>  "isHidden": false,<br>  "isInheritable": false,<br>  "isNullable": true,<br>  "isOverridable": false,<br>  "isPrimaryKey": false,<br>  "isReadOnly": false,<br>  "isTemplateField": false,<br>  "length": 50,<br>  "maskType": "None",<br>  "mustOverride": false,<br>  "name": "PhoneNumber",<br>  "storageType": "Plain",<br>  "type": "Text"<br> }<br>]</p></td></tr></tbody></table>

***

### &#x20;**Schema Requirements**

Your data **must** match the schema of your target Data Extension in Salesforce. Field names are **case-sensitive** and must align with column names in Salesforce.

#### **Example Schema**

| Field Name    | Type    | Example                                         |
| ------------- | ------- | ----------------------------------------------- |
| SubscriberKey | Text    | "12345"                                         |
| EmailAddress  | Email   | "[alice@example.com](mailto:alice@example.com)" |
| FirstName     | Text    | "Alice"                                         |
| LastName      | Text    | "Doe"                                           |
| SignupDate    | Date    | "2025-04-30T13:45:00Z"                          |
| IsSubscribed  | Boolean | true                                            |

### **Important** ⚠️

Field names in your dataset **must exactly match** the column names in the target Data Extension, including case sensitivity. Mismatched fields will result in errors or failed record processing.

Only include fields that exist in the Data Extension. All required (non-nullable or primary key) fields must be present.

***

### **Data Matching & Insert Behavior**

* The connector performs **bulk inserts** using your provided payload.
* Records with existing **primary keys** will be rejected unless upsert functionality is enabled.

***

For more information on Salesforce Data Extension APIs, visit the [official documentation](https://developer.salesforce.com/docs/marketing/marketing-cloud/references/mc-data_extension_rows_async?meta=Summary).
