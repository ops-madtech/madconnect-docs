# LinkedIn - Audiences DMP Segments

![](https://lh7-us.googleusercontent.com/hmt0Bu-TkGuUf5T78r_PlZ4pUf4MXoajCcZ8SOakq-um5b_FrwMBE-5A72MVP36NSsVKXTMr3ffZ1nA2gJj4V8SW4Fi5jckOwkDw6fLuVLr6vOfGUSHHgCNI20kbAv4uMxbQEG99b-N2tgaDsf5yFw)

#### **LinkedIn Ads - DMP Segment Audience Overview**

MadConnect integrates with LinkedIn’s DMP Segment API, allowing you to efficiently manage audience segments by adding or removing users based on hashed data, such as emails or mobile advertising IDs. Use LinkedIn’s platform to target the right audience for your campaigns.

***

#### **Connector Overview:**

* **Connector Type:** Destination
* **Data Type:** Audience
* **Description:** Sync hashed user data, such as emails or mobile advertising IDs, to update audiences in LinkedIn DMP Segments for targeted advertising.
* **Supported Actions:** Add, Remove

***

#### **Prerequisites:**

1. **OAuth Authentication:**
   * You will need to authenticate your LinkedIn Ads account using OAuth.
2. **Audience Segment Creation:**
   * Ensure the audience segment has already been created on LinkedIn before adding or removing users. You must wait at least 5 seconds after segment creation for it to be available.

***

#### **Configure Connector:**

1. **Navigate to the My Platforms Section:**
   * In the MadConnect UI, go to the "My Platforms" section.
2. **Add a New Platform:**
   * Click on "Add Platform."
3. **Select LinkedIn Ads - DMP Segment:**
   * Choose the "LinkedIn Ads - DMP Segment" tile and click on "Configure."
4. **Go to Destination Configuration:**
   * Navigate to the "Destination Configuration" tab.
5. **Sign in with LinkedIn:**
   * Click the "Sign in with LinkedIn" button.
6. **Authenticate with LinkedIn:**
   * You will be redirected to LinkedIn’s OAuth login page. Sign in using your LinkedIn account credentials.
7. **Authorize MadConnect:**
   * Grant MadConnect permission to access and manage your LinkedIn DMP Segments.
8. **Redirect to MadConnect:**
   * After successful authentication, you will be redirected back to MadConnect, where the configuration will be completed.

***

#### **Audience Schema Requirements:**

To successfully send data to LinkedIn DMP Segments via MadConnect, the following schema must be used:

1. **Key Field:**
   * **Field Name:** userIds
   * **Data Type:** List of Hashed IDs (such as SHA256\_EMAIL, SHA512\_EMAIL, GOOGLE\_AID)
   * **Description:** The list of hashed email addresses or mobile advertising IDs for the users to add or remove from the segment.
   * **Example:** 692682111bc191d915ac7009d118a78bc496cf7a2ba8c2d0134ade012ac1234
2. **Segment ID Field:**
   * **Field Name:** segment\_id
   * **Data Type:** String
   * **Description:** The unique ID assigned by LinkedIn for the specific audience segment.
   * **Example:** 10804
3. **Action Field:**
   * **Field Name:** action
   * **Data Type:** String
   * **Description:** Specifies whether to add or remove the user from the audience.
   * **Accepted Values:** add, remove
   * **Example:** add

***

For more information on LinkedIn Ads audience management, please refer to the[ LinkedIn Ads Documentation](https://learn.microsoft.com/en-us/linkedin/marketing/matched-audiences/create-and-manage-segment-users?view=li-lms-2023-11\&tabs=http).

