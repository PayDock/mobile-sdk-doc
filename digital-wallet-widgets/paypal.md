# PayPal Widget

The MobileSDK provides a PayPal Widget that integrates with PayPal using a WebView component. This handles the communication between PayPal and the SDK. Once completed, the WebView component returns the charge result.

![PayPal View](/img/PayPal.png)

## iOS

## How to use the PayPalView

### 1. Overview 

The iOS SDK supports payments using PayPal. The component's logic is contained in a Single View, and so you must initialize and embed this logic within your payment flow.

> **Note**:
>
> When the customer taps the Payment button for your Digital Wallet, the SDK triggers a callback to the Merchant app requesting a `wallet_token`. You must perform a wallet initialization request to receive your `wallet_token`. To do this, follow the instructions in the [generate a wallet_token](/digital-wallet-widgets/wallettoken.md) section of this guide.  

Use the following to initialize the **PayPalView**:

```Swift
PayPalWidget(
    viewState: ViewState?,
    loadingDelegate: WidgetLoadingDelegate?,
    payPalTokenHandler: @escaping (_ payPalToken: @escaping (String) -> Void) -> Void,
    completion: @escaping (Result<ChargeResponse, PayPalError>) -> Void)
```
In case of successful charge, the **PayPalView** returns a `ChargeResponse` that contains all the relevant information. In case of an error, the **PayPalError** object is returned with information regarding the failure.

The following is an example of a full PayPalView initialization:

```Swift
struct PayPalExampleView: View {
    var body: some View {
        VStack {
            PayPalWidget { onPayPalButtonTap in
                onPayPalButtonTap(payPalToken)
            } completion: { result in
                switch result {
                case .success(let chargeResponse): // Handle successful charge response
                case .failure(let error): // Handle error
                }
            }
        }
    }
}
```

### 2. Parameter Definitions

#### PayPalWidget

| Name             | Definition                                                                                        | Type                                            | Mandatory/Optional |
| :--------------- | :------------------------------------------------------------------------------------------------ | :---------------------------------------------- | :----------------- |
| viewState        |  View options that are two way fields to alter view state                                         | ViewState                                       | Optional           |
| loadingDelegate  |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown.  | WidgetLoadingDelegate                           | Optional           |
| payPalToken      |  A callback to obtain the wallet token asynchronously                                             | `(String) -> Void) -> Void`                     | Mandatory          |
| completion       |  Result callback with the Charge creation API response if successful, or error if not.            | `(Result<ChargeResponse, PayPalError>) -> Void` | Mandatory          |

#### MobileSDK.ViewState

| Name                   | Definition                                                              | Type                                               | Mandatory/Optional |
| ---------------------- | ----------------------------------------------------------------------- | -------------------------------------------------- |------------------  |
| state                  |  Sets the state the widget should be placed in                          | Enum (disabled, none)                              | Mandatory          |

The following definitions provide a more detailed overview of the parameters used in the Paypal initialisation, and the potential error messages that may occur.

#### MobileSDK.ChargeResponse

| Name     | Definition                                       | Type          | Mandatory/Optional |
| :------- | :----------------------------------------------- | :------------ | :----------------  |
| status   |  Status of a PayPal payment after completion.    | Swift.String  | Mandatory          |
| amount   |  The amount of money that was charged            | Swift.Decimal | Mandatory          |
| currency |  Currency of the charge.                         | Swift.String  | Mandatory          |

#### MobileSDK.PayPalError

| Name                       | Description                                                                     | Error Result            |
| :------------------------ | :------------------------------------------------------------------------------- | :---------------------- |
| errorFetchingPayPalUrl    |  Error thrown when there is an error fetching the URL for PayPal.                |  ErrorRes               |
| errorCapturingCharge      |  Error thrown when there is an error capturing the charge for PayPal.            |  ErrorRes               |
| webViewFailed             |  Error thrown when there is an error while communicating with a WebView.         |  NSError                |
| transactionCanceled       |  Error thrown when user cancels the flow.                                        |  nil                    |
| UnknownException          |  Error thrown when there is an unknown error related to PayPal.                  |  nil                    |

## Android

## How to use the PayPalWidget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `PayPalWidget` composable in your application. The widget facilitates the payment using PayPal services.

> **Note**:
>
> When the customer taps the Payment button for your Digital Wallet, the SDK triggers a callback to the Merchant app requesting a `wallet_token`. You must perform a wallet initialization request to receive your `wallet_token`. To do this, follow the instructions in the [generate a wallet_token](/digital-wallet-widgets/wallettoken.md) section of this guide.  

The following sample code demonstrates the definition of the `PayPalWidget`:

