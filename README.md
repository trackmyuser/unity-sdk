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
    implementation 'com.github.trackmyuser:android-sdk:1.1.3'
}
```

### Add these permissions to your AndroidManifest.xml file

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="com.google.android.gms.permission.AD_ID"/>
```

#### Add these to your settings.gradle file

```gradle
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS) // make sure RepositoriesMode is set to FAIL_ON_PROJECT_REPOS
    repositories {
        google()
        mavenCentral()
        gradlePluginPortal()
        maven { url 'https://jitpack.io' } // make sure to add this line
    }
}
```

## 4. Initialising the SDK

Modify any GameManager or startup script to initialize the SDK:

```cs
using UnityEngine;

public class GameManager : MonoBehaviour {
    void Start() {
        TrackMyUserConfig config = new TrackMyUserConfig();
        config.setAndroidSdkKey("YOUR_ANDROID_SDK_KEY");
        config.setiOSSdkKey("YOUR_IOS_SDK_KEY");
        TrackMyUserSDK.Init(config);  // Initialize SDK
    }
}
```

## 5. Custom Event Tracking

```cs
// Send tracking event with the event code copied from the dashboard
TrackMyUserSDK.TrackEvent("YOUR_EVENT_CODE");
```

## 6. In-App Purchase Tracking

```cs
using UnityEngine;
using UnityEngine.Purchasing;
using System;

public class IAPManager : MonoBehaviour, IStoreListener
{
    public PurchaseProcessingResult ProcessPurchase(PurchaseEventArgs args)
    {
        Product product = args.purchasedProduct;

        // Extract default values
        string productId = product.definition.id;
        string transactionID = product.transactionID;
        double revenue = (double)product.metadata.localizedPrice;
        string currency = product.metadata.isoCurrencyCode;
        string receipt = product.receipt;

        TrackMyUserIAPEvent iapEvent = new TrackMyUserIAPEvent();
        iapEvent.SetRevenue(currency, revenue);
        iapEvent.SetProductId(productId);
        iapEvent.SetTransactionId(transactionID);

        //required for android.
        iapEvent.setReceipt(receipt);
    
        TrackMyUserSDK.TrackIAPEvent(iapEvent);
        
        return PurchaseProcessingResult.Complete;
    }
}
```

## 6. Level completion Tracking

```cs
public void ProgressToNextLevel(int level) {
    TrackMyUserSDK.TrackLevelCompletedEvent(new TrackMyUserLevelCompletedEvent(level.ToString()));
}
```

## 7. Ad revenue Tracking

Follow the instructions in the link below to enable AdMob's impression-level ad revenue:

https://support.google.com/admob/answer/11322405?hl=en

#### Example using AdMob:

```cs
using GoogleMobileAds.Api;
using GoogleMobileAds.Common;
using System;

RewardedAd rewardedAd;

void LoadRewardedAd() {
    rewardedAd = new RewardedAd("your_ad_unit_id");

    rewardedAd.OnPaidEvent += HandlePaidEvent;

    AdRequest request = new AdRequest.Builder().Build();
    rewardedAd.LoadAd(request);
}

void HandlePaidEvent(AdValue adValue) {
    long valueMicros = args.AdValue.Value; // revenue in micros
    string currencyCode = args.AdValue.CurrencyCode;
    double revenue = valueMicros / 1_000_000.0;

    TrackMyUserSDK.TrackAdRevenueEvent(new TrackMyUserAdRevenueEvent(currencyCode, revenue));
}
```
