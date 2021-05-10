SkylinkCPP is a C++ SDK to use the Temasys platform.

## Getting the SDK

Documentation can be found here : https://temasys-cdn.s3.amazonaws.com/skylink/skylinksdk/cpp/latest/doc/html/index.html

You can download the latest SDK from : https://temasys-cdn.s3.amazonaws.com/skylink/skylinksdk/cpp/latest/skylinkcpp.tar.gz

Find previous version in the release notes.

## Getting an application key

Please visit [temasys.io](https://temasys.io/) for information about the platform, and [console.temasys.io](https://console.temasys.io/) to get an application key.

## Compatibilty

Currently supported on Linux platform only, future releases for Mac and Windows are planned.

## Dependencies

The static library depends on:
- libboost 1.65.0
- libcurl 7.59.0

## Examples

A minimal implentation sample is available on Github : https://github.com/Temasys/skylinkcpp-demo
(Additional information present in code comments)

## Known issues and limitations

We are aware of the following limitation of the current version:
- No error reporting system
- No DataChannel messaging support (P2P messages), use standard signaling channel messaging instead.
- No local screen capture

These issues will be corrected in future releases. 
