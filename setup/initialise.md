
# Initialize the SDK

You have downloaded the SDK dependency to your app/project during the installation stage of this setup. To begin using the SDK, you must initialize it in your project. The steps for this differ depending on whether you are using iOS or Android. The following section details the initialization instructions for both.  

## Initialize the iOS SDK

> **Note**:
>
> You can only initialize the MobileSDK once per app launch.

To initialize the Mobile SDK, pass the configuration object into the SDK. The configuration contains information such as the environment, theme and the merchant's public key.
The following is an example of the SDK initialization:

```Swift
let environment = SDKEnvironment.sandbox
let publicKey = "public_key"
let theme = Theme() // This is optional parameter

let config = MobileSDKConfig(environment: environment, publicKey: publicKey, theme: theme)
MobileSDK.shared.configureMobileSDK(config: config)
``` 

1. For ``let publicKey = "public_key"``, replace ``public_key`` with the value that you generated during [authentication](https://docs.paydock.com/#authentication).

2. Set the SDK Environment.

3. Apply a custom theme to the SDK. This step is optional.

### Definitions
#### MobileSDKConfig
| Name        | Definition                                                                                | Type                     | Mandatory/Optional |
| :---------- | :---------------------------------------------------------------------------------------- | :----------------------- | :----------------  |
| environment |  The target environment that will be used within the SDK                                  | MobileSDK.SDKEnvironment | Mandatory          |
| publicKey   |  The Paydock public key used for authentication with the backend services within the SDK. | Swift.String             | Mandatory          |
| theme       |  The theme to be applied across the Mobile SDK (colours, dimensions and font)             | MobileSDK.Theme          | Optional           |


## Initialize the Android SDK

{% callout %}

Note:

You can only initialize the MobileSDK once per app launch. 

{% /callout %}

You must use a builder pattern in the Android Application class to manage and setup the MobileSDK. 

```Kotlin
MobileSDK
  .Builder(<public_key>) // required
  .environment(Environment.<environment>) // optional -> default to Production
  .applyTheme(<MobileSDkTheme>) // optional
  .build(Application@this) // required (context)
```

1. Initialize the SDK from the Android Application class. The SDK can only be initialized once per app launch.

2. Add your Paydock account `public key` to `.Builder(<public_key>)`. This value is required and your MobileSDK will not work internally without it. If you do not have a public_key, you can generate it in the [Authentication instructions](https://docs.paydock.com/#authentication).

3. Set the SDK `environment`. This step is optional.

4. Apply a custom theme to the SDK. This step is also optional.

5. Add the context `.build(Application@this)`. This is mandatory. 

### Definitions
#### MobileSDKConfig
| Name        | Definition                                                                                | Type                                              | Mandatory/Optional |
| :---------- | :---------------------------------------------------------------------------------------- | :------------------------------------------------ | :----------------  |
| context     |  The Android context used for initializing the Mobile SDK and its dependencies.           | `android.content.Context`                         | Mandatory          |
| publicKey   |  The Paydock public key used for authentication with the backend services within the SDK. | String                                            | Mandatory          |
| environment |  The target environment that will be used within the SDK                                  | Enum: `com.paydock.core.domain.model.Environment` | Mandatory          |
| theme       |  The theme to be applied across the Mobile SDK (colors, dimensions and font)             | `com.paydock.MobileSDKTheme`                      | Optional           |****

