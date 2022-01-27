# Push notifications for messages

### Prerequisites
To be able to retrieve the message count, messages must first be persisted.

To set message persistence at the app level, the persistent message feature MUST be enabled at the key level in the [Temasys Developer Console](https://console.temasys.io/#).

Messages will also only be persisted if the messages are encrypted, are public messages and, are sent via the signaling server using the `sendMessage` method.

## How to retrieve persistent message count using the Temasys REST API
Endpoint url: `https://api.temasys.io`

**GET/rest/notifications/persistentMessages/\<appKey>/\<roomid>/\<timestamp>?credentials=\<credentials>**
> |Parameter|Description|
> |-|-|
> |appKey|The app key that is used in `initOptions` when instantiating the Skylink instance.|
> |roomid|The room id retrieved from the `peerInfo` object.|
> |timestamp|The timestamp, in epoch milliseconds, from which to retrieve all future messages.|
> |credentials|A Hex string generated using HmacSHA1 protocol for authenticating the request. *Refer to steps below.*|

The following elements are returned in the response:
> |Parameter|Description|
> |-|-|
> |senderPeerId|The message count of each peer keyed by `peerId`.|
> |totalMessagesSent|The total messages sent in the room from the `timestamp`.|

Example response:
```
{
   "status": 200,
   "error": null,
   "message": {
      "senderPeerId": {
         "qhVJYAEWdsf8tbosAAD7": 5,
         "H1IeA6VMTr2jrMbgAAD8": 1
      },
   "totalMessagesSent": 6
   }
}
```

## How to generate credentials?
1. Locate your keys `secret` on the [Temasys Developer Console](https://console.temasys.io/#)
2. Install and import [CryptoJS](https://www.npmjs.com/package/crypto-js)
3. Apply HmacSHA1 to the <appKey_roomid_timestamp, secret> pair <br>
   `const credentials = CryptoJS.enc.Hex.stringify(CryptoJS.HmacSHA1('${appKey}_${roomid}_${timestamp}',secret));`
