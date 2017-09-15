# Setup the Driving Engine
Here, we're going to configure the Driving Engine and start it in the foreground. We'll do this from the `ViewController` of our application. 

# Imports
A single import for the `CoreEngine` will be enough for this demo:

```swift
import CoreEngine
```

# Modify the ViewController
Extend the ViewController to conform to the `DEMDrivingEngineDelegate` protocol:

```swift
class ViewController: UIViewController, DEMDrivingEngineDelegate {
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // register the view controller as a listener
        registerDriveEventListener()

        // configure Driving Engine
        configureDrivingEngine()
        
        let sharedEngine = DEMDrivingEngineManager.sharedManager() as! DEMDrivingEngineManager
        sharedEngine.startEngine()
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    func registerDriveEventListener() {
        let sharedEngine  = DEMDrivingEngineManager.sharedManager() as! DEMDrivingEngineManager
        
        // set this view controller as the listener for all driving events
        sharedEngine.delegate = self
        
        // listen to all driving events available
        sharedEngine.register(for: DEMEventCaptureMask.all)
    }

    func configureDrivingEngine() {
        let driveEngine = DEMDrivingEngineManager.sharedManager() as! DEMDrivingEngineManager
        
        // insert configuration here
        let config = DEMConfiguration.sharedManager() as DEMConfiguration
        config.enableWebServices = false
        config.speedLimit = 40
        
        driveEngine.setConfiguration(config)
        driveEngine.startEngine()
    }

    func didStartTripRecording(_ drivingEngine: DEMDrivingEngineManager!) -> String! {
        print("Trip Recording Started")
        return ""
    }

    func didStartTripRecording(with tripInfo: DEMTripInfo!) {
        print("Trip Recording Started - Initial Draft")
    }

    func drivingEngine(_ drivingEngine: DEMDrivingEngineManager!, didSaveTripInformation trip: DEMTripInfo!, driveStatus driveCompletionFlag: Bool) {
        print("Trip Saved")
    }

    func didStopTripRecording(_ drivingEngine: DEMDrivingEngineManager!) {
        print("Trip Recording Stopped")
    }

    func didStopInvalidTripRecording(_ drivingEngine: DEMDrivingEngineManager!) {
        print("Trip Recording Stopped - Invalid Trip")
    }

    func drivingEngine(_ drivingEngine: DEMDrivingEngineManager!, didDetectAcceleration accelerationEvent: DEMEventInfo!) {
        print("Acceleration Detected")
    }

    func drivingEngine(_ drivingEngine: DEMDrivingEngineManager!, didDetectBraking brakingEvent: DEMEventInfo!) {
        print("Braking Detected")
    }

    func drivingEngine(_ drivingEngine: DEMDrivingEngineManager!, didDetectStartOfSpeeding overSpeedingEvent: DEMEventInfo!) {
        print("Started Speeding Detected")
    }

    func drivingEngine(_ drivingEngine: DEMDrivingEngineManager!, didDetectEndOfSpeeding overSpeedingEvent: DEMEventInfo!) {
        print("Stopped Speeding Detected")
    }

    func drivingEngine(_ drivingEngine: DEMDrivingEngineManager!, didErrorOccur errorInfo: DEMError!) {
        print("Drive Error Detected")
    }
}
```

This may seem like a lot so let's break it down:

```swift
func registerDriveEventListener() {
    let sharedEngine  = DEMDrivingEngineManager.sharedManager() as! DEMDrivingEngineManager
    
    // set this view controller as the listener for all driving events
    sharedEngine.delegate = self
    
    // listen to all driving events available
    sharedEngine.register(for: DEMEventCaptureMask.all)
}
```

In our `registerDriveEventListener()` method, we register the `ViewController` as the event listener for drive events. The third line tells the Driving Engine that it should surface all Drive Events to the listener. We call this function in `viewDidLoad` and then start the engine.

The rest of the functions are implementations of the `DEMDrivingEngineDelegate` protocol. These functions act as callbacks for different Driving Engine events. As you can see from the available callbacks, you have a rich number events to draw from the Driving Engine to affect the functionality of your application. A full listing is available [here](../../reference/available-callbacks.md).

## Next Steps
Now that the app is setup, let's test it with some mock data! You can do learn to do that [here](../test-mock-data/iOS.md).