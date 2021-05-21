
# Getting Started with the Temasys SDK for Web

## **A step-by-step guide to embedding Real-Time Communication features into your webapp or website**

This document is designed to help developers get started using the Temasys SDK for the Web to add video & voice calling, secure messaging and screen sharing features to any website.

## **Step 1: Create an Account on the Temasys Console and Generate an App Key**

[Here are instructions for creating an account and generating an App Key.](https://github.com/Temasys/GettingStarted)

## **Step 2: Include Temasys SkylinkJS code in your website**

- In the example below, we have a html file with a video and audio element in the body. The video and audio elements are needed to attach the video stream and the audio stream respectively.
- We use a CSS transform call to mirror the image, so it looks and feels more natural.
- We mute the audio, so you don’t hear yourself speaking. The autoplay attribute is needed in some browsers where there are restrictions on autoplay.  [Autoplay policy changes](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes)

#### Include Video and Audio Elements in HTML file
```html
    <html>
      <head>
        <title>WebRTC with Temasys SkylinkJS</title>
        <script></script>
      </head>
      <body>
        <video id="myvideo" style="transform: rotateY(-180deg);" autoplay playinsline muted</video>
        <audio id="myaudio" autoplay playsinline muted</audio>
      </body>
    </html>
```          

## **Step 3: Include dependencies**

- Include socket.io in script tag 

```javascript
    <script  src="./path/to/socket.io.js"></script>
```

- Obtain `socket.io.js` from [here](https://cdnjs.cloudflare.com/ajax/libs/socket.io/2.2.0/socket.io.js). Check that the version is 2.2.0.

## **Step 4: Import the SDK**

There are 2 ways to import SkylinkJS:

i) Import SkylinkJS in a javascript file and include it in a script tag in the html.

- In `main.js`:
```javascript
     import Skylink,  { SkylinkEventManager, SkylinkLogger, SkylinkConstants } from './path/to/skylink.complete.js'
```

- Add `main.js` to the script tag in `index.html`. The html should look like this:
```html
    <html>
      <head>
        <title>WebRTC with Temasys SkylinkJS</title>
        <script  src="./path/to/socket.io.js"></script>
        <script  src="main.js" type="module"></script>
      </head>
      <body>
        <video id="myvideo" style="transform: rotateY(-180deg);" autoplay playinsline muted</video>
        <audio id="myaudio" autoplay playsinline muted</audio>
      </body>
    </html>
```  

ii) Include SkylinkJS directly in the html.
- The html should look like this:
```html
    <html>
      <head>
        <title>WebRTC with Temasys SkylinkJS</title>
        <script  src="./path/to/socket.io.js"></script>
        <script  src="./path/to/skylink.complete.js"  type="module"></script>
      </head>
      <body>
        <video id="myvideo" style="transform: rotateY(-180deg);" autoplay playinsline muted</video>
        <audio id="myaudio" autoplay playsinline muted</audio>
      </body>
    </html>
```   

- Note that in both methods, the `socket.io` dependency should be included before `SkylinkJS`.

- Obtain the latest Skylink code  [here](https://cdn.temasys.io/skylink/skylinkjs/latest/skylink.complete.js).

## **Step 5: Initialise and then listen for events**

i) Instantiate Skylink and initialise with configuration options

