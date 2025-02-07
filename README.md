![logo](https://github.com/user-attachments/assets/0d41b803-968a-41a8-809a-0dd3d91ec489)

## 1. Download the unity package

## 2. Import the package into your project

### Drag and drop the package into the Unity Editor

## 3. Add Android Dependencies

#### Add these dependencies to your gradle file

```gradle
dependencies {
    // TrackMyUser
    implementation 'com.squareup.okhttp3:okhttp:5.0.0-alpha.11'
    implementation 'com.squareup.okhttp3:okhttp-urlconnection:5.0.0-alpha.11'
    implementation 'com.squareup.okhttp3:okhttp-dnsoverhttps:4.12.0'
    implementation 'com.squareup.retrofit2:retrofit:2.3.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.3.0'
    implementation "com.android.installreferrer:installreferrer:2.2"
    implementation 'com.google.android.gms:play-services-ads-identifier:18.0.1'
    implementation "androidx.work:work-runtime:2.8.0"
    //
}
```

### Add these permissions to your AndroidManifest.xml file

```xml
<uses-permission android:name="android.permission.INTERNET"/>
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

    // Send tracking event
    TrackMyUser.TrackEvent("Level_" + level);
}
```

