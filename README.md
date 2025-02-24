![logo](https://github.com/user-attachments/assets/0d41b803-968a-41a8-809a-0dd3d91ec489)

## 1. Download the unity package

https://github.com/user-attachments/assets/cfb894f4-87af-485b-87ed-c9063c2ec1ad

## 2. Import the package into your project

https://github.com/user-attachments/assets/4d9d2368-46f0-4df9-ad81-19fafb2aeac1

## 3. Add Android Dependencies

https://github.com/user-attachments/assets/b6a3ed3b-e8a8-43ce-86af-06bc3d0b74be

#### Add the dependency to your gradle file

```gradle
dependencies {
    implementation 'com.github.trackmyuser:android-sdk:0.2.17'
}
```

### Add these permissions to your AndroidManifest.xml file

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="com.google.android.gms.permission.AD_ID"/>
```

## 4. Initialising the SDK

Modify any GameManager or startup script to initialize the SDK:

```cs
using UnityEngine;

public class GameManager : MonoBehaviour {
    void Start() {
        TrackMyUser.Init("YOUR_SDK_KEY");  // Initialize SDK
    }
}
```

## 5. Event Tracking

Whenever the player progresses to a new level, call:


```cs
public void ProgressToNextLevel(int level) {
    Debug.Log("User progressed to Level " + level);

    // Send tracking event with the event code copied from the dashboard
    TrackMyUser.TrackEvent("YOUR_EVENT_CODE");
}
```

