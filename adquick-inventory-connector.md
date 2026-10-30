# AdQuick - Inventory Connector

MadConnect enables seamless integration with the AdQuick API, allowing you to pull out-of-home (OOH) inventory availability — available billboards, transit and digital screens, with pricing, impressions, and flight dates — for an AdQuick campaign directly into your own data warehouse.

## Connector Overview

| Field               | Description                                                                  |
| ------------------- | ---------------------------------------------------------------------------- |
| Connector Type      | Source                                                                       |
| Data Type           | Inventory                                                                    |
| Description         | Retrieve the available OOH units (avails) for an AdQuick guaranteed campaign |
| Primary Use Cases   | OOH media planning, reviewing avails and pricing across media owners         |
| Supported Inventory | Guaranteed (direct-sold) OOH inventory, static and digital                   |
| Supported Actions   | Read                                                                         |

## Prerequisites

### Authentication

* An AdQuick API key, issued by AdQuick (partner-gated access).
* MadConnect sends the key on every request as a Bearer token (`Authorization: Bearer <API key>`).
* The key does not need refreshing between runs.

### Access Requirements

* The API key must have access to the campaign you want to pull.
* Outbound HTTPS access to `api.adquick.com`.
* A MadConnect workspace with a destination configured.

### Know Your Campaign Before Configuring

* An AdQuick guaranteed campaign already exists for the advertiser, with flight dates and target locations set.
* The Campaign Token of that campaign (e.g. `z49dl31wfk`).

## Steps to Obtain Credentials

{% stepper %}
{% step %}
### Request AdQuick API Access

* Contact your AdQuick account team or api@adquick.com to request API access for MadConnect
{% endstep %}

{% step %}
### Create the Campaign in AdQuick

* Create a guaranteed campaign with budget, flight dates, locations and media types
{% endstep %}

{% step %}
### Find the Campaign Token

* Returned when the campaign is created; listed with each campaign in AdQuick's API
{% endstep %}
{% endstepper %}

## Setting Up the Connector in MadConnect

{% stepper %}
{% step %}
### Add Platform

* Go to **My Platforms → Add Platform**, select **AdQuick Inventory (Source)**, click **Configure**
{% endstep %}

{% step %}
### Authenticate

* Enter the AdQuick API Key and click **Save**
{% endstep %}

{% step %}
### Create Connection

* Choose **AdQuick Inventory** as the source, enter the **Campaign Token**, choose your destination
{% endstep %}

{% step %}
### Run or Schedule

* Run a **Manual Pull** or configure a **Scheduled Sync**
{% endstep %}

{% step %}
### Review and Save Connection
{% endstep %}
{% endstepper %}

## Data Dictionary

| Field          | Required | Description                             |
| -------------- | -------- | --------------------------------------- |
| API Key        | Yes      | AdQuick API key, sent as a Bearer token |
| Campaign Token | Yes      | AdQuick's ID of the guaranteed campaign |
| Destination    | Yes      | Where inventory is delivered            |
| Schedule       | No       | Manual Pull or Scheduled Sync           |

Output is one row per available OOH unit, columns named as AdQuick provides them. \[confirm: exact output column list/types not in source material — add concrete field-by-field schema once available from engineering/QA]

## Additional Considerations

* **Avails build over time** as media owners respond; schedule regular syncs.
* **Read-only**: never creates/changes/books campaigns in AdQuick.
* **One campaign per connection.**
* **Pricing in USD**; cpm/impressions are four-week values.
* \[confirm: as of this draft, the connector's code has merged to main but has had only limited testing per Harshad's Slack note — confirm end-to-end behavior, error handling and the output schema above before publishing this page live.]

## Additional Resources

* [AdQuick API documentation](https://docs.adquick.com/api)
* AdQuick API access: api@adquick.com
