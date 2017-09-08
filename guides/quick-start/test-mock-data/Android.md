# Test with Mock Data
Now that we've got the Driving Engine setup in our app, we need some test drive data to ensure it's working. In this section, we'll feed the Driving Engine mock data to make sure it works.

## Mock Data Files
With the SDK, you're provided with two files in the `Data` folder, `MockActivity.txt` and `MockLocation.txt`. Store these files on your phone and note your file paths. For the purposes of this guide, we'll hard code these locations to the Download folder on the phone:

```java
final String MOCK_ACTIVITY_PATH = "/storage/emulated/0/Download/MockActivity.txt";
final String MOCK_LOCATION_PATH = "/storage/emulated/0/Download/MockLocation.txt";
```

## Modify MainActivity
Add the following function to your `MainActivity`:

```java
private void startMockTrip(double cadence) {
    DEMDrivingEngineManager.getInstance().startMockTrip(MOCK_ACTIVITY_PATH, MOCK_LOCATION_PATH, true, cadence);
}
```

Add a call to this in your `onCreate`:

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    // set context for the Driving Engine
    DEMDrivingEngineManager.setContext(this.getApplicationContext());

    // register this activity as a listener
    registerDriveEngineListener();

    // start Driving Engine
    DEMDrivingEngineManager.getInstance().startEngine();

    // start mock trip
    startMockTrip(0.3);
}
```

The `cadence` that you set for `startMockTrip` will determine how fast the engine processes each GPS point. For example, a value of `0.5` would correspond to a half second between points where a value of `1.0` would be real time.

## Test the application
Since there isn't any UI on this app, we'll monitor `logcat` to ensure the mock trip is working. After you've built and deployed the app to your phone, run the following to monitor logs from the app in your Terminal / Command Prompt:

```bash
adb logcat -c
adb logcat ActivityManager:I DriveEngineSample:D *:S
```

If you don't see a trip start message after 15 seconds, you may need to close and restart the app on your phone. Over the next 5 minutes, you'll see the following messages show up:

```
08-14 13:09:33.040 14868 14868 D DriveEngineSample: Listeners Registered
08-14 13:09:36.915 14868 14965 D DriveEngineSample: Trip Recording Started
08-14 13:09:38.721 14868 14966 D DriveEngineSample: Trip Recording Started - Initial Draft
08-14 13:10:28.279 14868 14966 D DriveEngineSample: Braking Detected
08-14 13:10:39.056 14868 14966 D DriveEngineSample: Acceleration Detected
08-14 13:12:00.208 14868 14966 D DriveEngineSample: Trip Saved
```

## Next Steps
Great, you've got some test data working! Next, we'll learn to [interpret the trip summaries](../../reference/interpret-trip-summary.md).