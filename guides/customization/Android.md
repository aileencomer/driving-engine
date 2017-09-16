## Customizing the SDK

## Filter Drive Events

Using event capture masks, you can filter for only the type of drive events that you care about. Each event capture mask corresponds to one of the callbacks found on the `DEMDrivingEngineManager.EventListener` interface.

Because event capture masks are performed as bitwise operations, you may use any ternary operators to string together multiple events. 

For example, if you wanted to only capture trip start and stop,

```java
DEMDrivingEngineManager.getInstance().registerForEventCapture(DEMEventCaptureMask.RECORDING_STARTED_WITH_DATA|DEMEventCaptureMask.RECORDING_STOPPED);
```

You can find a full listing of event capture masks [here](../reference/available-callbacks.md)

## Access Raw GPS Trails

If you'd like finer granularity than trip summaries, you can access raw GPS and activity data from the SDK. You can do this by enabling Developer Mode and Raw Data capture as shown below.

```java
private void configureSDK() {
    // set application path for raw data storage
    DEMDrivingEngineManager.getInstance().setApplicationPath(getApplicationContext().getFilesDir().getAbsolutePath());

    // Read current configuration.
    DEMConfiguration demConfiguration = DEMConfiguration.getConfiguration();

    // To collect trip's raw data on device storage in developer mode
    demConfiguration.setRawDataEnabled(true);

    // To enable developer mode, true by default
    demConfiguration.setEnableDeveloperMode(true); 

    // Store configuration
    DEMDrivingEngineManager.getInstance().setConfiguration(demConfiguration);
}
```

## Modify the Speed Limit for Speeding Events
You can programmatically change the speed limit at which start and end of speeding events are fired.

```java
func configureDriveEngine() {
    // Read current configuration
    DEMConfiguration demConfiguration = DEMConfiguration.getConfiguration();
    
    // set speed limit
    demConfiguration.setSpeedLimit(40);

    // Store configuration
    DEMDrivingEngineManager.getInstance().setConfiguration(demConfiguration);
}