```Kotlin
fun PayPalWidget(
    modifier: Modifier = Modifier,
    enabled: Boolean = true,
    config: PayPalWidgetConfig = PayPalWidgetConfig(),
    appearance: PayPalWidgetAppearance = PayPalAppearanceDefaults.appearance(),
    tokenRequest: (tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit,
    loadingDelegate: WidgetLoadingDelegate? = null,
    completion: (Result<ChargeResponse>) -> Unit,
) {...}
```

The following sample code example demonstrates the usage within your application:

```Kotlin
// Initialize the PayPalWidget
PayPalWidget(
    modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp), // optional
    config = PayPalWidgetConfig(requestShipping = false), //optional (default: true)
    tokenRequest = { callback ->
        // show widget loader
        tokenResult.onSuccess { result ->
            // Handle token success
            val walletToken = result.token // ... retrieve the token
            Result.success(WalletTokenResult(token = walletToken))
        }.onFailure {
            // Handle token failure success
            Result.failure(it)
        }
    },
    loadingDelegate = DELEGATE_INSTANCE, // Delegate class to handle loading
) { result ->
    // Handle the result of the payment operation
    result.onSuccess { chargeResponse ->
        // Handle success - Update UI or perform actions
        Log.d("PayPalWidget", "Payment successful.")
    }.onFailure { exception ->
        // Handle failure - Show error message or take appropriate action
        Log.e("PayPalWidget", "Payment failed. Error: ${exception.message}")
    }
}
```

### 2. Parameter Definitions

