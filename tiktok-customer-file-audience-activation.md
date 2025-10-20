# TikTok Customer File – Audience Activation

## TikTok Customer File – Audience Activation&#x20;

#### Getting Started & Prerequisites

MadConnect enables advertisers to activate first-party customer data directly within **TikTok Ads Manager** through **Customer File Audiences**. This integration allows marketers to securely upload hashed customer identifiers (emails, phone numbers, MAIDs) to create or update TikTok audiences for more personalized ad targeting.

***

### Connector Overview

* **Connector Type**: Destination
* **Data Type**: Customer File Audience
* **Description**: TikTok Customer File Audience Activation allows advertisers to reach existing customers by uploading customer files with hashed identifiers such as email, phone number, or Mobile Advertising ID (MAID).
* **Supported Actions**: Create / Add / Remove

***

### Key Platform Requirements

1. **TikTok Audience Minimums:**
   * A TikTok Custom Audience must contain at least 1,000 matched users before it can be used in an ad group.
2. **EU Policy Compliance:**
   * TikTok requires advertisers targeting EU users to confirm compliance with their Politics, Governments, and Elections Advertising Policy by January 10, 2026.
   * Failure to confirm may restrict campaign management via API.\
     **Complete this confirmation within TikTok Ads Manager under Account Settings → Compliance.**\


***

### Prerequisites

Before configuring the TikTok connector in MadConnect, ensure the following:

1. **TikTok Ads Account Access**
   * Authenticate with a TikTok Ads Business Manager user who has sufficient privileges to manage Customer File Audiences.
   * You’ll be prompted to log in through TikTok’s OAuth screen during setup.
2. **Audience Schema Alignment**
   * Verify your audience data aligns with MadConnect’s Standard Audience Schema.
   * The following fields are required for TikTok uploads:\


| user\_id <**email\_sha256**, **phone\_sha256**, **maid**> | d7984b9599199b83cc213f19cb2906d2 | Hashed user identifiers used for audience matching.                            |
| --------------------------------------------------------- | -------------------------------- | ------------------------------------------------------------------------------ |
| **segment\_id**                                           | 987654321                        | TikTok-assigned ID for an existing audience. Used for add/remove operations.   |
| **segment\_name**                                         | Holiday Campaign Audience        | Used to create new TikTok audiences if no segment\_id is provided.             |
| **action**                                                | add / remove                     | Indicates whether to add or remove users. Use 'add' when creating an audience. |

\


3. **Hashing Rules:**

* Use SHA-256 for emails and phone numbers.
* MAIDs can be uploaded plain or SHA-256 hashed.
* Data must be normalized before hashing:
  * Email: lowercase, trim spaces, remove “+” and subsequent characters before “@”, and remove “.” from username.
  * Phone: digits only, include country code, remove leading zeros.\
    \


4. **Advertiser ID:**
   * You’ll need the **TikTok Advertiser ID** for the account you want to connect to.
   * Locate this by:
     * Logging in to TikTok Ads Manager.
       * Checking the URL when viewing your advertiser:
         * Example → https://ads.tiktok.com/i18n/nb\_creation/create/objectives?aadvid=**XXXXXXXXXXXXXXXXXXX**
         * Or selecting your advertiser name in the top right corner and noting the value after **ID**:

<figure><img src=".gitbook/assets/unknown (2).png" alt=""><figcaption></figcaption></figure>



5. **Active TikTok Business Account:**
   * Ensure the account is active, verified, and not under review or restricted for API use.

***

### Getting Started in MadConnect

#### Step 1: Add TikTok as a Destination

1. Navigate to **My Platforms** → **Add Platform**.
2. Select **TikTok Customer File – Audience Activation** and click **Configure**.

#### Step 2: Authenticate

1. Click **Sign in with TikTok**.
2. Log in with your TikTok Ads credentials and grant MadConnect access to manage audiences.

#### Step 3: Configure Your Connection

1. Provide your **Advertiser ID**.
2. Validate that the connection shows as **Configured** under **My Platforms → TikTok**.

#### Step 4: Schedule or Transfer

1. Choose **Manual** Transfer to run a one-time load, or **Scheduled** Transfer for recurring audience syncs.
2. MadConnect will hash, validate, and upload only compliant records to TikTok’s API.\
   \


***

### Important Notes

* TikTok enforces 1,000 matched users minimum for audiences to be eligible in campaigns.
* Audience creation and updates are subject to TikTok’s moderation and API rate limits.
* For EU advertisers, compliance confirmation is mandatory before January 10, 2026, or API access may be restricted.

***

### Summary

TikTok Customer File Audience Activation through MadConnect provides:

* Direct audience onboarding via TikTok API
* Secure hashed PII transfer
* Compliance with TikTok’s API and privacy policies
* Automated updates through manual or scheduled jobs

For additional guidance on TikTok Customer File Audience Activation, refer to the[ TikTok Business API documentation on Customer File Audiences](https://business-api.tiktok.com/portal/docs?id=1747012327159809) and[ File Upload Requirements](https://business-api.tiktok.com/portal/docs?id=1739940567842818).

\
