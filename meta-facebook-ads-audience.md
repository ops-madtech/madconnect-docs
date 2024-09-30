# META / Facebook Ads - Audience

![](<.gitbook/assets/image (16).png>)

MadConnect integrates seamlessly with Meta’s Custom Audiences API, enabling advertisers to effectively manage and target their audiences on Facebook. With this connector, businesses can sync customer data directly from their data sources to Meta, ensuring more precise and impactful audience targeting.

**Summary**

* **Source / Destination**: Destination
* **Connector Type**: CPC
* **Data Type**: Custom Audiences
* **Description**: Custom audiences allow advertisers to target their ads to a specific set of people with whom they have already established a relationship on/off Facebook.

**Prerequisites**

To activate Custom Audiences on Meta using MadConnect, ensure the following prerequisites are met:

* Meta Ads Account: Ensure you have an active Meta Ads account and the appropriate permissions to manage Custom Audiences.
* OAuth Authentication: Authenticate your Meta Ads account destination using OAuth to enable audience sync and management.
* Ensure the Audience(s) exist in the Meta Ads account.

**Authentication Process**&#x20;

1. **Navigate to the My Platforms Section:** In the MadConnect UI, go to the "My Platforms" section.
2. **Add a New Platform:** Click on "Add Platform."
3. **Select Meta - Custom Audiences:** Choose the "Meta - Custom Audiences" tile and click on "Configure."
4. **Go to Destination Configuration:** Navigate to the "Destination Configuration" tab.
5. **Sign in with Meta:** Click the "Sign in with Meta" button.
6. **Authenticate with Meta:** You will be redirected to Meta’s OAuth login page. Sign in using your Meta account credentials.
7. **Authorize MadConnect:** Grant MadConnect permission to access and manage your Meta Custom Audiences.
8. **Redirect to MadConnect:** After successful authentication, you will be redirected back to MadConnect, where the configuration will be completed.

#### Steps to Create an Empty Custom Audience in Meta

1. **Create Custom Audience:** Once in your account, go directly to Audiences, then go to Create Audience > Custom Audience > Customer list.
2. **Name Your Audience:** Provide a name for your empty audience.&#x20;
3. **Save the Audience:** Click "Create Audience" to save the empty Custom Audience.
4. **Audience Created:** The empty audience will now appear in the "Audiences" list within Ads Manager, ready for future data uploads or audience syncing via the API.
5. Obtain the Audience ID and provide the ID to your representative.

#### Where Can I Find My Meta Audience ID?

To obtain your Meta Audience ID, follow these steps:

1. Navigate to the Audience page within Ads Manager.
2. Audience ID will be available as a column in the list of Audiences

For more details on Meta Custom Audiences, please refer to the[ Meta Custom Audiences documentation](https://www.facebook.com/business/help/744354708981227).
