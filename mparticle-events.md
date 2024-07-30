---
description: >-
  Learn how to deploy mparticle connector as a source and destination on
  MadConnect.
---

# mParticle - Events

![](https://lh7-us.googleusercontent.com/Q8emtVgduOrId7OkscCawIrq-k-NTuGqKx9TuoRj1Rx-N1ChgEop3mncZoiOHA2L3vi4cQ36aHdXIV7nNJU73us3attk8Mhf4Em49U18f-8\_8l4rcD7RhOWhZpaMG7PPSD1H-ZlVkzHVG0fOOrpLvg)

[MParticle](https://docs.mparticle.com/), is a leading customer data platform (CDP) that empowers businesses to harness and utilize their customer data effectively. The platform enables seamless data integration and activation across various channels, providing a unified view of customer interactions. MParticle facilitates personalized and targeted marketing strategies, enhancing customer experiences.

mParticle provides an HTTP-based Events API that you can use to collect data from your backend systems.

**Connection Type:** Source and Destination.

**Data Type:** Events.

### Configuring mParticle - Events as Source

Prerequisite: You must have a dedicated S3 bucket assigned to mParticle.

To configure mParticle as source,

1. Copy the API key. that is provided in your MadConnect account before activating your connection.
2. [Login](https://app.mparticle.com/login) to mParticle account.
3. Go to connection settings.
4. &#x20;Paste the API key.

### Configuring mParticle - Events as Destination

mParticle provides a Key and Secret during Setup > Input > Selected Platform > Platform Configuration.

Copy the Key and Secret in MadConnect > mParticle > Destination API Configuration.

#### Data Attributes for Event Data

The data attributes for Event data depend on the selected parameters. &#x20;

| <p>Common event data node properties</p><p><code>{</code></p><p>    <code>"data": {</code></p><p>        <code>"event_id": 6004677780660780000,</code></p><p>        <code>"event_num": 0,</code></p><p>        <code>"source_message_id": "e8335d31-2031-4bff-afec-17ffc1784697",</code></p><p>        <code>"session_id": 4957918240501247982,</code></p><p>        <code>"session_uuid": "91b86d0c-86cb-4124-a8b2-edee107de454",</code></p><p>        <code>"timestamp_unixtime_ms": 1402521613976,</code></p><p>        <code>"location": {},</code></p><p>        <code>"device_current_state": {},</code></p><p>        <code>"custom_attributes": {},</code></p><p>        <code>"custom_flags": {}</code></p><p>    <code>},</code></p><p>    <code>"event_type": ""</code></p><p><code>}</code><br></p> |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

