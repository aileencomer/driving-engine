# Integrating the Library
The Android version of the Drive Engine SDK is delivered as an `Android Archive (.aar)`. With a couple steps, you can quickly get up and running with the Drive Engine SDK in your project.

## Dependencies
The SDK requires the following libraries to run:
* Google Play Services (com.google.android.gms:play-services)
* Gson (com.google.code.gson:gson)

Add these to your project from File > Project Structure > Dependencies > '+' Library Dependency

## Import the Library
Switch to the Project View and drag Core Engine `.aar` file into `libs` folder. Add the following line to your App level `build.gradle` file (denoted as Module: app in the editor):

```gradle
dependencies {
    implementation(name: 'coreEngine_release_1.11.3', ext: 'aar')
}

repositories {
    flatDir {
        dirs 'libs'
     }
}
```

Run a `Gradle Sync` to see the changes.

__NOTE__: You'll need to update `coreEngine_release_1.12.0` to match the version of the SDK you're using.

## Manifest Changes
In order to notify Android about metadata and permissions that the SDK requires, you'll need to make the below modifications to `AndroidManifest.xml` in your project.

### Permissions
Add the following permissions to your app. 
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="com.google.android.gms.permission.ACTIVITY_RECOGNITION" />
```
__NOTE__: As of Android M, there are several runtime permissions you can also request. You'll learn more about when you setup the drive engine in the next step.

## Next Steps
You should now be able to build your application without any errors. If that's working, let's move on to the next step and [setup the drive engine](../setup-drive-engine/Android.md)