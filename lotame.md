# Lotame

![](https://lh7-us.googleusercontent.com/ZQ6ACjyqsVxBVYNxgauzFNKqW-wgS4CmKB7Cn75IgJhrexyq2YNvZt-77qQTJMaGtxqNaFvt2XbY-4WDotcAOrX2OJVhHpIENPk4w4WkBvAq9iBB1KMwHIl889\_IenApAuOZct1ImVQa8mmrSfXMuQ)

[Lotame](https://www.lotame.com/) is a data management platform that helps marketers, publishers, and agencies collect customers' data to optimize audience segments and campaign targeting processes. The solution allows managers to analyze users' behavior using machine learning algorithms and create strategies to reach performance goals.

### LOTAME Audiences

Audiences Data is used to maximize customer acquisition all while allowing for suppression and look alike modeling

**Connection Type:** Source and Destination.

**Data Type:** Audiences.

Prerequisite: You must have a dedicated S3 bucket assigned to Lotame.

#### Configuring Lotame as a Source

To configure Lotame as a source, you must set up AWS S3 Transfer with your [Lotame account](https://platform.lotame.com/).

1. Set up AWS S3 Transfer with your Lotame account. You may need to coordinate with Lotame’s Technical Account Manager. You will get clientDir value while setting up AWS S3 Transfer.
2. Login to your MadConnect account.
3. Click Platforms. Ensure that Lotame displays in your instance of My Platforms.
4. Click Lotame.
5. Paste clientDir in source configuration.

#### Configuring Lotame as a Destination

To configure Lotame as a destination, you must set up AWS S3 Transfer with your [Lotame account](https://platform.lotame.com/).

1. Set up AWS S3 Transfer with your Lotame account. You may need to coordinate with Lotame’s Technical Account Manager.
2. You will get clientDir value while setting up the AWS S3 Transfer.
3. Login to your MadConnect account.
4. Click My Platforms. Ensure that Lotame displays in your instance of My platforms.
5. Click Lotame.&#x20;
6. Paste clientDir in destination configuration

**Data Attributes for Audiences Data**



**Supported User Identities**

<table data-header-hidden><thead><tr><th width="216">Field</th><th>Description</th></tr></thead><tbody><tr><td>User ID</td><td>The id of the user. Supported <a href="https://dev098b.madconnect.io/e6d1adf4-c955-11ed-8cb2-f7ad6676aa63/Lotame/Lotame.html#kix.rvcy7u1zqt9">user ids</a>  </td></tr><tr><td>Audience segments</td><td>Behaviors that you want to associate with the user id</td></tr><tr><td>country</td><td>Two-character ISO country code that allows Lotame to apply a country behavior to the UserID.</td></tr><tr><td>ips</td><td>The sightings data for the userID.</td></tr></tbody></table>

**Supported Device Ids**

| PANO  | IDFA        | CHRM        | SONY        |
| ----- | ----------- | ----------- | ----------- |
| PID   | GAID        | AFAI        | LGTV        |
| TPID  | RIDA        | MSAI        | FTCH        |
| EMAIL | TVOS        | SMSG        | HIP4        |
| UUID  | <p><br></p> | <p><br></p> | <p><br></p> |

**Types of user ids**

<table data-header-hidden><thead><tr><th width="270.3333333333333">Type</th><th width="182">Field</th><th>Description</th></tr></thead><tbody><tr><td>Universal IDs</td><td>PANO</td><td>Lotame Panorama ID</td></tr><tr><td>Web IDs</td><td>PID</td><td>Lotame Profile IDs (pid)</td></tr><tr><td></td><td>TPID</td><td>Third Party IDs (tpid)</td></tr><tr><td>Declared IDs</td><td>EMAIL</td><td>Plain-Text Email</td></tr><tr><td>Mobile IDs</td><td>IDFA</td><td>Apple Identifier for Advertising</td></tr><tr><td></td><td>GAID</td><td>Google Advertising ID</td></tr><tr><td>Connected TV IDs (OTT) IDs</td><td>RIDA</td><td>Roku</td></tr><tr><td></td><td>TVOS</td><td>Apple TV</td></tr><tr><td></td><td>CHRM</td><td>Android TV/Chromecast</td></tr><tr><td></td><td>AFAI</td><td>Amazon Firestick</td></tr><tr><td></td><td>MSAI</td><td>Microsoft Advertiser ID</td></tr><tr><td></td><td>SMSG</td><td>Samsung TV</td></tr><tr><td></td><td>SONY</td><td>Sony TV</td></tr><tr><td></td><td>LGTV</td><td>LG TV</td></tr><tr><td></td><td>FTCH</td><td>Fetch TV</td></tr><tr><td></td><td>HIP4</td><td>IPv4 Address</td></tr><tr><td></td><td>UUID</td><td>Generic CTV ID</td></tr></tbody></table>
