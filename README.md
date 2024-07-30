# Overview

Connectors are at the heart of the MadConnect platform. Creating connectors on MadConnect is a simple drag-and-drop process. MadConnect currently supports the following types of connectors:

* Streaming Connectors
* Batch Connectors
* Pull Connectors
* Push Connectors

### Streaming Connectors

Streaming Connectors are bi-directional data connectors designed for non-scheduled platform-to-platform communication via individual row or small batch rapid transfer of data

Examples:

* Meta Marketing API
* Snapchat Conversions API

### Batch Connectors

Batch Connectors are used for scheduled, large file transfer of data between platforms. They are designed for the purpose of transferring large volumes of data.&#x20;

Example:

File transfer from source AWS S3 to destination cloud storage.

### Pull Connectors

A Pull Connector is a type of streaming connector from which data is fetched from the Destination connector side via API.

Example:

Facebook Analytics Connector.

### Push Connectors

A Push Connector is a type of streaming connector in which data is pushed from the Source connector side via API.

Example:

Snapchat Conversions Connector.
