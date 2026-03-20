# VideoAmp - Audience

## VideoAmp Audience Connector

### Connector Overview

| Field                 | Value                                                                                                                                                                                                                                                           |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Connector Type**    | Destination                                                                                                                                                                                                                                                     |
| **Data Type**         | Audience                                                                                                                                                                                                                                                        |
| **Description**       | Enables creation and delivery of audiences into VideoAmp using Snowflake Direct Share or S3-based ingestion. Audiences are created within VideoAmp and can then be exported to downstream destinations such as Snowflake, clean rooms, or activation platforms. |
| **Supported Actions** | Create, Get, Export                                                                                                                                                                                                                                             |

***

### Prerequisites

* Active **VideoAmp account with API access**
* VideoAmp-provisioned:
  * Client ID
  * Username (email)
  * Password
* Configured **Holding Company and Business Units**
* Snowflake environment configured for **Direct Share with VideoAmp**
* Required identifier types enabled in VideoAmp (e.g., EMAIL\_SHA256, DEVICE\_ID)
