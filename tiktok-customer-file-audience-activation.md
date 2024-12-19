# TikTok Customer File – Audience Activation

## **TikTok Customer File – Audience Activation Overview**

MadConnect empowers advertisers to activate their customer data on TikTok, enabling precise targeting through TikTok’s Customer File Audience Activation. This connector allows businesses to upload customer lists to TikTok Ads Manager, utilizing various identifiers to reach specific audience segments effectively.

### **Connector Overview**

* **Connector Type**: Destination
* **Data Type**: Customer File Audience
* **Description**: TikTok Customer File Audience Activation allows advertisers to reach existing customers by uploading customer files with hashed identifiers such as email, phone number, or Mobile Advertising ID (MAID).
* **Supported Actions**: Create /Upload

### **Prerequisites**

1. **Active TikTok Ads Account**
   * Ensure you have an active TikTok Ads account with permissions to manage Customer File Audiences.
2. **OAuth Authentication**
   * Authenticate your TikTok Ads account within MadConnect using OAuth.
3. **Audience File Requirements**
   * Prepare a file containing customer data with hashed or plain identifiers following TikTok's guidelines (details in schema requirements). Ensure files meet the accepted formats and hashing standards.

### **Configure Connector**

1. **Navigate to the My Platforms Section**
   * Go to "My Platforms" in the MadConnect UI.
2. **Add a New Platform**
   * Click on "Add Platform."
3. **Select TikTok Customer File – Audience Activation**
   * Choose the "TikTok Customer File – Audience Activation" tile and click "Configure."
4. **Go to Destination Configuration**
   * Open the "Destination Configuration" tab.
5. **Sign in with TikTok**
   * Click "Sign in with TikTok" and log in using your TikTok Ads credentials.
6. **Authorize MadConnect**
   * Grant MadConnect permission to access and manage your TikTok Customer File audiences.
7. **Verify Configuration**
   * Ensure the platform status is marked as "Configured" under My Platforms.

### **Schema Requirements for TikTok Customer File – Audience Activation**

To upload a customer file to TikTok via MadConnect, ensure the file meets the following format:

#### **Single ID Type Format**

* **Description**: Each row contains a single identifier (email, phone, or MAID) hashed using SHA-256.
* **Sample file with one ID type:**

| 9f5326721cb782c |
| --------------- |
| 9f5326721cb782c |

#### **Multiple ID Types Format**

* **Description**: Each row can contain multiple identifiers—email, phone, and MAID, with emails and phone numbers hashed using SHA-256.
* **Sample file with multiple ID types:**

| email\_SHA256   | phone\_SHA256   | MAID            |
| --------------- | --------------- | --------------- |
| 9f5326721cb782c | 9f5326721cb782c | 9f5326721cb782c |
| 9f5326721cb782c | 9f5326721cb782c | 9f5326721cb782c |

#### **Accepted Hashing Algorithm**

* **Hashing Algorithm**: SHA-256 for emails and phone numbers. MAIDs can be uploaded as plain text.
* **Example (SHA-256 Hash)**:
  * Email: 9f5326721cb782c
  * Phone: 9f5326721cb782c
  * MAID: 9f5326721cb782c

#### Email Specific Normalization

Assuming an email address is generalized to this format: local-part.123+suffix@domain.com (example)

* Remove '+' and any characters following the '+' symbol until '@'. After normalization, the above example will be: local-part.123@domain.com.
* Remove all possible '.' within the username before '@'. After normalization, the above example will be: local-part123@domain.com .
* Email addresses need to be lowercased before hashing.

For additional guidance on TikTok Customer File Audience Activation, refer to the [TikTok Business API documentation on Customer File Audiences](https://business-api.tiktok.com/portal/docs?id=1747012327159809) and [File Upload Requirements](https://business-api.tiktok.com/portal/docs?id=1739940567842818).
