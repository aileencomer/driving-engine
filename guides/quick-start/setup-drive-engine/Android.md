# Setup the Driving Engine
Here, we're going to configure the Driving Engine and start it in the foreground. We'll do this from the `MainActivity` of our application. 

## Imports
Add the following imports to your `MainActivity`:

```java
import android.util.Log;

import com.allstate.coreEngine.beans.DEMEventInfo;
import com.allstate.coreEngine.beans.DEMTripInfo;
import com.allstate.coreEngine.beans.DEMError;
import com.allstate.coreEngine.constants.DEMEventCaptureMask;
import com.allstate.coreEngine.driving.DEMDrivingEngineManager;
```

## Register the Event Listener
```java
public class MainActivity extends AppCompatActivity implements DEMDrivingEngineManager.EventListener {

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // set context for the Driving Engine
        DEMDrivingEngineManager.setContext(this.getApplicationContext());

        // register this activity as a listener
        registerDriveEngineListener();

        // start Driving Engine
        DEMDrivingEngineManager.getInstance().startEngine();
    }

    public void registerDriveEngineListener() {
        // set this activity as the listener for all driving events
        DEMDrivingEngineManager.getInstance()
            .setEventListener(this);

        // listen to all driving events available
        DEMDrivingEngineManager.getInstance()
            .registerForEventCapture(DEMEventCaptureMask.DEM_EVENT_CAPTURE_ALL);
    }

    @Override
    public String onTripRecordingStarted() {
        Log.d("DriveEngineSample", "Trip Recording Started");
        return null;
    }

    @Override
    public void onTripRecordingStarted(DEMTripInfo demTripInfo) {
        Log.d("DriveEngineSample", "Trip Recording Started - Initial Draft");
    }

    @Override
    public void onTripInformationSaved(DEMTripInfo demTripInfo, boolean completionFlag) {
        Log.d("DriveEngineSample", "Trip Saved");
    }

    @Override
    public void onTripRecordingStopped() {
        Log.d("DrivingEngineSample", "Trip Recording Stopped");
    }

    @Override
    public void onInvalidTripRecordingStopped() {
        Log.d("DriveEngineSample", "Trip Recording Stopped - Invalid Trip");
    }

    @Override
    public void onBrakingDetected(DEMEventInfo demEventInfo) {
        Log.d("DriveEngineSample", "Braking Detected");
    }

    @Override
    public void onAccelerationDetected(DEMEventInfo demEventInfo) {
        Log.d("DriveEngineSample", "Acceleration Detected");
    }

    @Override
    public void onStartOfSpeedingDetected(DEMEventInfo demEventInfo) {
        Log.d("DriveEngineSample", "Started Speeding Detected");
    }

    @Override
    public void onEndOfSpeedingDetected(DEMEventInfo demEventInfo) {
        Log.d("DriveEngineSample", "Stopped Speeding Detected");
    }

    @Override
    public void onError(DEMError demError) {
        Log.d("DriveEngineSample", "Drive Error Detected");
    }

    @Override
    public String onRequestMetaData() { return null; }

    @Override
    public void onGpsAccuracyChangeDetected(int gpsAccuracyLevel) { }
}
```

This may seem like a lot so let's break it down:

```java
public void registerDriveEngineListener() {
    // set this activity as the listener for all driving events
    DEMDrivingEngineManager.getInstance()
        .setEventListener(this);

    // listen to all driving events available
    DEMDrivingEngineManager.getInstance()
        .registerForEventCapture(DEMEventCaptureMask.DEM_EVENT_CAPTURE_ALL);
}
```

In our `registerDriveEventListener()` method, we register the `MainActivity` as the event listener for drive events. The second line tells the Driving Engine that it should surface all Drive Events to the listener.

Once we've called this method in `onCreate`, the rest of the overridden methods from `DEMDrivingEngineManager.EventListener` are purely callbacks for different drive events. As you can see from the available callbacks, you have a rich number events to draw from the Driving Engine to affect the functionality of your application. A full listing is available [here](../../reference/available-callbacks.md).

## Runtime Permissions
With the introduction of Android M, users have more granular control over the permissions they give an app. For your app to work with the Driving Engine, you'll need to require several runtime permissions:

* `Manifest.permission.ACCESS_FINE_LOCATION`
* `Manifest.permission.WRITE_EXTERNAL_STORAGE`
* `Manifest.permission.READ_EXTERNAL_STORAGE`

Add these as a field to our class:

```java
    final List<String> requiredPermissions = Arrays.asList(
        Manifest.permission.ACCESS_FINE_LOCATION,
        Manifest.permission.WRITE_EXTERNAL_STORAGE,
        Manifest.permission.READ_EXTERNAL_STORAGE
    );

    final int PERMISSIONS_REQUEST_ACCESS = 1;
```

To account for this, let's add a new method to check for granted permissions:

```java
private void checkPermissions() {
    List<String> denied = new ArrayList();
    for (String permission : requiredPermissions) {
        if (ContextCompat.checkSelfPermission(this, permission) == PackageManager.PERMISSION_DENIED) {
            denied.add(permission);
        }
    }

    if(denied.size() == 0) return;

    ActivityCompat.requestPermissions(this, denied.toArray(new String[denied.size()]),
            PERMISSIONS_REQUEST_ACCESS);
}
```

Let's add this before the `startEngine` call in `onCreate`

```java
// check runtime permissions
checkPermissions();

// start Driving Engine
DEMDrivingEngineManager.getInstance().startEngine();
```

And let's account for a user changing permissions later on in `onError`:

```java
@Override
public void onError(DEMError demError) {
    Log.d("DriveEngineSample", "Drive Error Detected");
    if (demError.getCategory().equalsIgnoreCase(DEMError.ErrorCategory.ERROR_OBTAINING_PERMISSION))
        checkPermissions();
}
```

## Notes
In a production app, it's best practice to run this in a background service. You can learn more about that [here](,,/../../best-practices/background-service/Android.md).

## Next Steps
Now that the app is setup, let's test it with some mock data! You can do learn to do that [here](../test-mock-data/Android.md).