This subsection describes the various parameters required by the `PayPalWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `PayPalWidget`.

#### PayPalWidget

| Name                  | Definition                                                                                      | Type                                          | Mandatory/Optional |
| :-------------------- | :---------------------------------------------------------------------------------------------- | :-------------------------------------------- | :----------------- |
| modifier              |  Compose modifier for container modifications                                                   | `androidx.compose.ui.Modifier`                | Optional           |
| enabled               |  Controls the enabled state of this Widget.                                                     | Boolean                                       | Optional           |
| appearance            |  Customization options for the visual appearance of the widget                                  | `PayPalWidgetAppearance`                      | Optional           |
| config                |  Configuration options for the PayPal widget                                                    | `PayPalWidgetConfig`                          | Mandatory          |
| tokenRequest          |  A callback to obtain the wallet token result asynchronously                                    | `tokenRequest: (tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit`     | Mandatory          |
| requestShipping       |  Flag passed to determine if PayPal will ask the user for their shipping address.               | Boolean (default = `true`)                    | Optional           |
| loadingDelegate       |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown.| `WidgetLoadingDelegate`                       | Optional           |
| completion            |  Result callback with the Charge creation API response if successful, or error if not.          | `(Result<ChargeResponse>) -> Unit`            | Mandatory          |

#### ChargeResponse

This subsection outlines the structure of the result or response object returned by the `PayPalWidget` composable. It details the format and components of the object, enabling you to handle the response effectively within your application.

The following sample code demonstrates the response structure:

```Kotlin
data class ChargeResponse(
    val status: Int, 
    val resource: ChargeResource
) {

    data class ChargeResource(
        val type: String,
        val data: ChargeData?
    )
    
    data class ChargeData(
        val status: String,
        val id: String,
        val amount: BigDecimal,
        val currency: String
    )
}
```

| Name           | Definition                                                                       | Type                       | 
| :------------- | :------------------------------------------------------------------------------- | :------------------------- | 
| status         |  Charge result HTTP status code                                                  | Int                        | 
| resource       |  Charge resource containing data related to charge                               | `ChargeResponse.Resource`  |

#### ChargeResponse.Resource

| Name           | Definition                                                                                      | Type                       | 
| :------------- | :---------------------------------------------------------------------------------------------- | :------------------------- | 
| type           |  Charge result HTTP status code                                                                 | String                     | 
| data           |  Charge data result containing actual data regarding charge ie. amount, currency etc            | `ChargeResponse.Data`      |

#### ChargeResponse.ChargeData

| Name           | Definition                                | Type                 | 
| :------------- | :---------------------------------------- | :------------------- | 
| status         |  Charge result status as a string         | String               | 
| amount         |  Charge amount that was charged           | `BigDecimal`         |
| currency       |  Charge currency that was used            | String               |

### 3. Callback Explanation

#### Token Callback

The `token` callback obtains the wallet token result asynchronously. It receives a callback function `tokenRequest: (tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit` as a parameter, which you must invoke with the result of the wallet token API request once it is obtained. 

The `WalletTokenResult` acts as a token wrapper containing the actual token result.

### WidgetLoadingDelegate

This `loadingDelegate` allows the calling app to take control of the internal widget loading states. When set, internal loaders will not be shown. 
It defines methods to handle the start and finish of a loading process. This can be accompanied by the `enabled` flag to signal to the widget that the calling app may be loading.

```Kotlin
interface WidgetLoadingDelegate {
    // Called when a widget's loading process starts.
    fun widgetLoadingDidStart()

    // Called when a widget's loading process finishes.
    fun widgetLoadingDidFinish()
}
```

#### Completion Callback

The `completion` callback is invoked after the payment operation is completed. It receives a `Result<ChargeResponse>` if the payment is successful. The callback is used to handle the outcome of the payment operation.

#### 4. Error/Exceptions Mapping

The following describes PayPal exceptions that can be thrown. 

```Kotlin
FetchingUrlException(error: ApiErrorResponse) : PayPalException(error.displayableMessage)
CapturingChargeException(error: ApiErrorResponse) : PayPalException(error.displayableMessage)
WebViewException(code: Int?, displayableMessage: String) : PayPalException(displayableMessage)
CancellationException(displayableMessage: String) : PayPalException(displayableMessage)
InitialisationWalletTokenException(displayableMessage: String, val errorBody: String?) : PayPalException(displayableMessage)
UnknownException(displayableMessage: String) : PayPalException(displayableMessage)
```

| Exception                 | Description                                                                                   | Error Model       |
| :------------------------ | :-------------------------------------------------------------------------------------------- | :---------------- |
| FetchingUrlException      |  Exception thrown when there is an error fetching the URL for PayPal.                         |  PayPalError      |
| CapturingChargeException  |  Exception thrown when there is an error capturing the charge for PayPal.                     |  PayPalError      |
| WebViewException          |  Exception thrown when there is an error while communicating with a WebView.                  |  PayPalError      |
| CancellationException     |  Exception thrown when there is a cancellation error related to PayPal.                       |  PayPalError      |
| InitialisationWalletTokenException            |  Exception thrown when the wallet token result returns a failure result.  |  PayPalError      |
| UnknownException          |  Exception thrown when there is an unknown error related to PayPal.                           |  PayPalError      |

### 5. Widget Styling

Defines the visual appearance for specific elements within the `PayPalWidget`.
The primary visual element is the PayPal button, which has its own branding guidelines. This appearance configuration mainly focuses on the loading indicator displayed during interactions with the PayPal flow.

#### Appearance Contract

The `PayPalWidgetAppearance` class encapsulates the configurable style properties for the widget, currently centered on the loader.

```Kotlin
@Immutable
class PayPalWidgetAppearance(
    val loader: LoaderAppearance
)
```

#### Default Appearance & Customisation

A default appearance is provided by `PayPalAppearanceDefaults`. This configures a specific loader appearance (black color, default stroke width) intended to be visually compatible with the standard PayPal button.

##### Using Default Appearance


```Kotlin
    PayPalWidget( 
        ...
        appearance = PayPalAppearanceDefaults.appearance() // Uses the default appearance
    )
```

##### Customising Appearance

You can create a custom `PayPalWidgetAppearance` by providing a specific `LoaderAppearance` configuration if the default doesn't meet your needs or if you want to ensure consistency with other loaders in your app.

```Kotlin
@Composable 
fun MyCustomPayPalScreen() { 
    // Create appearance by using provided defaults, with custom changes
    val customAppearanceFromDefaults = PayPalAppearanceDefaults.appearance().copy(     
        loader = LoaderAppearanceDefaults.appearance().copy( 
            color = Color.Blue,
            strokeWidth = 3.dp   
        )
    )

    // Alternatively, create entirely from scratch:
    val completelyCustomAppearance = PayPalWidgetAppearance(
    loader = LoaderAppearanceDefaults.appearance().copy( // Or new LoaderAppearance(...)
        color = Color.DarkGray,
        strokeWidth = 2.dp
        // ... other LoaderAppearance properties
    )
)

    PayPalWidget(
        ...
        appearance = customAppearanceFromDefaults, // Use your custom appearance
    )
}
```

#### Style Attributes

|  Name                | Description                                                                                              | Type                               | Default Value (from `PayPalAppearanceDefaults`)   |
| ---------------------|----------------------------------------------------------------------------------------------------------|------------------------------------|---------------------------------------------------|
| `loader`             | Defines the appearance of the loading indicator shown when the widget is processing or loading content.  | `LoaderAppearance`                 | `LoaderAppearanceDefaults.appearance()`           |

---

**Note:**
*   The PayPal button itself follows strict branding guidelines from PayPal and is generally not customizable beyond what the PayPal SDK or web view provides.
*   The `LoaderAppearance` itself would have its own detailed documentation explaining its configurable attributes.