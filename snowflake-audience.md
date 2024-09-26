# Snowflake - Audience

MadConnect integrates with Snowflake, enabling businesses to seamlessly connect their audience data stored in Snowflake to various advertising platforms for audience activation. This connector allows users to sync audience data tables or views directly from Snowflake to their desired destinations, facilitating smooth and efficient data transfers for targeted advertising campaigns.

**Connection Type**: Source.

**Data Type:** Audiences.

\
**Schema**&#x20;

{% code fullWidth="false" %}
```
| Key           | Data Type | Example                                                                 |
| ------------- | --------- | ----------------------------------------------------------------------- |
| \<ID>         | String    | It can be any value such as TDID, UID2, EMAIL etc. Refer to table below |
| segment\_id   | String    | 22343245                                                                |
| segment\_name | String    | Sport Lover                                                             |
| action        | String    | Possible options are add/remove/opt-out                                 |
```
{% endcode %}

Possible value of ids

1. EMAIL - Email address, EMAIL\_HASH
2. FNAME - First name, FNAME\_HASH
3. LNAME - Last name, LNAME\_HASH
4. PHONE - Phone number, PHONE\_HASH
5. DOB - Date of birth, DOB\_HASH
6. DOBM - Date of birth Month, DOBM\_HASH
7. DOBY - Date of birth Year,  DOBY\_HASH
8. DOBD - Date of birth Day, DOBD\_HASH
9. GENDER - Gender, GENDER\_HASH
10. LOCATION  - Location, LOCATION\_HASH
11. CITY - City, CITY\_HASH
12. STATE - State, STATE\_HASH
13. STATE\_CODE - 2 digit, STATE\_CODE\_HASH
14. COUNTRY - Country, COUNTRY\_HASH
15. REGION - Region, REGION\_HASH
16. REGION\_CODE, REGION\_CODE\_HASH
17. IDFA - Apple Identifier for Advertising
18. GAID - Google Advertising ID
19. RIDA - Roku TV and Streaming Devices
20. TVOS - Apple TV
21. CHRM - Android TV / Google Chromecast Devices
22. AFAI - Amazon Fire TV Devices
23. SMSG - Samsung TVs
24. HIP4 - IPv4 Address
25. Hashed types are identified by adding \_\<hash\_algo> suffix to the types. Example: EMAIL\_SHA256 as shown below json
26. UID2 - Universal ID 2.0
27. ID5 - ID5 id
28. RAMPID - LiveRamp RampID
29. POSTAL\_CODE - Postal Code , POSTAL\_CODE\_HASH
30. COUNTRY\_CODE - Country Code , COUNTRY\_CODE\_HASH
31. DEVICE\_ID - Unknown device id
32. TDID - GUID - The Trade Desk 36-character GUID (including dashes) for this user.
33. DAID - GUID - The raw device ID for this user, sent in 36-character GUID format (including dashes). Use iOS IDFA or Android's AAID.
34. UID2Token - String - The encrypted UID2 token, also known as an advertising token. This token is case-sensitive. Tokens are - generated and managed using UID2 APIs. For details about UID2 APIs, see UID2 Endpoints. For details about UID2 tokens, - see Raw IDs vs. Tokens.
35. EUIDToken - String - The encrypted EUID token, also known as an advertising token. This token is case-sensitive. For details, see Unified IDs.
36. EUID - String - The raw European Unified ID value, also known as EUID. This value is case-sensitive. EUID offers user transparency and privacy controls designed to meet market requirements in European regions with the same normalization and encoding of PII as UID2. See also European Unified ID Overview.
37. IDL - String - "The 49-character or 70-character RampID (previously known as IdentityLink).
38. NetID - String - The user's netID as a 70-character base64-encoded string. For details about netID, see the netID Developer Portal.
39. FirstId - String - The user's First-id, a first-party cookie typically set by publishers in France. For details about First-id, see the First-id site.
40. MADID - String  -  Mobile advertiser ID , Use all lowercase, and keep hyphens ,Hashing NOT required   39.  EXTERN\_ID - String - ID , no hashing required 40.  FI - String - first name initial, Hashing required, Use a-z only. Lowercase only. Special characters in UTF-8 format , FI\_HASH.&#x20;
41. IDFA\_MD5 - MD5 hash of an iOS IDFA
42. AAID\_MD5 MD5 hash of an Android AAID/GAID
43. IDFA\_SHA256 SHA256 iOS IDFA
44. AAID\_SHA256 SHA256 Android AAID/GAID

NOTE:

1. At-least one id should be present per object
2. All id keys should be mentioned in lowercase. Example: email, idfa, gaid fname, dob\_hash
