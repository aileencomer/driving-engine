# Running in a Background Service
In order to support continuous capture of data from the SDK, you'll need to run it in the background. In order to do that, we'll wrap it in a service that can be initiated at the start of the application. This service will listen to all drive events and surface them to the app.

## Reference Implementation
You can find a reference implementation in the `sample` app provided with your downloaded release. The main modifications are as follows:

### Create Service
You'll need to start the Driving Engine and capture events using a service. This service should capture driving events and broadcast data to your main application. The implementation of this is found in `service/CoreEngineService.java` of the sample app.

### Modify Application Lifecycle
If you don't have an `Application.java` singleton class or some other class to manage the Application lifecycle, you'll need to create one and kick off the service from the `onCreate` function. The implementation of this is found in `Application.java` of the sample app.

### Consume events in the UI / App Singleton
Once you can successfully subscribe to events, you'll need to capture them in either your UI or a singleton class you use to manage state. In the sample app, you can find out how we create UI changes in `MainActivity.java`.
