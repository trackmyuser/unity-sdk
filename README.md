![logo](https://github.com/user-attachments/assets/0d41b803-968a-41a8-809a-0dd3d91ec489)

## 1. Download the unity package

https://github.com/user-attachments/assets/cfb894f4-87af-485b-87ed-c9063c2ec1ad

## 2. Import the package into your project

https://github.com/user-attachments/assets/4d9d2368-46f0-4df9-ad81-19fafb2aeac1

## 3. Add Android Dependencies

#### Add these dependencies to your gradle file

https://github.com/user-attachments/assets/5c1ac3f9-499f-4db7-bb98-324d15670fe0

```gradle
dependencies {
    //TrackMyUser
    implementation(name: 'com.trackmyuser.sdk_v0.0.4', ext: 'aar')
    implementation 'com.android.installreferrer:installreferrer:2.2'
    implementation 'com.squareup.okhttp3:okhttp:5.0.0-alpha.11'
    implementation 'com.squareup.okhttp3:okhttp-urlconnection:5.0.0-alpha.11'
    implementation 'com.squareup.okhttp3:okhttp-dnsoverhttps:4.12.0'
    implementation 'com.squareup.retrofit2:retrofit:2.3.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.3.0'
    implementation "androidx.work:work-runtime:2.8.0"
    implementation 'com.google.android.gms:play-services-ads-identifier:18.0.1'
    //
}
```

### Add these permissions to your AndroidManifest.xml file

https://github.com/user-attachments/assets/5c1ac3f9-499f-4db7-bb98-324d15670fe0

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

