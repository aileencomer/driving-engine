## Customizing the SDK

## Filter Drive Events

Using event capture masks, you can filter for only the type of drive events that you care about. Each event capture mask corresponds to one of the callbacks found on the `DEMDrivingEngineDelegate` protocol.

Because event capture masks are performed as bitwise operations, you may use any ternary operators to string together multiple events. 

For example, if you wanted to only capture trip start and stop,

```swift
driveEngine.register(DEMEventCaptureMask.recordingStartedWithData|DEMEventCaptureMask.recordingStopped)
```

You can find a full listing of event capture masks [here](../reference/available-callbacks.md)

## Access Raw GPS Trails

If you'd like finer granularity than trip summaries, you can access raw GPS and activity data from the SDK. You can do this by enabling Developer Mode and Raw Data capture as shown below.

```swift
func configureDriveEngine() {
    let driveEngine = DEMDrivingEngineManager.sharedManager() as! DEMDrivingEngineManager
    
    // insert configuration here
    let config = DEMConfiguration.sharedManager() as DEMConfiguration
    config.enableWebServices = false

    // To enable developer mode, true by default
    config.enableDeveloperMode = true
    
    driveEngine.setConfiguration(config)
}
```

## Modify the Speed Limit for Speeding Events
```swift
func configureDriveEngine() {
    let driveEngine = DEMDrivingEngineManager.sharedManager() as! DEMDrivingEngineManager
    
    // insert configuration here
    let config = DEMConfiguration.sharedManager() as DEMConfiguration
    config.enableWebServices = false

    // set the speed limit to 40 mph
    config.speedLimit = 40

    driveEngine.setConfiguration(config)
}