# Test with Mock Data
Now that we've got the Drive Engine setup in our app, we need some test drive data to ensure it's working. In this section, we'll feed the Drive Engine mock data to make sure it works.

## Mock Data Files
With the SDK, you're provided with two files in the `Data` folder, `MockActivity.txt` and `MockLocation.txt`. Store these files on your phone and note your file paths. For the purposes of this guide, we'll embed these files in the app for testing.

On the `Build Phases` section of your project settings, add both files to the `Copy Bundle Resources` build step. As before, ensure you check the `Copy files if needed` option when you locate the files.

## Modify MainActivity
Add the following function to your `ViewController`:

```swift
func startMockTrip(fastMock: Bool, cadence: TimeInterval) {
    let driveEngine = DEMDrivingEngineManager.sharedManager() as! DEMDrivingEngineManager
    let mockLocationData = Bundle.main.path(forResource: "MockLocation", ofType: "txt")
    let mockActivityData = Bundle.main.path(forResource: "MockActivity", ofType: "txt")
    
    driveEngine.startMockTrip(usingLocation: mockLocationData, activity: mockActivityData, fastMock: fastMock, cadence: cadence)
}
```

Add a call to this in your `viewDidLoad`:

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view, typically from a nib.
    
    // configure SDK
    configureSDK()
    
    // register the view controller as a listener
    registerDriveEventListener()
    
    let driveEngine = DEMDrivingEngineManager.sharedManager() as! DEMDrivingEngineManager
    driveEngine.startEngine()
    
    startMockTrip(fastMock: true, mockCadence: 0.3)
}
```

TODO: explain what fastMock and mockCadence are

TODO: convert this into iOS
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
Great, you've got some test data working! Next, we'll learn to [interpret the trip summaries](../interpret-trip-summary.html).