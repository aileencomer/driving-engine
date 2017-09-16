# Integrating the Library
The iOS version of the Driving Engine SDK is delivered as a `Framework (.framework)` for iOS. With a couple steps, you can quickly get up and running with the Driving Engine SDK in your project.

## Import the Library
In the `General` page for the project settings under `Embedded Binaries`, add a new binary. Locate the SDK, `CoreEngine.framework` in either the `release` (for devices) or `universal` (for emulators) folder and add it. Ensure that you select `Copy items if needed` when doing this. The library should show up under both `Embedded Libraries` and `Linked Frameworks and Binaries`.

## Permissions and Declarations
In your `Info.plist`, you'll need to add several keys for location prompts and to notify iOS that you'll be using background location services.

In the source view:

```xml
<key>NSLocationUsageDescription</key> 
<string>Press Allow</string> 
<key>NSLocationAlwaysUsageDescription</key> 
<string>Press Allow</string> 
<key>NSLocationWhenInUseUsageDescription</key> 
<string>Press Allow</string> 
<key>NSMotionUsageDescription</key>
<string>Press Allow</string>
<key>UIBackgroundModes</key> 
<array>
    <string>location</string>
</array>
```

You'll want to change these description entries to describe to a user why you'll be accessing their location.

## Next Steps
You should now be able to build your application without any errors. If that's working, let's move on to the next step and [setup the Driving Engine](../setup-drive-engine/iOS.md)
