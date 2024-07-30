# Criteo Audience API

![](<.gitbook/assets/image (68).png>)

Criteo is a prominent global technology company that specializes in performance-driven digital advertising. Its platform empowers marketers with targeted, personalized ad campaigns, utilizing extensive data analytics to enhance online retail and advertising strategies. Known for its dynamic retargeting techniques, Criteo's solutions are designed to drive sales and user engagement, making it a key player in the digital marketing landscape with a strong focus on delivering measurable results.

**Platform Category:** Advertising

Criteo's Audience API enables you to effectively manage contact lists for advertising campaigns, with functionalities to create, delete, and update audience data. It allows the management of audience details, including names, descriptions, and user lists, and provides insights into audience size and matched users in relation to Criteo's Shopper Graph.

[https://developers.criteo.com/marketing-solutions/docs/audiences](https://developers.criteo.com/marketing-solutions/docs/audiences)&#x20;

**Connection Type:** Destination.

#### Configuring Criteo as Destination

To configure Criteo as a destination,

1. Go to My Platforms. Make sure Criteo displays in your instance of My Platforms.
2. Click Criteo.
3. Login using your Criteo Account.
4. Click allow to authenticate using OAuth 2.0

#### Data Attributes for Criteo Data

Audiences have some specific parameters, which can be Required, Optional or Computed (filled automatically after Audience creation)

<table data-header-hidden><thead><tr><th width="162"></th><th width="100"></th><th width="196"></th><th></th></tr></thead><tbody><tr><td>Field Name</td><td>Type</td><td>Optional / Required / Computed</td><td>Description</td></tr><tr><td>id</td><td>String</td><td>Computed</td><td>Unique ID of the audience</td></tr><tr><td>name</td><td>String</td><td>Required</td><td>Name of the audience. It must be unique per advertiser.</td></tr><tr><td>description</td><td>String</td><td>Optional</td><td>Description of the audience</td></tr><tr><td>createdAt</td><td>String</td><td>Computed</td><td>ISO-8601 timestamp in UTC of audience creation (read-only)</td></tr><tr><td>updatedAt</td><td>String</td><td>Computed</td><td>ISO-8601 timestamp in UTC of audience update (read-only)</td></tr><tr><td>advertiserId</td><td>String</td><td>Required</td><td>Advertiser associated to the audience</td></tr><tr><td>adSetIds</td><td>String Array</td><td>Computed</td><td>Ad sets associated with the audience (read-only).</td></tr><tr><td>algebra</td><td><a href="https://developers.criteo.com/marketing-solutions/docs/algebra-nodes">Algebra Node</a></td><td>Required</td><td>Algebra Node with the definition of how the different segments are mixed to create the audience using logical operators: AND, OR, NOT.</td></tr></tbody></table>
