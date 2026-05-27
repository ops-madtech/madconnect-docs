# Overview

## Source & Destination Connectors

MadConnect integrations are built around a connector-based model that separates where data comes from from where it is sent. This design allows customers to create flexible, reusable data workflows across a wide range of platforms and use cases.At a high level:

* **Source connectors** define where data originates
* **Destination connectors** define where data is delivered
* **Connections** combine a source and a destination into an executable workflow

This separation enables MadConnect to support many platforms and use cases while maintaining consistency, scalability, and control.

***

#### Source Connectors <a href="#source-connectors" id="source-connectors"></a>

A source connector represents a system where data is read from.Sources may include:

* Data warehouses and cloud storage platforms
* Advertising and measurement platforms (for reporting data)
* Operational or partner systems

Source connectors are responsible for:

* Accessing data from the underlying platform
* Exposing available datasets, tables, or objects
* Supporting scheduling and incremental data extraction where applicable

A single source connector can be reused across multiple connections and destinations.

***

#### Destination Connectors <a href="#destination-connectors" id="destination-connectors"></a>

A destination connector represents a system where data is delivered.Destinations typically include:

* Advertising platforms
* Measurement and analytics systems
* Partner APIs and downstream data platforms

Destination connectors are responsible for:

* Enforcing schema and API requirements
* Supporting platform-specific actions and behaviors
* Validating data before delivery

A single destination connector can be reused across multiple connections and sources.

***

#### The Connector Framework <a href="#the-connector-framework" id="the-connector-framework"></a>

MadConnect uses a standardized connector framework to ensure integrations are consistent, secure, and scalable across the platform.At a high level, each connector defines:

* How MadConnect authenticates with an external system
* What data schemas, identifiers, or metrics are supported
* How data is read from or delivered to the platform
* Platform-specific validation, constraints, and error handling

Connectors are modular and reusable, meaning the same connector can be used across multiple workspaces and connections. The connector framework is tightly integrated with:

* **Cloud Configuration**, which defines where execution occurs
* **Schemas and Mappings**, which define how data is structured
* **Execution and Monitoring**, which ensures workflows are reliable and observable

This approach allows MadConnect to support a growing ecosystem of platforms while maintaining a consistent user experience.

***

#### Connector-Specific Documentation <a href="#connector-specific-documentation" id="connector-specific-documentation"></a>

Each source and destination connector has its own documentation page that outlines:

* Supported use cases
* Required and optional fields
* Supported identifiers, metrics, and actions
* Platform-specific limitations or requirements<br>
