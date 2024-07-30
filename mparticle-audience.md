# mParticle - Audience

![](https://lh7-us.googleusercontent.com/Q8emtVgduOrId7OkscCawIrq-k-NTuGqKx9TuoRj1Rx-N1ChgEop3mncZoiOHA2L3vi4cQ36aHdXIV7nNJU73us3attk8Mhf4Em49U18f-8\_8l4rcD7RhOWhZpaMG7PPSD1H-ZlVkzHVG0fOOrpLvg)

[MParticle](https://docs.mparticle.com/), is a leading customer data platform (CDP) that empowers businesses to harness and utilize their customer data effectively. The platform enables seamless data integration and activation across various channels, providing a unified view of customer interactions. MParticle facilitates personalized and targeted marketing strategies, enhancing customer experiences.

You can Audience data externally to many destinations and use it to drive user engagement, encourage app downloads, etc..

**Connection Type**: Source.

**Data Type:** Audiences.

### Configuring mParticle- Audiences as Source

Prerequisite: You must have a dedicated S3 bucket assigned to mParticle.

To configure mParticle as source,

1. An API key is provided in your MadConnect account before activating your connection.
2. &#x20;Copy the API key.
3. &#x20;[Login](https://app.mparticle.com/login) to mParticle account
4. &#x20;Goto connection settings
5. &#x20;Paste this in the API key.

#### Data Attributes for Audience Data

The data attributes for audience data depend on the selected platform.  Following is a sample audience data structure:

| <p><code>{</code></p><p>  <code>"$age": "18",</code></p><p>  <code>"$gender": "M", "$</code></p><p>  <code>country": "USA",</code></p><p>  <code>"$zip": "10016",</code></p><p>  <code>"$city": "New York",</code></p><p>  <code>"$state": "NY",</code></p><p>  <code>"$address": "381 Park Avenue S",</code></p><p>  <code>"$firstname": "John",</code></p><p>  <code>"$lastname": "Doe",</code></p><p>  <code>"$mobile": "123-456-7890",</code></p><p>  <code>"a_custom_attribute": "some_value",</code></p><p>  <code>"a_custom_list": [ "value1", "value2", "valueN" ]</code></p><p><code>}</code></p> |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
