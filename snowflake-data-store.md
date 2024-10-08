# Snowflake - Data store

MadConnect integrates with Snowflake to allow seamless loading of reporting data from various source platforms directly into Snowflake tables or views. This connector enables businesses to centralize their advertising, audience, or campaign data within Snowflake for more efficient data analysis, reporting, and optimization.



```


|   Source/Destination   |   Connector Type         |   Data Type    |   Description                                                             |   Supported Actions   |
| ---------------------- | ------------------------ | -------------- | ------------------------------------------------------------------------- | --------------------- |
| Destination            | Cost Per Connector (CPC) | Reporting Data | Load reporting data from source platforms directly into Snowflake tables. | Get                   |


```







#### Prerequisites

To set up the **Snowflake - Datastore** connector in MadConnect, ensure the following prerequisites are met:

* MADCONNECTUSER has access to the table used to receive data

```
// Commands for granting MADCONNECT application access to the table 
GRANT USAGE ON DATABASE <DATABASE_NAME> TO APPLICATION MADCONNECT;
GRANT USAGE ON SCHEMA <DATABASE_NAME>.<SCHEMA_NAME> TO APPLICATION MADCONNECT;
GRANT CREATE STAGE ON SCHEMA <DATABASE_NAME>.<SCHEMA_NAME> TO APPLICATION MADCONNECT;
GRANT CREATE TABLE ON SCHEMA <DATABASE_NAME>.<SCHEMA_NAME> TO APPLICATION MADCONNECT;
```

#### Example Report Types

This connector supports loading reporting data such as:

* Campaign Performance Data
* Audience Segments
* Ad Impressions, Clicks, and Conversions
* Financial Data related to campaign costs
* Custom Metrics and Dimensions from the source platform

\
**Note**

* Data received will be appended to the table
* If data received has extra columns, they will be added to the table&#x20;
