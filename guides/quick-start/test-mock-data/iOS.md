# Test with Mock Data
In this section, we'll feed the Driving Engine mock data to make sure it works. We'll use simulated accelerometer and GPS data to capture acceleration and braking events.

## Mock Data Files
With the SDK, you're provided with two files in the `Data` folder, `MockActivity.txt` and `MockLocation.txt`. Store these files on your phone and note your file paths. For the purposes of this guide, we'll embed these files in the app for testing.

On the `Build Phases` section of your project settings, add both files to the `Copy Bundle Resources` build step. As before, ensure you check the `Copy files if needed` option when you locate the files.

## Modify MainActivity
Add the following functions to your `ViewController`:

```swift
func startMockTrip(cadence: TimeInterval) {
    let driveEngine = DEMDrivingEngineManager.sharedManager() as! DEMDrivingEngineManager
    let mockLocationData = Bundle.main.path(forResource: "MockLocation", ofType: "txt")
    let mockActivityData = Bundle.main.path(forResource: "MockActivity", ofType: "txt")
    
    driveEngine.startMockTrip(usingLocation: mockLocationData, activity: mockActivityData, fastMock: true, cadence: cadence)
}
```

Add a call to this in your `viewDidLoad`:

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional set up after loading the view, typically from a nib.
    
    // configure Driving Engine
    configureDrivingEngine()
    
    // register the view controller as a listener
    registerDriveEventListener()
    
    let driveEngine = DEMDrivingEngineManager.sharedManager() as! DEMDrivingEngineManager
    driveEngine.startEngine()
    
    startMockTrip(cadence: 0.3)
}
```

The `cadence` that you set for `startMockTrip` will determine how fast the engine processes each GPS point. For example, a value of `0.5` would correspond to a half second between points where a value of `1.0` would be real time.

## Test the Application
Since there isn't any UI on this app, we'll monitor the `Device Console` to ensure the mock trip is working. After you've built and deployed the app to your phone, run the app to with it connected to your laptop to see debug messages.

## Next Steps
Great, you've got some test data working! Next, we'll learn to [interpret the trip summaries](../../reference/interpret-trip-summary.md).
