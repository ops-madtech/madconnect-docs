# LiveRamp - Envelope Decryption

![](<.gitbook/assets/image (29).png>)

LiveRamp is a technology company that provides identity resolution and data connectivity services. It plays a crucial role in the advertising technology (ad tech) ecosystem, helping advertisers and marketers connect, unify, and activate their data across various channels and platforms.

The RampID API provides the decryption capability using the envelope decryption endpoint. This endpoint is used to decode and decrypt an envelope to extract the identifier in the envelope. The RampID API then encrypts the value with a partner-specific encryption key and partner ID. The result is a RampID in the partner ID space that is usable for transactions.

[https://developers.liveramp.com/rampid-api/docs/envelope-decryption-endpoint](https://developers.liveramp.com/rampid-api/docs/envelope-decryption-endpoint) &#x20;

**Connection Type:** Destination.

#### Configuring Envelope Decryption API as a Destination

To configure LiveRamp Envelope Decryption as destination,

1. Go to My Platforms. Make sure LiveRamp Envelope Decryption displays in your instance of My Platforms.
2. Click LiveRamp Envelope Decryption.
3. Insert your Client Id and Secret Key which you can request from your LiveRamp CSM.
4. Click allow to authenticate using OAuth 2.0.
