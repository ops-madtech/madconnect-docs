# Yahoo Traffic API - Audiences

#### Yahoo DSP - Audiences Overview

MadConnect supports updating Yahoo DSP audiences, allowing you to seamlessly include email and phone number audiences.

Yahoo DSP helps you build and target audiences with similar interests, habits, or behaviors, enhancing your campaign effectiveness. With MadConnect, you can easily update these audiences, ensuring your marketing efforts are always directed at the most responsive and relevant consumers.

***

**Connector Overview**

* **Connector Type**: Destination
* **Data Type**: Audience
* **Description**: Sync your Email and Phone Numbers to update audiences in Yahoo DSP for activation.
* **Supported Actions**: Add&#x20;

***

**Prerequisites**

1. **Enable DSP API**:&#x20;
   * Contact your Yahoo DSP Account Manager or Product Support to enable the DSP API for your account.
2. **Existing Audience Segment**:&#x20;
   * Ensure the audience segment already exists in the Yahoo DSP platform. MadConnect can update existing segments but does not create new segments with Yahoo DSP.
3. **Credentials Required for Authentication**:
   * **Client ID**
   * **Client Secret**\
     If the DSP API is not enabled, you will see a message indicating that the Client ID and Client Secret are “undefined.” Contact your Yahoo DSP Account Manager or Product Support to resolve this.

***

**Steps to Obtain Credentials:**

1. **Get Client ID and Secret**:
   * Open the DSP UI and select your name in the upper-right corner.
   * Select **My Account** and then the **Activate** button.
   * Agree to the terms of service to receive a success message with your Client ID and Secret.
   * Copy and store the Client ID and Secret safely.

***

**Configure Connector:**

1. Navigate to the **My Platforms** Section:
   * In the MadConnect UI, go to the **My Platforms** section.
2. Add a New Platform:
   * Click on **Add Platform**.
3. Select **Yahoo DSP - Custom Audiences**:
   * Choose the **Yahoo DSP - Custom Audiences** tile and click on **Configure**.
4. Go to **Configuration**:
   * Navigate to the **Configuration** tab.
5. Enter **Client ID** and **Client Key**:
   * Manually enter your Yahoo DSP **Client ID** and **Client Key** into the provided fields.
6. **Verify** Configuration:
   * Ensure the connector status is marked as **Configured** under **My Platforms**.

***

**Audience Schema Requirements**

To successfully send data to Yahoo DSP audiences via MadConnect, the following minimum schema must be used:

1. \<ID> Field
   * **Field Name**: EMAIL\_HASH
   * **Data Type**: String (Hashed)
   * **Description**: This field contains the hashed emails of the audience members
   * **Accepted Hashing Algorithms**: SHA-256
   * **Example**: A hashed email such as 5d41402abc4b2a76b9719d911017c592.
2. Segment ID Field
   * **Field Name**: segment\_id
   * **Data Type**: String
   * **Description**: The unique audience ID assigned by Yahoo DSP for the specific audience segment.
   * **Example**: 123456
3. Action Field
   * **Field Name**: action
   * **Data Type**: String
   * **Description**: Specifies whether to add or remove the user from the audience.
   * **Accepted Values**: add
   * **Example**: add

For more information on Yahoo DSP Authentication, please review the [Yahoo DSP Documentation](https://developer.yahooinc.com/dsp/api/docs/traffic/audience/about-audience.html).

***

\
