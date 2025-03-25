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

## 5. Event Tracking

```cs
public void ProgressToNextLevel(int level) {
    Debug.Log("User progressed to Level " + level);

    // Send tracking event with the event code copied from the dashboard
    TrackMyUser.TrackEvent("YOUR_EVENT_CODE");
}
```

## 5. In-App Purchase Tracking

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
        string purchaseToken = ExtractPurchaseToken(receipt);
        if(purchaseToken != null)
        {
            iapEvent.setPurchaseToken(purchaseToken);
        }
    
        TrackMyUserSDK.TrackIAPEvent(iapEvent);
        
        return PurchaseProcessingResult.Complete;
    }

    private string ExtractPurchaseToken(string receipt)
    {
        if (string.IsNullOrEmpty(receipt))
        {
            return null;
        }

        try
        {
            const string tokenKey = "\"purchaseToken\":\"";
            int tokenStartIndex = receipt.IndexOf(tokenKey);

            if (tokenStartIndex == -1)
            {
                Debug.LogError("Purchase token not found in receipt.");
                return null;
            }

            tokenStartIndex += tokenKey.Length;
            int tokenEndIndex = receipt.IndexOf("\"", tokenStartIndex);

            if (tokenEndIndex == -1)
            {
                Debug.LogError("Failed to extract purchase token.");
                return null;
            }

            string purchaseToken = receipt.Substring(tokenStartIndex, tokenEndIndex - tokenStartIndex);
            return purchaseToken;
        }
        catch (Exception ex)
        {
            Debug.LogError($"Error extracting purchase token: {ex.Message}");
            return null;
        }
    }
}

```

