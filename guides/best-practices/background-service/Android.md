# Running in a Background Service
In order to support continuous capture of data from the SDK, you'll need to run it in the background. In order to do that, we'll wrap it in a service that can be initiated at the start of the application. This service will listen to all drive events and surface them to the app.

## Create the background service
Create a new service class. We'll call it `CoreEngineService.java` in our example. In this code file, we'll need the boilerplate for a service startup. An example is provided below:

```java
import android.app.Service;
import android.content.Intent;
import android.os.IBinder;
import android.support.annotation.Nullable;
import android.support.v4.content.LocalBroadcastManager;
import android.util.Log;

import com.allstate.coreEngine.beans.DEMError;
import com.allstate.coreEngine.beans.DEMEventInfo;
import com.allstate.coreEngine.beans.DEMTripInfo;
import com.allstate.coreEngine.driving.DEMDrivingEngineManager;
import com.google.gson.Gson;

public class CoreEngineService extends Service {

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }

    @Override
    public final int onStartCommand(Intent intent, int flags, int startId) {
        super.onStartCommand(intent, flags, startId);
        return START_STICKY;
    }

    @Override
    public void onCreate() {
        super.onCreate();
    }
}
```

This service will need extend the `Service` class. In addition, to capture Driving Engine events, we'll need to implement the `DEMDrivingEngineManager.EventListener` interface.

## Capture Drive Events
Before the SDK starts, we need it to let it know that our service will capture drive events. Let's create a function to do so and change the `onCreate` of our service to call it.

```java
@Override
public void onCreate() {
    super.onCreate();

    configureDriveEngine();
}

private void configureDriveEngine() {
    // set the service as the event listener. We'll need to implement methods to capture these events
    DEMDrivingEngineManager.getInstance().setEventListener(this);

    // capture all events from the DE SDK
    DEMDrivingEngineManager.getInstance().registerForEventCapture(DEMEventCaptureMask.DEM_EVENT_CAPTURE_ALL);

    // TODO: this is a good spot for additional configuration
}
```


## Setup Drive Events Listener
In order to capture the drive events, we need to implement the `DEMDrivingEngineManager.EventListener` interface. 

```java
public class CoreEngineService extends Service implements DEMDrivingEngineManager.EventListener {
```

You'll need to override the following methods:

```java
    @Override
    public String onTripRecordingStarted() {
        Log.d("DriveEngineSample", "*** Trip Recording Started ***");
        return null;
    }

    @Override
    public void onTripRecordingStarted(DEMTripInfo demTripInfo) {
        Log.d("DriveEngineSample", "*** Trip Recording Started - Initial Draft ***");
    }

    @Override
    public void onTripInformationSaved(DEMTripInfo demTripInfo, boolean completionFlag) {
        Log.d("DriveEngineSample", "*** Trip Saved ***");
    }

    @Override
    public void onTripRecordingStopped() {
        Log.d("DrivingEngineSample", "*** Trip Recording Stopped ***");
    }

    @Override
    public void onInvalidTripRecordingStopped() { }

    @Override
    public void onBrakingDetected(DEMEventInfo demEventInfo) { }
    }

    @Override
    public void onAccelerationDetected(DEMEventInfo demEventInfo) { }


    @Override
    public void onStartOfSpeedingDetected(DEMEventInfo demEventInfo) { }

    @Override
    public void onEndOfSpeedingDetected(DEMEventInfo demEventInfo) { }

    @Override
    public void onError(DEMError demError) { }

    @Override
    public String onRequestMetaData() { return null; }

    @Override
    public void onGpsAccuracyChangeDetected(int gpsAccuracyLevel) { }
```

## Modify Application Lifecycle