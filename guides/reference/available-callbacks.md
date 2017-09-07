# Available Callbacks
The following event capture masks are available for 

#### Recording Started
Android: `DEMEventCaptureMask.RECORDING_STARTED`
iOS: `DEMEventCaptureMask.recordingStarted`

Event fires at the beginning of trip recording

#### Recording Started With Data
Android: `DEMEventCaptureMask.RECORDING_STARTED_WITH_DATA`
iOS: `DEMEventCaptureMask.recordingStartedWithData`

Event fires when we have our initial draft of the trip object at the beginning of recording

#### Recording Stopped
Android: `DEMEventCaptureMask.RECORDING_STOPPED`
iOS: `DEMEventCaptureMask.recordingStopped`

Event fires when trip recording ends

#### Trip Info Saved
Android: `DEMEventCaptureMask.TRIP_INFO_SAVED`
iOS: `DEMEventCaptureMask.tripInfoSaved`

Event fires every 5 miles during the trip with a trip summary

#### Invalid Trip - Recording Stopped
Android: `DEMEventCaptureMask.INVALID_RECORDING_STOPPED`
iOS: `DEMEventCaptureMask.invalidRecordingStopped`

Event fires when the driving engine stops recording because of an invalid trip.

Causes of an invalid trip include: [TODO]

#### Hard Braking Detected
Android: `DEMEventCaptureMask.BRAKING_DETECTED`
iOS: `DEMEventCaptureMask.brakingDetected`

Event fires when hard braking is detected

#### Hard Acceleration Detected
Android: `DEMEventCaptureMask.ACCELERATION_DETECTED`
iOS: `DEMEventCaptureMask.accelerationDetected`

Event fires when hard acceleration is detected

#### Start of Speeding Detected
Android: `DEMEventCaptureMask.START_OF_SPEEDING_DETECTED`
iOS: `DEMEventCaptureMask.startOfSpeedingDetected`

Event fires when the driving engine detects the start of a speeding event

#### End of Speeding Detected
Android: `DEMEventCaptureMask.END_OF_SPEEDING_DETECTED`
iOS: `DEMEventCaptureMask.endOfSpeedingDetected`

Event fires when the driving engine detects the end of a speeding event

#### Capture all events
Android: `DEMEventCaptureMask.ALL`
iOS: `DEMEventCaptureMask.all`