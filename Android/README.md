**The SkylinkSDK for Android lets you build real time webRTC applications with voice calling, video chat, screen sharing, P2P file sharing or data and messages exchange. Go multi-platform with our [Web](https://github.com/Temasys/GettingStarted/tree/main/Web) and [iOS](https://github.com/Temasys/GettingStarted/tree/main/iOS) SDKs.**

## How to implement the SkylinkSDK in your app

### Step-by-step guide

#### STEP 1  

**Add the SDK to your project**

With Android Studio, you just need to do the following to start using the SDK.

#### Set Gradle configuration to download and use Skylink SDK

Gradle configuration changes depending on the version of Gradle that you use in your Android Studio project.

Version of Gradle that you use can be found in top-level build.gradle file.

Example :

``` gradle
buildscript {
    repositories {
        google()
        mavenCentral()
    }
    dependencies {
        classpath "com.android.tools.build:gradle:7.2.1"
    }
}
```

In this case, Gradle version is 7.2.1. 

##### For newer versions of Gradle (version 7.0.0 and above)

Add repository to download Skylink SDK to settings.gradle file.

``` gradle
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()

        // Repository to download Skylink SDK
        maven {
            url = 'https://archiva.temasys.io/repository/internal'
        }
    }
}
```

Then add Skylink SDK under the dependencies tag in your app's build.gradle file.

``` gradle
dependencies {
    // Other dependencies comes here...

    // Skylink SDKs
    implementation(group: 'sg.com.temasys.skylink.sdk',
            name: 'skylink_sdk',
            version: '2.3.1-RELEASE',
            ext: 'aar') {
        transitive = true
        exclude group: 'sg.com.temasys.skylink.sdk', module: 'skylink_message_cache_sdk'
    }
    implementation('sg.com.temasys.skylink.sdk:skylink_message_cache_sdk:1.0.1-RELEASE')
}
```

##### For older versions of Gradle (version 4.2.x and below)

Add repository to download Skylink SDK under the repositories tag in your app's build.gradle file.

Then add Skylink SDK under the dependencies tag in your app's build.gradle file.

``` gradle
android {
    // Other configurations comes here...

    compileOptions {
        sourceCompatibility 1.8
        targetCompatibility 1.8
    }
}

repositories {
    // Repository to download Skylink SDK
    maven {
        url = 'https://archiva.temasys.io/repository/internal'
    }
}

dependencies {
    // Other dependencies comes here...

    // Skylink SDKs
    compile(group: 'sg.com.temasys.skylink.sdk',
            name: 'skylink_sdk',
            version: '2.3.1-RELEASE',
            ext: 'aar') {
        transitive = true
        exclude group: 'sg.com.temasys.skylink.sdk', module: 'skylink_message_cache_sdk'
    }
    compile('sg.com.temasys.skylink.sdk:skylink_message_cache_sdk:1.0.1-RELEASE')
}
```

**Note the Android SDK version required for the Skylink SDK used**

    Temasys Android SDK : 2.3.1
    Skylink Message Cache SDK : 1.0.1
    Minimum API Level required : 16  
    Targeted Android version : 29  


#### STEP 2  
Follow the instructions [here](https://github.com/Temasys/GettingStarted) to generate your application key and secret.

#### STEP 3  

Implement Listeners in the Class which needs to receive the events sent from the SDK, for example:

``` java
/***
* This class is responsible for implementing all SkylinkListeners for common use of all demos/functions and directly works with SkylinkSDK.
* In case user does not want to implement a specific demo/function, no need to implement corresponding listener(s).
*/
public abstract class SkylinkCommonService implements LifeCycleListener, MediaListener, OsListener, RemotePeerListener, MessagesListener,
    DataTransferListener, FileTransferListener, RecordingListener {

        .....
        /**
        * Implementation of callbacks provided by the listeners
        */

        .....
        .....
}
```

List of Listeners and the callbacks they provide can be found [here](http://cdn.temasys.io/skylink/skylinksdk/android/latest/doc/reference/sg/com/temasys/skylink/sdk/listener/package-summary.html).


For more information on the SDK usage, please refer to the [Sample Application](https://github.com/Temasys/skylink-android-sample).
The sample app uses the latest SkylinkSDK for Android. You will just have to download, import to Android Studio, build and run.

#### Always implement [LifeCycleListener](https://cdn.temasys.io/skylink/skylinksdk/android/latest/doc/reference/sg/com/temasys/skylink/sdk/listener/LifeCycleListener.html) and [RemotePeerListener](https://cdn.temasys.io/skylink/skylinksdk/android/latest/doc/reference/sg/com/temasys/skylink/sdk/listener/RemotePeerListener.html)

In addition to that, depending on the functionality you wish to achieve, add the respective listener(s).  

1. For Audio and/or Video Call : Implement [OsListener](https://cdn.temasys.io/skylink/skylinksdk/android/latest/doc/reference/sg/com/temasys/skylink/sdk/listener/OsListener.html) and [MediaListener](https://cdn.temasys.io/skylink/skylinksdk/android/latest/doc/reference/sg/com/temasys/skylink/sdk/listener/MediaListener.html)  
2. For Data Transfer : Implement [DataTransferListener](https://cdn.temasys.io/skylink/skylinksdk/android/latest/doc/reference/sg/com/temasys/skylink/sdk/listener/DataTransferListener.html)  
3. For File Transfer : Implement [OsListener](https://cdn.temasys.io/skylink/skylinksdk/android/latest/doc/reference/sg/com/temasys/skylink/sdk/listener/OsListener.html) and [FileTransferListener](https://cdn.temasys.io/skylink/skylinksdk/android/latest/doc/reference/sg/com/temasys/skylink/sdk/listener/FileTransferListener.html)  
4. For Messaging : Implement [MessagesListener](https://cdn.temasys.io/skylink/skylinksdk/android/latest/doc/reference/sg/com/temasys/skylink/sdk/listener/MessagesListener.html)  
5. For Android OS/permission related : Implement [OsListener](https://cdn.temasys.io/skylink/skylinksdk/android/latest/doc/reference/sg/com/temasys/skylink/sdk/listener/OsListener.html)  
6. For Recording : Implement [RecordingListener](https://cdn.temasys.io/skylink/skylinksdk/android/latest/doc/reference/sg/com/temasys/skylink/sdk/listener/RecordingListener.html)  

#### Please note that when targeting Android 6 (API 23) and above, you will need to implement [OsListener](https://cdn.temasys.io/skylink/skylinksdk/android/latest/doc/reference/sg/com/temasys/skylink/sdk/listener/OsListener.html) to handle Android Runtime permissions.

   In addition to the methods in OsListener, also override the Android method onRequestPermissionsResult.  
   In short, Android Runtime permissions required by SDK will be prompted to app via OsListener#onPermissionRequired.  
   App should check if permission has already been granted, and if not, request it from the user.  
   Once granted or denied via the Android system, app should call SkylinkConnection#processPermissionsResult. The SDK will process this result and also notify app via OsListener#onPermissionGranted or OsListener#onPermissionDenied.  

   Apps that target below Android API 23 **MUST NOT** implement OsListener.

   Refer to the classes below in the Sample App for more details on implementing these methods.
   There are standardised methods in PermissionUtils that each Fragment makes use of to implement each of these methods.

        1. AudioFragment: For Audio permissions.
        2. VideoFragment: For Audio and Video permissions.
        3. MultiVideosFragment: For Audio and Video permissions.
        4. FileTransferFragment: For external storage read and write permissions.

#### Initialize SkylinkConfig to specify what features are required from the SDK

``` java
/**
    * Get the config for video function
    * User can custom video config by using SkylinkConfig
    */
@Override
public SkylinkConfig getSkylinkConfig() {
    SkylinkConfig skylinkConfig = new SkylinkConfig();
    // VideoCall config options can be:
    // NO_AUDIO_NO_VIDEO | AUDIO_ONLY | VIDEO_ONLY | AUDIO_AND_VIDEO
    skylinkConfig.setAudioVideoSendConfig(SkylinkConfig.AudioVideoConfig.AUDIO_AND_VIDEO);
    skylinkConfig.setAudioVideoReceiveConfig(SkylinkConfig.AudioVideoConfig.AUDIO_AND_VIDEO);
    skylinkConfig.setP2PMessaging(true);
    skylinkConfig.setFileTransfer(true);
    skylinkConfig.setMirrorLocalFrontCameraView(true);
    skylinkConfig.setReportVideoResolutionUntilStable(true);

    // Allow only 1 remote Peer to join as our UI just support 1 remote peer
    skylinkConfig.setMaxRemotePeersConnected(MAX_REMOTE_PEER, SkylinkConfig.AudioVideoConfig.AUDIO_AND_VIDEO);

    // Set the room size
    skylinkConfig.setSkylinkRoomSize(SkylinkConfig.SkylinkRoomSize.EXTRA_SMALL);

    // Set some common configs.
    Utils.skylinkConfigCommonOptions(skylinkConfig);

    //Set default video resolution setting
    String videoResolution = Utils.getDefaultVideoResolution();
    if (videoResolution.equals(VIDEO_RESOLUTION_VGA)) {
        skylinkConfig.setDefaultVideoWidth(SkylinkConfig.VIDEO_WIDTH_VGA);
        skylinkConfig.setDefaultVideoHeight(SkylinkConfig.VIDEO_HEIGHT_VGA);
    } else if (videoResolution.equals(VIDEO_RESOLUTION_HDR)) {
        skylinkConfig.setDefaultVideoWidth(SkylinkConfig.VIDEO_WIDTH_HDR);
        skylinkConfig.setDefaultVideoHeight(SkylinkConfig.VIDEO_HEIGHT_HDR);
    } else if (videoResolution.equals(VIDEO_RESOLUTION_FHD)) {
        skylinkConfig.setDefaultVideoWidth(SkylinkConfig.VIDEO_WIDTH_FHD);
        skylinkConfig.setDefaultVideoHeight(SkylinkConfig.VIDEO_HEIGHT_FHD);
    }

    return skylinkConfig;
}
```

**There are four kinds of AudioVideoConfig**

    01. SkylinkConfig.AudioVideoConfig.NO_AUDIO_NO_VIDEO
    02. SkylinkConfig.AudioVideoConfig.AUDIO_ONLY
    03. SkylinkConfig.AudioVideoConfig.VIDEO_ONLY
    04. SkylinkConfig.AudioVideoConfig.AUDIO_AND_VIDEO


#### Initialize SkylinkConnection object using SkylinkConfig object

``` java
SkylinkConnection skylinkConnection;
skylinkConnection = SkylinkConnection.getInstance();
skylinkConnection.init(skylinkConfig, context.getApplicationContext(), new SkylinkCallback() {
        @Override
        public void onError(SkylinkError error, HashMap<String, Object> details) {
            String contextDescription = (String) details.get(SkylinkEvent.CONTEXT_DESCRIPTION);
            Log.e("SkylinkCallback", contextDescription);
        }
    }
);

// register respective listeners
skylinkConnection.setLifeCycleListener(this);
skylinkConnection.setRemotePeerListener(this);
skylinkConnection.setMediaListener(this);
skylinkConnection.setOsListener(this);
```

#### Connect to a room using Skylink SDK - using the App key and secret obtained from the Temasys Console.

``` java
// you will be connected to the room named "roomName" using a user name or user data JSONObject.
String ROOM_NAME = "roomName";
String MY_USER_NAME = "userName";

skylinkConnection.connectToRoom(Config.getAppKey(), Config.getAppKeySecret(), ROOM_NAME, MY_USER_NAME,
    new SkylinkCallback() {
        @Override
        public void onError(SkylinkError error, HashMap<String, Object> details) {
            String contextDescription = (String) details.get(SkylinkEvent.CONTEXT_DESCRIPTION);
            Log.e("SkylinkCallback", contextDescription);
        }
    }
);
```

#### Connect to a room using Skylink SDK - using a secured connection string (recommended to use in production, refer to sample app for more details)

``` java
// SkylinkConnectionString generated with room name, appKey, secret, startTime, duration and roomSize
skylinkConnection.connectToRoom(skylinkConnectionString, mUserName, new SkylinkCallback() {
        @Override
        public void onError(SkylinkError error, HashMap<String, Object> details) {
            String contextDescription = (String) details.get(SkylinkEvent.CONTEXT_DESCRIPTION);
            Log.e("SkylinkCallback", contextDescription);
        }
    }
);
```

#### Verify if the connection works by logging on callback

``` java
/***
* Lifecycle Listener Callbacks -- triggered on events that happen during the SDK's lifecycle
*/

/**
    * This is the first callback from the SkylinkSDK to specify whether the attempt to connect to the room was successful.
    */
@Override
public void onConnectToRoomSucessful(){
    Log.d(TAG, "onConnectToRoomSucessful");
    // do some logic or update UI to connected state here
    ...
}

/**
    * This is triggered when there is an error upon connecting to the room
    */
@Override
public void onConnectToRoomFailed(String errorMessage){
    Log.d(TAG, "onConnectToRoomFailed(" + errorMessage + ")");
    // do some logic or update UI to disconnected state here
    ...
}

/**
    * This method is triggered from the SkylinkSDK to inform whether or not the user has been successfully disconnected from the room
    */
@Override
public void onDisconnectFromRoom(SkylinkEvent skylinkEvent, String contextDescription){
    Log.d(TAG, "onDisconnectFromRoom(" + skylinkEvent + ", message: " + contextDescription + ")");
    // do some logic or update UI to disconnected state here
    ...
}

...

/**
    * This is triggered from the SkylinkSDK to deliver messages that may be useful to the user.
    */
@Override
public void onReceiveInfo(SkylinkInfo skylinkInfo, HashMap<String, Object> details) {
    String contextDescriptionString = (String) details.get(CONTEXT_DESCRIPTION);
    Log.d(TAG, "onReceiveInfo(skylinkInfo: " + skylinkInfo.toString() + ", details: " + contextDescriptionString);
    ...
}

/**
    * This is triggered from the SkylinkSDK to deliver a warning message to the user
    */
@Override
public void onReceiveWarning(SkylinkError skylinkError, HashMap<String, Object> details) {
    String contextDescriptionString = (String) details.get(CONTEXT_DESCRIPTION);
    ...
}

/**
    * This is triggered from the SkylinkSDK to deliver an error message to the user
    */
@Override
public void onReceiveError(SkylinkError skylinkError, HashMap<String, Object> details) {
    String contextDescriptionString = (String) details.get(CONTEXT_DESCRIPTION);
    ...
}
```

## Subscribe

Star the [Android Sample App Github](https://github.com/Temasys/skylink-android-sample) repo to be notified of new release tags. You can also view release notes on our [support portal](http://support.temasys.com.sg/en/support/solutions/folders/12000009705).


## Feedback

Please do not hesitate to reach out to us if you encounter any issues or if you have feedback or suggestions on how we can improve Skylink.
You can raise tickets on our [support portal](http://support.temasys.io/).


# How to integrate the Skylink Message Cache SDK into your app

#### Initialize SkylinkConfig to enable message cache feature is required

``` java
SkylinkConfig skylinkConfig = new SkylinkConfig();

// Enable message caching (Message caching is disabled by default in SkylinkConfig)
skylinkConfig.setMessageCachingEnable(true);

// Set maximum number of messages that will be cached per Skylink Room (default value is 50)
skylinkConfig.setMessageCachingLimit(100);
```

#### Get cached messages

``` java
if ( SkylinkMessageCache.getInstance().isEnabled() ) {
    // Get a readable session from the message cache to your room
    JSONArray cachedMsgs = SkylinkMessageCache.getInstance().getReadableSession("<your-room-name>").getCachedMessages();
    
    // Processing cached messages :-
    // Assuming all cached messages are String messages (not JSONObject or JSONArray messages)
    for (int i = 0; i < cachedMsgs.length(); i++) {
        JSONObject cachedMsg = (JSONObject) cachedMsgs.get(i);
        String senderId   = cachedMsg.getString("senderId");
        String msgContent = cachedMsg.getString("data");
        long timestamp    = cachedMsg.getLong("timeStamp");
    }
}
```

#### Clear cached messages (optional)

``` java
// Get a writable session from the message cache to your room and clear cached messages of that room
SkylinkMessageCache.getInstance().getWritableSession("<your-room-name>").clearCachedMessages();

// Clear a specific cached room (including cached room information + cached messages)
SkylinkMessageCache.getInstance().clearRoom("<your-room-name>");

// Clear everything in the message cache (all cached rooms and cached messages)
SkylinkMessageCache.getInstance().clearAll();
```


# Temasys SDK for Android - Sample Application

The Sample Application(SA), which uses the latest SkylinkSDK for Android, demonstrates use of the SkylinkSDK to provide embedded real time communication in the easiest way. Just download, import to Android Studio, build and run!

In the SA, there are simple demos on:

  - Audio
  - Video
  - Chat/Message
  - DataTransfer
  - FileTransfers
  - ScreenShare

# Sample App structure

The architecture of Sample app is the way we organize the code to have a clear structure. We try to separate between the application layer and the SDK usage layer. With the separated parts, the user can easily change each part without changing the others and extend the functionality of the application. For example, the user can using different view components to display GUI of the application while keeping the same logics of using the SDK.

The **MVP (Model - View - Presenter)** architecture used in the Sample App mainly divided into three main parts: View - Presenter - Service

  - View: responsible for displaying GUI and getting user events.
  - Presenter: responsible for processing app logic and implementing callbacks sent from the SkylinkSDK
  - Service: responsible for sending requests to SkylinkSDK, using SkylinkConnection instance to communicate with the SkylinkSDK, the service part also contain the models (M) of the application.

For more details in the Sample app's architecture, please refer to (https://github.com/Temasys/skylink-android-sample/blob/master/SAArch.md)

## How to run the sample project

### Step-by-step guide for Android Studio

#### STEP 1
Clone this repository.

#### STEP 2
Import the project into Android Studio with File -> Open and select the project.

#### STEP 3
Follow the instructions [here](https://github.com/Temasys/GettingStarted) to create an App and a key on the Temasys Console.

#### STEP 4
Create a copy of **config_example.xml** under **res/values** and name it **config.xml** (OR any other name unused in the res/value/ directory).
You may also choose to not create a new file and edit **config_example.xml** itself as needed.

#### STEP 5
Add your preferred values for **app_key** and **app_secret**. An appropriate App key and corresponding secret are required to connect to Skylink successfully.

# Instructions for populating the config.xml file.

SMR stands for Skylink Media Relay. For more information about SMR, see our [FAQs](http://support.temasys.com.sg/support/solutions/12000000313)

  - [What is MCU/SFU/Skylink Media Relay?](http://support.temasys.com.sg/support/solutions/articles/12000047799) and
  - [How can I enable/ disable Skylink Media Relay (MCU) for my App?](http://support.temasys.com.sg/support/solutions/articles/12000047800)

In the Sample App, keys with SMR enabled and those without are kept in 2 separate categories. You may provide a default App Key for each category. If not, simply uncomment the xml elements without changing their values.

Populate the boolean value for **is_app_key_smr** with whether the app should start by selecting **app_key_no_smr (false)** or **app_key_smr (true)** as the App key to use.

- Next, populate the values for **app_key_no_smr** and/or **app_key_smr** with the appropriate app keys.
- Next, populate the values for **app_key_secret_no_smr** and/or **app_key_secret_smr** with the appropriate app secret.
- Next, populate the values for **app_key_desc_no_smr** and/or **app_key_desc_smr** with the appropriate app description.

# Sample Configuration of a config.xml file

Example of config.xml file using a single App Key: 12345678-abc2-abc3-abc4-abc5abc6abc7 with the corresponding secret: 123456789123 with MCU set to OFF on the console:

``` xml
<!-- App Keys and secrets. -->
<!--Uncomment in config.xml-->
<!--Since MCU has been set to OFF on the console, the boolean value of is_app_key_smr has been set to false-->
<bool name="is_app_key_smr">false</bool>

<!--Uncomment in config.xml-->
<!--Enter Details of your App Key for which SMR has been set to false below-->
<string name="app_key_no_smr">12345678-abc2-abc3-abc4-abc5abc6abc7</string>
<string name="app_key_secret_no_smr">123456789123</string>
<string name="app_key_desc_no_smr">Non SMR Key for my awesome application</string>

<!--Uncomment in config.xml-->
<!--Since this is a case where only 1 key has been created with SMR set to false, you need not make any changes to the below-->
<string name="app_key_smr">Any string.</string>
<string name="app_key_secret_smr">Any string</string>
<string name="app_key_desc_smr">Any string</string>
```

### For more examples, you may wish to refer to our guide: [How does the Sample App handle SMR Functionality?](http://support.temasys.com.sg/support/solutions/articles/12000064630)

#### STEP 6
Build the project

#### STEP 7
Run the sampleapp module


# Temasys SDK for Android

## SDK documentation

For more information on the usage of the Temasys SDK for Android, please refer to the following:

 - [Temasys SDK for Android Readme](https://github.com/Temasys/GettingStarted/tree/main/Android)

## Tutorials and FAQs

[FAQS](http://support.temasys.com.sg/support/solutions/12000000313)



## Subscribe

Star this repo to be notified of new release tags. You can also view [release notes on our support portal](http://support.temasys.com.sg/support/solutions/folders/12000009705)

## Feedback

Please do not hesitate to reach get in touch with us if you encounter any issue or if you have any feedback or suggestions on how we could improve the Temasys SDK for Android or Sample Application.
You can raise tickets on our [support portal](http://support.temasys.io/) or on github.


## Copyright and License

Copyright 2019 Temasys Communications Pte Ltd Licensed under [APACHE 2.0](http://www.apache.org/licenses/LICENSE-2.0.html)


# Sample app architecture in MVP

The **MVP (Model - View - Presenter)** architecture used in the Sample App mainly divided into three main parts: **View - Presenter - Service**

  - **View**: responsible for displaying GUI and getting user interaction.
  - **Presenter**: responsible for processing app logic and implementing callbacks sent from the SkylinkSDK
  - **Service**: responsible for sending requests to SkylinkSDK, using SkylinkConnection instance to communicate with the Skylink SDK, the service part also contain the models (M) of the application.

## Class diagram:
https://github.com/Temasys/skylink-android-sample/blob/master/SA_MVP_Class_Diagram.png


## App structure:
  - Put related classes into packages for clearer design:
      + **audio** package: contain classes like AudioCallActivity, AudioCallContract, AudioCallFragment, AudioCallPresenter to implement the audio function.
      + **chat** package: contain classes to implement the chat or message function.
      + **datatransfer** package: contain classes to implement the data transfer function.
      + **filetransfer** package: contain classes to implement the file transfer function.
      + **multivideos** package: contain classes to implement the multi videos function.
      + **video** package: contain classes to implement the video function, including video from camera and video from screen sharing
      + **service** package: contain AudioService, VideoService,...SkylinkCommonService, SkylinkConnectionManager, PermissionService and model package to implement the service tasks of all functions
      + **setting** package: contain classes to implement the setting like user and room setting, application key setting
      + **utils** package: contains some utility classes
      + **videoresolution**: contain classes to implement the video resolution function. The video resolution fragment and presenter can be injected to any other fragments/presenter which contains video view and want to show its video resolution

  - **Model** package: inside the service package and contains all models in the application.
      + **KeyInfo** : encapsulate info about application Key
      + **PermRequesterInfo**: encapsulate info about permission request
      + **SkylinkPeer**: encapsulate info about peer id, peer name and peer's media
      + **VideoResolution**: encapsulate info about video resolution (video width, video height, video frame rate)

## Class details:
  - **BaseView** : a common interface for all views including fragments
  - **BasePresenter**: a common abstract class for all presenters.
                    This class defined all methods which responsible for updating UI requested by the service layer. Some of those which do not need to be override in the concrete classes can be implemented in the BasePresenter (such as just displaying toast to inform changes to user or logging info)
  - **BaseService**: a common interface for all services which are responsible for communicating with Skylink SDK.
  - **Contract(s)**: interfaces to define the public methods of views, presenters, services classes for invoking each others and avoid circle relations between 2 classes
  - **Fragment(s)**: the view instance to display the UI of the app, and get the user interaction like click on buttons, input text,...
  - **Presenter(s)**: the presenter instance to implement all logics of the app such as tell the view to update UI to correct states, using the services to call to API in the SDK,...
  - **Service(s)**: the service instance to communicate with API in the SDK. Only service classes should directly communicate with the SDK to send the request from application, these classes also help the user to custom the skylink configurations by using SkylinkConfig object.
  - **SkylinkCommonService**: an abstract class responsible for implementing all listeners which sent from the SDK, using BasePresenter to update GUI and keep track of peers in room, and singleton SkylinkConnection instance.
  - **SkylinkConnectionManager**: a class responsible for managing connection to the SDK like initializeSkylinkConnection, connectToRoom, disconnectFromRoom, generate SkylinkConnectionString
  - **PermissionService**: responsible for processing runtime permissions required by media/file usages.


# Skylink Message Cache SDK for Android - Sample Application

The application which uses the latest Skylink Message Cache SDK for Android demonstrates the use of persistent message caching feature.

Implementation of the app is divided in to 2 Gradle modules.

* `app` : Implements the UI and app logic
* `skylink_sample` : Contains Java class implementations taken from [skylink-android-sample](https://github.com/Temasys/skylink-android-sample) project.

ChatService implementation is completely taken from the [skylink-android-sample](https://github.com/Temasys/skylink-android-sample) project and modified slightly to enable message caching feature.

Check out [Skylink Message Cache Sample App](https://github.com/Temasys/android-sample-message-cache).


# Skylink Message Cache SDK

This is an addon SDK for the Temasys SDK for Android.

## SDK documentation

* [Skylink Message Cache SDK for Android Reference](https://cdn.temasys.com.sg/skylink/skylinkmessagecachesdk/android/latest/doc/reference/sg/com/temasys/skylink/sdk/messagecache/SkylinkMessageCache.html)

## Tutorials and FAQs

[Skylink Message Cache SDK usage](https://github.com/Temasys/android-sample-message-cache#skylink-message-cache-sdk-usage)

## Feedback

Please raise your tickets in [Github issues](https://github.com/Temasys/skylink-android-sample/issues).

## Copyright and License

Copyright 2022 Temasys Communications Pte Ltd Licensed under [APACHE 2.0](http://www.apache.org/licenses/LICENSE-2.0.html)
