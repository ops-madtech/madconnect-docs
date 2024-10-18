# META / Facebook Ads - Audience

![](<.gitbook/assets/image (16).png>)

#### Meta Ads -Custom  Audiences Overview

MadConnect integrates seamlessly with Meta’s Custom Audiences API, enabling advertisers to effectively manage and target their audiences on Meta platforms, including Facebook and Instagram. With this connector, businesses can sync customer data directly from their data sources to Meta, ensuring more precise and impactful audience targeting.

***

**Connector Overview**

* **Connector Type**: Destination
* **Data Type**: Custom Audiences
* **Description**: Custom audiences allow advertisers to target their ads to a specific set of people with whom they have already established a relationship.
* **Supported Actions**: Add

***

#### Prerequisites

1. **Active Meta Ads Account**
   * Ensure you have an active Meta Ads account with permissions to manage Custom Audiences.
2. **OAuth Authentication**
   * Authenticate your Meta Ads account using OAuth within MadConnect.
3. **Existing Audiences in Meta Ads**
   * Verify the audience exists in Meta; if not, create it within Meta Ads Manager before syncing.

***

#### Configure Connector

1. **Navigate to the My Platforms Section**
   * Go to "My Platforms" in the MadConnect UI.
2. **Add a New Platform**
   * Click on "Add Platform."
3. **Select Meta - Custom Audiences**
   * Choose the "Meta - Custom Audiences" tile and click "Configure."
4. **Go to Destination Configuration**
   * Open the "Destination Configuration" tab.
5. **Sign in with Meta**
   * Click "Sign in with Meta" and log in using your Meta account credentials.
6. **Authorize MadConnect**
   * Grant MadConnect permission to access and manage your Meta Custom Audiences.
7. **Verify Configuration**
   * Ensure the platform status is marked as "Configured" under My Platforms.

***

#### Steps to Create an Empty Custom Audience in Meta

1. **Log in to Ads Manager**
   * Go to Meta Ads Manager and click on "Audiences" in the menu.
2. **Click New Audience**
   * Select "Create Audience" > "Custom Audience" > "Customer List."
3. **Create a Name**
   * Provide a name for the new audience.
4. **Select Data Type**
   * Choose the type of data used: Email, Phone Number, or Mobile Ad ID.
5. **Create the Audience**
   * Save the empty audience.
6. **Obtain the Audience ID**
   * Locate the audience ID in the Ads Manager and provide it to your representative for syncing.

For more details on Meta Custom Audiences, please refer to the[ Meta Custom Audiences documentation](https://www.facebook.com/business/help/744354708981227).
