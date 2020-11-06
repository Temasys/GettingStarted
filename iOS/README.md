# SkylinkSDK for iOS
[![Version](https://img.shields.io/cocoapods/v/MyLibrary.svg?style=flat)](http://cocoadocs.org/docsets/SKYLINK)  [![License](https://img.shields.io/cocoapods/l/MyLibrary.svg?style=flat)](http://cocoadocs.org/docsets/SKYLINK) [![Platform](https://img.shields.io/cocoapods/p/MyLibrary.svg?style=flat)](http://cocoadocs.org/docsets/SKYLINK)

The **SkylinkSDK for iOS** lets you build real time webRTC applications with voice calling, video chat, P2P file sharing or data and messages exchange. Go multi-platform with our [Web](https://temasys.io/products/sdks/js/) and [Android](https://temasys.io/products/sdks/android/) SDKs.


## Prerequisites
Your project should use [ARC](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html)  

## Minimum iOS version required
iOS 9 or higher.

## How to install the SkylinkSDK for iOS on your app

### Step-by-step guide

##### STEP 1  
You can install the SkylinkSDK for iOS via CocoaPods, or Carthage. If you do not have it installed, follow the below steps:

###### Installing Cocoapods  
Check that you have Xcode command line tools installed (Xcode > Preferences > Locations > Command line tools([?](http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/)). If not, open the terminal and run `xcode-select --install`.
Install cocoa pods in the terminal: `$ sudo gem install cocoapods`
###### Installing Carthage 
Download and run the Carthage.pkg file for the latest [release](https://github.com/Carthage/Carthage/releases), then follow the on-screen instructions. If you are installing the pkg via CLI, you might need to run sudo chown -R $(whoami) /usr/local first

##### STEP 2  
If you are using CocoaPods, add the following line to your Podfile:

    pod "SKYLINK"
    #If facing issues with installation, please use:
    #pod 'SKYLINK', :git => 'https://github.com/Temasys/SKYLINK-iOS.git'
If you are using Carthage, add the following line to your Cartfile:
	
    git "https://github.com/Temasys/SKYLINK-iOS.git"
##### STEP 3  
Follow the instructions [here](https://temasys.io/creating-an-account-generating-a-key/) to create an App and a key on the Temasys Console.



##### STEP 4

To create a Swift project using Teamsys iOS SDK, follow these steps:  
###### If you are using CocoaPods:

Your Podfile should look like that:
	
    project 'SampleApp_Swift.xcodeproj'
    platform :ios, '9.0'
    target 'SampleApp_Swift' do
    use_frameworks!
    pod "SKYLINK"
    #If facing issues with installation, please use:
    #pod 'SKYLINK', :git => 'https://github.com/Temasys/SKYLINK-iOS.git'
    end

In the terminal, run `pod install`
###### If you are using Carthage:
Your Cartfile should look like that:
	
    git "https://github.com/Temasys/SKYLINK-iOS.git"

In the terminal, run `carthage update`

#####Note: If you install by Carthage and the installation is successful, you have to link the frameworks to your project, in Xcode, go to "TARGETS" --> "Frameworks, Libraries, and Embedded Content", click "+", in the prompted window "Choose frameworks and libraries to add:", click "Add Other...", "Add Files...", in the prompted window, choose "Carthage" --> "Checkouts" --> "SKYLINK-iOS" --> "frameworks", select all the four frameworks here, then click open, you will see the four frameworks added into your "Frameworks, Libraries, and Embedded Content" window. After that, build your project, see if there is any error
![image](./1.png)![image](./2.png)![image](./3.png)
Create the `Project-Bridging-Header.h` and refer to it in build settings (swift compiler section)
Add `#import <SKYLINK/SKYLINK.h>` to the newly created file
You should be able to run your project after this, and use Temasys iOS SDK with Swift.

### Configuring Settings

- After running 'pod install', use the .xcworkspace file. Do not work with the .xcodeproj file.
- For each target planned for use with the SkylinkSDK for iOS:  
    go to Build settings  (make sure “all” is selected) >  
    Build Options >   
    Enable bit code and set it to NO.   
    This will avoid the “…does not contain bitcode” message  

- Optionally, if you want your app to be able to process audio even when the users leaves the app or locks the device, just enable the VoIP background capability or the audio background capability in the target’s “capabilities” tab.
- You may need to specify the swift language version in some pod targets. Use Swift 5.

## Start coding !

The SkylinkSDK for iOS is designed to be simple to use. The main idea when using it is to prepare and create a connection to a "room" via the Temasys platform. After that, you will be able to send messages to the connection and implement the desired protocols to control what happens between the local device and the peers connected to the same "room".


## Resources

### References  

See our [Swift](https://github.com/Temasys/SkylinkSDK_iOS_SampleApp_Swift) and [Objective C](https://github.com/Temasys/SkylinkSDK-iOS-Sample) Sample apps for usage instructions and examples.

### Documentation, Guides and FAQs  
[SDK Documentation](https://cdn.temasys.io/skylink/skylinksdk/ios/latest/docs/html/index.html)  
[Getting started with Temasys iOS SDK for iOS](http://temasys.io/getting-started-skylinksdk-ios/)    
[Handle the video view stretching](http://temasys.io/a-simple-solution-for-video-stretching/)    
[FAQs](http://support.temasys.com.sg/support/solutions/12000000562)  


## Subscribe  
Star this repo to be notified of new release tags. You can also view release notes on our [support portal](http://support.temasys.com.sg/en/support/solutions/folders/12000009706)

## Feedback  
Please do not hesitate to reach get in touch with us if you encounter any issue or if you have any feedback or suggestions on how we can improve the Skylink SDK for iOS or Sample Applications. You can raise tickets on our [support portal](http://support.temasys.io/).




# SkylinkSDK iOS SampleApp Objective C

The Sample Application(SA), which uses the latest version of the Skylink SDK for iOS, demonstrates its use to provide embedded real time communication in the easiest way.
Excluding 'Settings', this App has 6 distinct view controllers, each of them demonstrating how to build the following features:

- One to one video call with Screen Share
- Multi party video call
- Multi party audio call
- Chatroom and custom messages
- File transfer
- Data transfer

## Code introduction  
The code should be self explanatory. Each view controller works by itself and there is minimal UI code due to to Storyboard usage. 
In each view controller, the main idea is to **configure and instantiate a connection to a room with the Skylink iOS SDK**. 
You will then be able to communicate with another peer joining the same room.  

##### Sample Code with Video and Audio 

    // Creating configuration
    SKYLINKConnectionConfig *config = [SKYLINKConnectionConfig new];
    [config setAudioVideoReceiveConfig:AudioVideoConfig_AUDIO_AND_VIDEO];
    [config setAudioVideoSendConfig:AudioVideoConfig_AUDIO_AND_VIDEO];
    
    // Creating SKYLINKConnection
    SKYLINKConnection *skylinkConnection = [[SKYLINKConnection alloc] initWithConfig:config callback:nil];
    self.skylinkConnection.lifeCycleDelegate = self;
    self.skylinkConnection.mediaDelegate = self;
    self.skylinkConnection.remotePeerDelegate = self;
	self.skylinkConnection = skylinkConnection;
    
    // Coonnecting to room
    [skylinkConnection connectToRoomWithAppKey:self.skylinkApiKey secret:self.skylinkApiSecret roomName:ROOM_NAME userData:nil callback:nil];


You can then control what happens in the room by **sending messages to the `SKYLINKConnection` instance** (like triggering a file transfer request for example), and **respond to events by implementing the delegate methods** from the 6 protocols.
Always set at least the [lifeCycleDelegate](https://cdn.temasys.io/skylink/skylinksdk/ios/latest/docs/html/Protocols/SKYLINKConnectionLifeCycleDelegate.html). For a list of all protocols, see [here](https://cdn.temasys.io/skylink/skylinksdk/ios/latest/docs/html/index.html)


Aditionally, in each view controller example's viewDidLoad/initWithCoder method, some properties are initialized.
A disconnect button is set in the navigation bar (left corner) as well as its selector implementation (called disconnect). An info button is set on the right corner, as well as its implementation (called showInfos). Those 2 navigation bar buttons selectors are the same in every View Controller example.

The rest of the example view controllers gives you 6 example usages of the Temasys iOS SDK.

## How to run the sample project

### Step-by-step guide

##### Prerequisites  
Please use Xcode 11

##### STEP 1  
It is recommended to install the SkylinkSDK for iOS via [cocoapods](http://cocoapods.org). If you do not have it installed, follow the below steps:

###### Installing Cocoapods  
Check that you have Xcode command line tools installed (Xcode > Preferences > Locations > Command line tools([?](http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/)). If not, open the terminal and run `xcode-select --install`.
Install cocoa pods in the terminal: `$ sudo gem install cocoapods`

##### STEP 2  
Clone the repo or download the project.

##### STEP 3  
Navigate to the Sample App and Run `pod install`

##### STEP 4  
Open the .xcworkspace file

##### STEP 5  
Follow the instructions [here](https://temasys.io/creating-an-account-generating-a-key/) to create an App and a key on the Temasys Console.

##### STEP 6   
Set your App Key and secret in Constant.h. You may also alter the room names here.
    
    NSString *APP_KEY = @"ENTER APP KEY HERE";
    NSString *APP_SECRET = @"ENTER SECRET HERE";

    NSString *ROOM_ONE_TO_ONE_VIDEO = @"ROOM_ONE_TO_ONE_VIDEO";
    NSString *ROOM_MULTI_VIDEO = @"ROOM_MULTI_VIDEO";
    NSString *ROOM_AUDIO = @"ROOM_AUDIO";
    NSString *ROOM_MESSAGES = @"MESSAGES-ROOM";
    NSString *ROOM_FILE_TRANSFER = @"ROOM_FILE_TRANSFER";
    NSString *ROOM_DATA_TRANSFER = @"ROOM_DATA_TRANSFER";


##### STEP 7  
Build and Run. You're good to go!

##### Please Note
The XCode Simulator does not support video calls.  
If you have connected a phone, ensure it is unlocked and the appropriate team is selected under Signing & Capabilities.    

### Resources


##### SDK documentation  
For more information on the usage of the SkylinkSDK for iOS, please refer to [SkylinkSDK for iOS Readme](https://github.com/Temasys/SKYLINK-iOS/blob/master/README.md)

##### Subscribe  
Star this repo to be notified of new release tags. You can also view release notes on our [support portal](http://support.temasys.com.sg/en/support/solutions/folders/12000009706)

##### Feedback  
Please do not hesitate to reach get in touch with us if you encounter any issue or if you have any feedback or suggestions on how we can improve the Skylink SDK for iOS or Sample Applications. You can raise tickets on our [support portal](http://support.temasys.io/).

##### Copyright and License  
Copyright 2019 Temasys Communications Pte Ltd Licensed under APACHE 2.0  


#### Tutorials and FAQs

[Getting started with Temasys iOS SDK for iOS](http://temasys.io/getting-started-skylinksdk-ios/)  
[Handle the video view stretching](http://temasys.io/a-simple-solution-for-video-stretching/)  
[FAQs](http://support.temasys.com.sg/support/solutions/12000000562)
  

Also checkout our Skylink SDKs for [Web](http://skylink.io/web/) and [Android](http://skylink.io/android)

*This document was edited for Temasys iOS SDK version 2.0.0*




# SkylinkSDK iOS SampleApp Swift

The Swift Sample Application(SA), which uses the latest version of the Skylink SDK for iOS, demonstrates its use to provide embedded real time communication in the easiest way.
Excluding 'Settings', this App has 6 distinct view controllers, each of them demonstrating how to build the following features:

- One to one video call with Screen Share
- Multi party video call
- Multi party audio call
- Chatroom and custom messages
- File transfer
- Data transfer

## Code introduction  
The code should be self explanatory. Each view controller works by itself and there is minimal UI code due to to Storyboard usage. 
In each view controller, the main idea is to **configure and instantiate a connection to a room with the Skylink iOS SDK**. 
You will then be able to communicate with another peer joining the same room.  

##### Sample Code  

    // SKYLINKConnection creation for video call room
    lazy var skylinkConnection: SKYLINKConnection = {
        // Creating configuration
        let config = SKYLINKConnectionConfig()
        config.setAudioVideoSend(AudioVideoConfig_AUDIO_AND_VIDEO)
        config.setAudioVideoReceive(AudioVideoConfig_AUDIO_AND_VIDEO)  
        
        // Creating SKYLINKConnection
        if let skylinkConnection = SKYLINKConnection(config: config, callback: nil) {
            skylinkConnection.lifeCycleDelegate = self
            skylinkConnection.mediaDelegate = self
            skylinkConnection.remotePeerDelegate = self
            return skylinkConnection
        } else {
            return SKYLINKConnection()
        }
    }()  
	
    // Use SKYLINKConnection(with your key and secret) to connect to a room
    skylinkConnection.connectToRoom(withAppKey: skylinkApiKey, secret: skylinkApiSecret, roomName: ROOM_NAME, userData: USER_NAME, callback: nil)

You can then control what happens in the room by **sending messages to the `SKYLINKConnection` instance** (like triggering a file transfer request for example), and **respond to events by implementing the delegate methods** from the 6 protocols.
Always set at least the [lifeCycleDelegate](https://cdn.temasys.io/skylink/skylinksdk/ios/latest/docs/html/Protocols/SKYLINKConnectionLifeCycleDelegate.html). For a list of all protocols, see [here](https://cdn.temasys.io/skylink/skylinksdk/ios/latest/docs/html/index.html)


Aditionally, in each view controller example's viewDidLoad/initWithCoder method, some properties are initialized.
A disconnect button is set in the navigation bar (left corner) as well as its selector implementation (called disconnect). An info button is set on the right corner, as well as its implementation (called showInfos). Those 2 navigation bar buttons selectors are the same in every View Controller example.

The rest of the example view controllers gives you 6 example usages of the Temasys iOS SDK.

## Swift Version Supported
Swift 5.1 and Swift 5.1.2

## How to run the sample project

### Step-by-step guide

##### Prerequisites  
Please use Xcode 11

##### STEP 1  
It is recommended to install the SkylinkSDK for iOS via [cocoapods](http://cocoapods.org). If you do not have it installed, follow the below steps:

###### Installing Cocoapods  
Check that you have Xcode command line tools installed (Xcode > Preferences > Locations > Command line tools([?](http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/)). If not, open the terminal and run `xcode-select --install`.
Install cocoa pods in the terminal: `$ sudo gem install cocoapods`

##### STEP 2  
Clone the repo or download the project.

##### STEP 3  
Navigate to the Swift Sample App and Run `pod install`

##### STEP 4  
Open the .xcworkspace file

##### STEP 5  
Follow the instructions [here](https://temasys.io/creating-an-account-generating-a-key/) to create an App and a key on the Temasys Console.

##### STEP 6   
Set your App Key and secret in Constants.swift. You may also alter the room names here.

    var APP_KEY = "ENTER APP KEY HERE"
    var APP_SECRET = "ENTER SECRET HERE"
    var ROOM_ONE_TO_ONE_VIDEO = "VIDEO-CALL-ROOM"
    var ROOM_MULTI_VIDEO = "MULTI-VIDEO-CALL-ROOM"
    var ROOM_AUDIO = "AUDIO-CALL-ROOM"
    var ROOM_MESSAGES = "MESSAGES-ROOM"
    var ROOM_FILE_TRANSFER = "FILE-TRANSFER-ROOM"
    var ROOM_DATA_TRANSFER = "ROOMNAME_DATATRANSFER"
    

##### STEP 7  
Build and Run. You're good to go!

##### Please Note
The XCode Simulator does not support video calls.  
If you have connected a phone, ensure it is unlocked and the appropriate team is selected under Signing & Capabilities.    

### Resources


##### SDK documentation  
For more information on the usage of the SkylinkSDK for iOS, please refer to [SkylinkSDK for iOS Readme](https://github.com/Temasys/SKYLINK-iOS/blob/master/README.md)

##### Subscribe  
Star this repo to be notified of new release tags. You can also view release notes on our [support portal](http://support.temasys.com.sg/en/support/solutions/folders/12000009706)

##### Feedback  
Please do not hesitate to reach get in touch with us if you encounter any issue or if you have any feedback or suggestions on how we can improve the Skylink SDK for iOS or Sample Applications. You can raise tickets on our [support portal](http://support.temasys.io/).

##### Copyright and License  
Copyright 2019 Temasys Communications Pte Ltd Licensed under APACHE 2.0  


#### Tutorials and FAQs

[Getting started with Temasys iOS SDK for iOS](http://temasys.io/getting-started-skylinksdk-ios/)  
[Handle the video view stretching](http://temasys.io/a-simple-solution-for-video-stretching/)  
[FAQs](http://support.temasys.com.sg/support/solutions/12000000562)



Also checkout our Skylink SDKs for [Web](http://skylink.io/web/) and [Android](http://skylink.io/android)

*This document was edited for Temasys iOS SDK version 2.0.0*




