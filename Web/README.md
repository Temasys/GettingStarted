
# Getting Started with the Temasys SDK for Web

## **A step-by-step guide to embedding Real-Time Communication features into your webapp or website**

This document is designed to help developers get started using the Temasys SDK for the Web to add video & voice calling, secure messaging, file sharing and screen sharing features to any website.

## **Step 1: Create an Account on the Temasys Console and Generate an App Key**

[Here are instructions for creating an account and generating an App Key.](https://temasys.io/creating-an-account-generating-a-key/)

## **Step 2: Include Temasys SkylinkJS code in your website**

### Include Video and Audio Elements in HTML file

    <html>
      <head>
        <title>WebRTC with Temasys SkylinkJS</title>
        <script><script>
      </head>
      <body>
        <video id="myvideo" style="transform: rotateY(-180deg);" autoplay playinsline muted</video>
        <audio id="myaudio" autoplay playsinline muted</audio>
      </body>
    </html>

The body contains a video tag to attach the video stream from the camera and an audio tag to attach the audio stream. We use a CSS transform call to mirror the image, so it looks and feels more natural. We mute the audio, so you don’t hear yourself speaking. The autoplay attribute is needed in some browsers where there are restrictions on autoplay.  [Autoplay policy changes](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes)

## **Step 3: Include dependencies**

### Include socket.io in script tag

    <script  src="./path/to/socket.io.js"></script>

socket.io.js can be obtained from [here](https://cdnjs.cloudflare.com/ajax/libs/socket.io/2.2.0/socket.io.js). Check that the version is 2.2.0.

## **Step 4: Import the SDK**

### Import from a path

#### I) IMPORT IN MAIN.JS

     import Skylink,  { SkylinkEventManager, SkylinkLogger, SkylinkConstants } from './path/to/skylink.complete.js'

Obtain the latest Skylink code  [here](https://cdn.temasys.io/skylink/skylinkjs/latest/skylink.complete.js).

#### II) INCLUDE AS A SCRIPT TAG WITH TYPE AS ‘MODULE’ IN INDEX.HTML

     <script  src="./path/to/skylink.complete.js"  type="module"></script>

## **Step 5: Initialise and then listen for events**

### Instantiate Skylink and init with config

For appKey: ‘XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX’, use the appKey you generated in the Temasys Console.

Full list of init configuration options here: [initOptions](http://cdn.temasys.io/skylink/skylinkjs/2.x/docs/global.html#initOptions)

    const config =  {
    appKey:  'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX',
    defaultRoom:  'skylinkRoom',
    };
    
    skylink =  new Skylink(config);

### Declare joinRoomOptions and pass as argument in joinRoom method. MediaStreams can be obtained from the resolved Promise.

Configure by setting audio: true and video: true to access camera and microphone.

Full list of joinRoom config options here: [joinRoomOptions](http://cdn.temasys.io/skylink/skylinkjs/2.x/docs/global.html#joinRoomOptions)

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

## **Step 6: Subscribing and Unsubscribing to events**

### Listen on ‘peerJoined’ event with isSelf=true for self join room success outcome

[**peerJoined**](http://cdn.temasys.io/skylink/skylinkjs/latest/docs/SkylinkEvents.html#.event:peerJoined)**:** informs you that a peer has joined the room and shares their peerID and peerInfo with you. In this example, we create a new video element for this peer and use the peerId to identify this element in the DOM of our website.


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

### Listen on ‘incomingStream’ event with isSelf=true for self MediaStream

Skylink 2.x incoming stream will be either an audio stream or a video stream but not both.

[**incomingStream**](http://cdn.temasys.io/skylink/skylinkjs/latest/docs/SkylinkEvents.html#.event:onIncomingStream)**:** This event is fired after peerJoined when Temasys SkylinkJS begins to receive the audio and video streams from that peer. This peer could also be yourself (the local peer) – in which case the event is fired when you grants access to your microphone and/or camera and joins a room successfully. In this example, we use the _attachMediaStream()_ function of our enhanced AdapterJS library to feed this stream into our previously created video tag in Step 2. We use _attachMediaStream() because t_he different browser vendors have slightly different ways to do this and _attachMediaStream()_  enables us to abstract this.

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

### Listen on ‘peerLeft’ event to handle peers leaving the room

[**peerLeft**](http://cdn.temasys.io/skylink/skylinkjs/latest/docs/SkylinkEvents.html#.event:peerLeft)**:**  informs you that a peer has left the room. In our example, we look in the DOM for the video element corresponding to the peerId in the peerLeft event payload and remove it. Subscribe to the peer left event with the code below.

    SkylinkEventManager.addEventListener(SkylinkConstants.EVENTS.PEER_LEFT, (evt) => {
        const { isSelf, peerInfo, peerId } = evt.detail;
        const videoElId = isSelf ? 'myvideo' : peerId + '_video';
        const audioElId = isSelf ? 'myaudio' : peerId + '_audio';
        const vid = document.getElementById(videoElId);
        const audio = document.getElementById(audioElId);
        document.body.removeChild(vid);
        document.body.removeChild(aud);
    });

### Unsubscribing to events

Unsubscribe to the peer joined event with the code below.

    SkylinkEventManager.removeEventListener(SkylinkConstants.EVENTS.PEER_JOINED, peerJoinedHandler);

### Logging

Toggle console logging on and off with the setLevel method. For more logger options refer to: [SkylinkLogger](http://cdn.temasys.io/skylink/skylinkjs/latest/docs/SkylinkLogger.html)

    SkylinkLogger.setLevel(SkylinkLogger.logLevels.DEBUG, storeLogs);

## **Step 7: Get ready to impress!**

You’ve created a simple video conference app. Easy, right? Now explore all the ways that you can real-time interactions to your website. You can find an overview of all the methods and events Skylink offers (like lockRoom, disableAudio or disableVideo) in the [API documentation.](https://cdn.temasys.io/skylink/skylinkjs/latest/docs/index.html)

Here is an example Codepen that we’ve created that shows how you can create a very simple audio/video conference with JavaScript client-side code, with no additional server code required. See the Pen [WebRTC Audio/Video conference demo with Temasys SkylinkJS](https://codepen.io/temasys/pen/GogabE/) by Temasys ([@temasys](https://codepen.io/temasys)) on [CodePen](https://temasys.io/temasys-rtc-getting-started-web-sdk/www.codepen.io).

We hope you’ve enjoyed getting started with the Temasys Web SDK! Have fun, share this and let us know if you run into any [issues!](http://support.temasys.io/)

### **ADDITIONAL RESOURCES**

-   [Temasys Developer Console](https://console.temasys.io/)
-   [Skylink API Documentation](https://cdn.temasys.io/skylink/skylinkjs/latest/docs/index.html)
-   [Temasys SkylinkJS Releases](https://github.com/Temasys/SkylinkJS/releases)
-   [Temasys SkylinkJS source code on Github](http://github.com/Temasys/SkylinkJS) (Reference Code[here](https://github.com/Temasys/SkylinkJS/tree/2.x.x/master/demos/collection))
-   [Sample Codepen](https://codepen.io/temasys/pen/GogabE)
-   [How to get support or contribute](https://temasys.io/support)