- For the appKey value, use the appKey which was provided in your app definition in the Temasys Console (https://console.temasys.io).

- Refer to the full list of init configuration options here: [initOptions](http://cdn.temasys.io/skylink/skylinkjs/2.x/docs/global.html#initOptions)

```javascript
    const config =  {
      appKey:  'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX',
      defaultRoom:  'skylinkRoom',
    };
    
    skylink =  new Skylink(config);
```

ii) Declare joinRoomOptions and pass as an argument in the joinRoom method. The joinRoom method resolves with an array of MediaStreams.

- To access the camera and microphone, set `audio: true` and `video: true` in `joinRoomOptions`.

- Refer to the full list of joinRoom options here: [joinRoomOptions](http://cdn.temasys.io/skylink/skylinkjs/2.x/docs/global.html#joinRoomOptions)

```javascript
    const joinRoomOptions =  {
      audio:  true,
      video:  true,
    };
    
    skylink.joinRoom(joinRoomOptions)
    .then((streams)  =>  {
      // if there is an audio stream
      if  (streams[0])  {
        window.attachMediaStream(audioEl, streams[0]);
      }
      // if there is a video stream
      if  (streams[1])  {
        window.attachMediaStream(videoEl, streams[1]);
      }
    })
    .catch();
```

## **Step 6: Subscribing and Unsubscribing to events**

### Subscribing to events

i) Add a listener for the `PEER_JOINED` event.
- The [**PEER_JOINED**](http://cdn.temasys.io/skylink/skylinkjs/latest/docs/SkylinkEvents.html#.event:peerJoined) event is fired when a peer has successfully joined the room. 
- The peer can be yourself (the local peer) or a remote peer. The `isSelf` property in the event payload indicates if the peer is yourself (`isSelf=true`) or the remote peer (`isSelf=false`). 
- Other important values in the event payload are: 
    - `peerId` - A unique identifier for the peer
    - `peerInfo` - Information about the peer
- In the example below, we create a new video element for the remote peer and use the peerId to identify this element in the DOM of our website.

```javascript
    SkylinkEventManager.addEventListener(SkylinkConstants.EVENTS.PEER_JOINED, (evt) => {
        const { isSelf, peerId, peerInfo } = evt.detail;
        if (isSelf) {
            return; // We already have a video and audio element for our video and audio and don't need to create a new one.
        }
     
     
        const vid = document.createElement('video');
        vid.autoplay = true;
        vid.id = peerId + '_video';
        vid.setAttribute('playsinline', true);
        document.body.appendChild(vid);
     
        const aud = document.createElement('audio');
        aud.autoplay = true;
        aud.setAttribute('playsinline', true);
        aud.id = peerId + '_audio';
        document.body.appendChild(aud);
    });
```

ii) To receive local and remote MediaStreams, add a listener for the `ON_INCOMING_STREAM` event.
- The [**ON_INCOMING_STREAM**](http://cdn.temasys.io/skylink/skylinkjs/latest/docs/SkylinkEvents.html#.event:onIncomingStream) event is fired after `PEER_JOINED` when SkylinkJS begins to receive the audio and video streams from a peer. 
- This peer could be yourself (the local peer) – in which case the event is fired when you grant access to your microphone and/or camera and join a room successfully.
- In the example below, we use the _attachMediaStream()_ function of our enhanced AdapterJS library to feed this stream into our previously created video tag in Step 2. We use _attachMediaStream() because the different browser vendors have slightly different ways to do this and _attachMediaStream()_  enables us to abstract this.
- The incoming stream will be either an audio stream or a video stream but not both. The type of stream is indicated by the `isAudio` and `isVideo` property in the event payload.

```javascript
    SkylinkEventManager.addEventListener(SkylinkConstants.EVENTS.ON_INCOMING_STREAM, (evt) => {
        const { isSelf, stream, peerInfo, isVideo, isAudio } = evt.detail;
        if (isSelf) {
            return; // We already attached our local audio and video streams from the resolved promise in joinRoom.
        }
     
        if(isAudio) {
            const aud = document.getElementById(peerId + '_audio');
            attachMediaStream(aud, stream);
        }
        if(isVideo) {
            const vid = document.getElementById(peerId + '_video');
            attachMediaStream(vid, stream);
        }
    });
```

iii) To handle peers leaving the room, add a listener for the `PEER_LEFT` event.
- The [**PEER_LEFT**](http://cdn.temasys.io/skylink/skylinkjs/latest/docs/SkylinkEvents.html#.event:peerLeft) event informs you that a peer has left the room.
- In our example, we look in the DOM for the video element corresponding to the `peerId` in the `PEER_LEFT` event payload and remove it.

```javascript
    SkylinkEventManager.addEventListener(SkylinkConstants.EVENTS.PEER_LEFT, (evt) => {
        const { isSelf, peerInfo, peerId } = evt.detail;
        const videoElId = isSelf ? 'myvideo' : peerId + '_video';
        const audioElId = isSelf ? 'myaudio' : peerId + '_audio';
        const vid = document.getElementById(videoElId);
        const audio = document.getElementById(audioElId);
        document.body.removeChild(vid);
        document.body.removeChild(aud);
    });
```

### Unsubscribing to events

- Unsubscribe to the peer joined event with the code below.

```javascript
    SkylinkEventManager.removeEventListener(SkylinkConstants.EVENTS.PEER_JOINED, peerJoinedHandler);
```

### Logging

- Toggle console logging on and off with the setLevel method. For more logger options refer to: [SkylinkLogger](http://cdn.temasys.io/skylink/skylinkjs/latest/docs/SkylinkLogger.html)

```javascript
    SkylinkLogger.setLevel(SkylinkLogger.logLevels.DEBUG, storeLogs);
```
## **Step 7: Get ready to impress! Your app is ready to test.**

You’ve created a simple video conference app and are on your way to explore all the ways that you can real-time interactions to your website.  

You can find an overview of all the methods and events Skylink offers in the [SDK documentation.](https://cdn.temasys.io/skylink/skylinkjs/latest/docs/index.html)

You can find a CodePen example that we’ve created that shows how you can create a very simple audio/video conference with JavaScript client-side code, with no additional server code required. See the Pen [WebRTC Audio/Video conference demo with Temasys SkylinkJS](https://codepen.io/temasys/pen/GogabE/) by Temasys ([@temasys](https://codepen.io/temasys)) on [CodePen](https://codepen.io/search/pens?q=temasys).

We hope you’ve enjoyed getting started with the Temasys Web SDK! Have fun, share this and let us know if you run into any [issues!](http://support.temasys.io/)

### **ADDITIONAL RESOURCES**

-   [Temasys Developer Console](https://console.temasys.io/)
-   [Skylink API Documentation](https://cdn.temasys.io/skylink/skylinkjs/latest/docs/index.html)
-   [Temasys SkylinkJS Releases](https://github.com/Temasys/SkylinkJS/releases)
-   [Temasys SkylinkJS source code on Github](http://github.com/Temasys/SkylinkJS) (Reference Code can be found [here](https://github.com/Temasys/SkylinkJS/tree/2.x.x/master/demos/collection))
-   [Sample Codepen](https://codepen.io/temasys/pen/GogabE)
-   [How to get support or contribute](https://temasys.io/knowledge-center/)






