# Skylink React Native SDK


### Pre-Requisites

- A Temasys Platform Console Account, App and an App Key ID (Please refer to Steps 1 -4 here: https://temasys.io/temasys-rtc-getting-started-web-sdk)
- Install node.js (Min Version 10.x)
- Install the following:
   * Android: Install and update Android Studio: https://developer.android.com/studio/index.html
   * Android: React Native WebRTC Installation: https://github.com/react-native-webrtc/react-native-webrtc/blob/master/Documentation/AndroidInstallation.md
   * iOS: Install and update Xcode: https://developer.apple.com/xcode/
   * iOS: React Native WebRTC Installation: https://github.com/react-native-webrtc/react-native-webrtc/blob/master/Documentation/iOSInstallation.md

### Installation
Import WebRTC methods from react-native-webRTC node module and pass those to the window object.
```sh
import React, { Component } from 'react';
import {
  Button,
  Dimensions,
  Platform, StyleSheet, Text, View
} from 'react-native';


//import CryptoJS to generate a secure token to connect with the Temasys Platform
import CryptoJS from 'crypto-js';


//import the Skylink react SDK
import { Skylink } from './skylink_react_complete';

const skylink = new Skylink();
```

```sh
//subscribe to Skylink events on the componentDidMount() lifecycle function

componentDidMount() {
  const self = this;
  skylink.on('incomingStream', (peerId, stream, isSelf, peerInfo, isScreensharing, streamId) => {
    if (isSelf) {
      return;
    }
    const url = stream.toURL();
    self.setState({
      remoteStreamURL: url
    })
  });
  skylink.on('peerLeft', peerID => {
    console.log('this peer peerLeft');

  });
  skylink.on('peerJoined', (peerId, peerInfo, isSelf) => {
    console.log("new peer has joined,", peerId, peerInfo, isSelf);
  });
}
```

```sh
//skylink init and joinRoom


skylink.init(config, (error, success) => {
    skylink.joinRoom({
      audio: true,
      video: true
    });
  });

  skylink.on('mediaAccessSuccess', stream => {
    console.log('mediaAccessSuccess');
    const url = stream.toURL();
    self.setState({
      localStreamURL: url
    })
  });
}
```
### RTCView
Please note that rendering the video stream should be done in the React way as shown below.
```sh
//Rendering RTCView
<RTCView streamURL={this.state.stream.toURL()}/>
```

### Limitations
 - MCU Keys are not supported with this version of Skylink RN