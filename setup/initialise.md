
# Initialize the SDK

You have downloaded the SDK dependency to your app/project during the installation stage of this setup. To begin using the SDK, you must initialize it in your project. The steps for this differ depending on whether you are using iOS or Android. The following section details the initialization instructions for both.  

## Initialize the iOS SDK

> **Note**:
>
> You can initialize the MobileSDK multiple times to reconfigure the SDK if necessary.

To initialize the MobileSDK, pass the configuration object into the SDK. The configuration contains information such as the environment, theme and enableTestMode.
The following is an example of the SDK initialization:

```Swift
let environment = SDKEnvironment.sandbox
let theme = Theme() // This is optional parameter
let testMode = true // This is optional parameter

let config = MobileSDKConfig(environment: environment, theme: theme, enableTestMode: testMode)
MobileSDK.shared.configureMobileSDK(config: config)
``` 

1. Set the SDK Environment.

2. Apply a custom theme to the SDK. This step is optional.

3. Set test mode flag to the SDK. This step is optional.

### Definitions
#### MobileSDKConfig
| Name           | Definition                                                                                | Type                       | Mandatory/Optional |
| :------------- | :---------------------------------------------------------------------------------------- | :------------------------- | :----------------  |
| environment    |  The target environment that will be used within the SDK                                  | `MobileSDK.SDKEnvironment` | Mandatory          |
| enableTestMode |  Flag to enable test mode. This is only allowed in non-production environments            | Bool                       | Optional          |
| theme          |  The theme to be applied across the Mobile SDK (colours, dimensions and font)             | `MobileSDK.Theme`          | Optional           |


## Initialize the Android SDK

> **Note**:
>
> You can only initialize the MobileSDK once per app launch.

You must use a builder pattern in the Android Application class to manage and setup the MobileSDK. 

```Kotlin
MobileSDK.Builder()
  .environment(Environment.<environment>) // optional -> default to Production
  .enableTestMode(<true/false>) // optional -> default to false
  .applyTheme(<MobileSDkTheme>) // optional
  .build(Application@this) // required (context)
```

1. Initialize the SDK from the Android Application class. The SDK can only be initialized once per app launch.

2. Set the SDK `environment`. This step is optional. (defaults to `production`)

3. Set `enableTestMode` flag. This step is also optional. (defaults to false)

4. Apply a custom theme to the SDK. This step is also optional.

5. Add the context `.build(Application@this)`. This is mandatory. 

### Definitions
#### MobileSDKConfig
| Name           | Definition                                                                                | Type                                              | Mandatory/Optional |
| :------------- | :---------------------------------------------------------------------------------------- | :------------------------------------------------ | :----------------  |
| context        |  The Android context used for initializing the Mobile SDK and its dependencies.           | `android.content.Context`                         | Mandatory          |
| environment    |  The target environment that will be used within the SDK                                  | Enum: `com.paydock.core.domain.model.Environment` | Optional           |
| enableTestMode |  Flag to enable test mode. This is only allowed in non-production environments            | Boolean                                           | Optional           |
| theme          |  The theme to be applied across the Mobile SDK (colors, dimensions and font)              | `com.paydock.MobileSDKTheme`                      | Optional           |

