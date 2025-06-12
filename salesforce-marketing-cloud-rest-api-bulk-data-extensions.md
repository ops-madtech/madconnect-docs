# Salesforce Marketing Cloud - REST API Bulk Data Extensions

## **Salesforce Marketing Cloud - Data Extensions Connector Overview**

### **Introduction**

The Salesforce Marketing Cloud - Data Extensions connector allows you to securely sync customer data from your systems into Salesforce Marketing Cloud. Use this connector to populate Data Extensions with contact information for targeted messaging, automation workflows, and campaign personalization.

### **Connector Overview**

| Attribute                | Value                                                                                                                                                                                                         |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Source / Destination** | Destination                                                                                                                                                                                                   |
| **Connector Type**       | Audience                                                                                                                                                                                                      |
| **Data Type**            | Customer Data                                                                                                                                                                                                 |
| **Description**          | Sync customer data to Salesforce Marketing Cloud Data Extensions. This connector allows you to insert customer records into your pre-configured Data Extensions for marketing personalization and automation. |
| **Supported Actions**    | Add                                                                                                                                                                                                           |

***

### **Prerequisites**

Before activating this connector, ensure you have the following:

1. **Active Salesforce Marketing Cloud Account** with access to the REST API.
2. **CustomerKey** of the Data Extension you wish to update. This key identifies the target Data Extension in your Marketing Cloud instance.

You can obtain your CustomerKey by navigating to:

> Email Studio → Subscribers → Data Extensions → \[Your DE] → Properties → External Key (CustomerKey)

***

### **Authentication**

This connector uses **OAuth2 Authorization Code Flow**. During setup:

1. Click **"Sign in with Salesforce"** from the configuration screen.
2. You will be redirected to Salesforce’s login page.
3. Enter your credentials and authorize MadConnect.
4. MadConnect securely retrieves the access token to authenticate future data transfers.

***

### **Configure Connector**

1. **Go to My Platforms**
   * Navigate to the **"My Platforms"** section of the MadConnect UI.
2. **Add New Platform**
   * Select **Salesforce Marketing Cloud - Data Extensions** and click **"Configure."**
3. **Sign in with Salesforce**
   * Use OAuth login to connect securely.
4. **Authorize Access**
   * Grant MadConnect access to manage your Data Extensions.
5. **Verify Configuration**
   * Once validated, your connector will display as **Configured** in the platform.

***

### **Schema Requirements**

Your data **must** match the schema of your target Data Extension in Salesforce. Field names are **case-sensitive** and must align with column names in Salesforce.

#### **Example**

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
