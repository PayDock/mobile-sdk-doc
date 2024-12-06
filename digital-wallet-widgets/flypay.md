# Flypay Widget

The Flypay Widget integrates with Flypay using a WebView component. This handles the communication between Flypay and the SDK. Once completed, the WebView component returns the `FlyPayOrderId`.

![FlyPay View](/img/FlyPay.png)

## iOS

## How to use the FlyPayWidget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `FlyPayWidget` SwiftUI view in your application. The widget facilitates the payment using Flypay services.

The following sample code demonstrates the definition of the `FlyPayWidget`:

```Swift
FlyPayWidget(
    clientId: String,
    flyPayToken: (@escaping (String) -> Void) -> Void,
    completion: (Result<ChargeResponse, FlyPayError>) -> Void)
```

The following sample code demonstrates how you can use the widget in your application:

```Swift
struct FlyPayExampleView: View {
    @StateObject private var viewModel = FlyPayExampleVM()
    var body: some View {
        FlyPayWidget(clientId: <merchantClientId>) { onFlyPayButtonTap in
            // Example: Initiate Wallet Transaction to retrieve the wallet token
            viewModel.initializeWalletCharge(completion: onFlyPayButtonTap)
        } completion: { result in
            switch result {
            case .success(let chargeResponse): // Handle success
            case .failure(let error): // Handle error
            }
        }
    }
}
```

### 2. Parameter definitions

This subsection describes the parameters required by the `FlyPayWidget` SwiftUI view. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `FlyPayWidget`.

#### FlyPayWidget

| Name                  | Definition                                                                        | Type                                              | Mandatory/Optional |
| :-------------------- | :-------------------------------------------------------------------------------- | :------------------------------------------------ | :----------------- |
| clientId              |  A merchant supplied client ID                                                    | String                                            | Mandatory          |
| flyPayToken           |  A callback to obtain the wallet token asynchronously                             | `(@escaping (String) -> Void) -> Void`            | Mandatory          |
| completion            |  Result callback with the *FlyPayOrderId* if successful, or error if not.         | `(Result<ChargeResponse, FlyPayError>) -> Void`   | Mandatory          |

### MobileSDK.FlyPayError

| Name                      | Description                                                            | Error Result            |
| :------------------------ | :--------------------------------------------------------------------- | :---------------------- |
| errorFetchingFlyPayOrder  |  Error thrown when fetching FlyPay order ID fails.                     |  ErrorRes               |
| flyPayUrlError            |  Error thrown when generating the FlyPay URL.                          |  nil                    |
| webViewFailed             |  Error thrown when there is an issue communicating with the WebView.   |  NSError                |
| unknownError              |  Error thrown when there is an unknown error related to FlyPay.        |  nil                    |

### 3. Callback Explanation

#### Token Callback

> **Note**:
>
> The `flyPayToken` callback obtains the wallet token asynchronously. It receives a callback function `(@escaping (String) -> Void) -> Void` as a parameter, which you must invoke with the `wallet_token` once it is obtained. To obtain the `wallet_token`, follow the instructions in the [generate a wallet_token](/digital-wallet-widgets/wallettoken.md) section of this guide.  

#### Completion Callback

The `completion` callback is invoked after the payment operation is completed. It receives a `Result<ChargeResponse, FlyPayError>` where ChargeResponse contains the *FlyPayOrderId* if the payment is successful. The callback handles the outcome of the payment operation.

## Android

## How to use the FlyPayWidget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `FlyPayWidget` composable in your application. The widget facilitates the payment using Flypay services.

The following sample code demonstrates the definition of the `FlyPayWidget`:

```Kotlin
fun FlyPayWidget(
    modifier: Modifier,
    enabled: Boolean = true,   
    clientId: String,
    token: (onTokenReceived: (String) -> Unit) -> Unit,
    loadingDelegate: WidgetLoadingDelegate?,
    completion: (Result<String>) -> Unit
) {...}
```

The following sample code demonstrates how you can use the widget in your application:

```Kotlin
// Initialize the FlyPayWidget
FlyPayWidget(
    modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp), // optional
    clientId = merchantClientId,
    token = { callback ->
        // Obtain Wallet Token asynchronously using the callback
        // Example: Initiate Wallet Transaction to retrieve the wallet token
        val walletToken = // ... retrieve the token
        callback(walletToken)
    }
) { result ->
    // Handle the result of the payment operation
    result.onSuccess { flyPayOrderId ->
        // Handle success - Update UI or perform actions
        Log.d("FlyPayWidget", "Payment successful. Order ID: $flyPayOrderId")
    }.onFailure { exception ->
        // Handle failure - Show error message or take appropriate action
        Log.e("FlyPayWidget", "Payment failed. Error: ${exception.message}")
    }
}
```

### 2. Parameter definitions

This subsection describes the various parameters required by the `FlyPayWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `FlyPayWidget`.

#### FlyPayWidget

| Name                  | Definition                                                                        | Type                                              | Mandatory/Optional |
| :-------------------- | :-------------------------------------------------------------------------------- | :------------------------------------------------ | :----------------- |
| modifier              |  Compose modifier for container modifications                                     | `androidx.compose.ui.Modifier`                    | Optional           |
| enabled              |  A boolean to enable or disable the payment button                                 | Boolean                                           | Optional           |
| clientId              |  A merchant supplied client ID                                                    | String                                            | Mandatory          |
| token                 |  A callback to obtain the wallet token asynchronously                             | `(onTokenReceived: (String) -> Unit) -> Unit`     | Mandatory          |
| loadingDelegate       |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown.                     | `WidgetLoadingDelegate`                           | Optional          |
| completion            |  Result callback with the *FlyPayOrderId* if successful, or error if not.         | `(Result<ChargeResponse>) -> Unit`                | Mandatory          |

### 3. Callback Explanation

#### Token Callback

The `token` callback obtains the wallet token asynchronously. It receives a callback function `(onTokenReceived: (String) -> Unit)` as a parameter, which you must invoke with the wallet token once it is obtained. 

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

The `completion` callback is invoked after the payment operation is completed. It receives a `Result<String>` where String represents the *FlyPayOrderId* if the payment is successful. The callback handles the outcome of the payment operation. To obtain the `wallet_token`, follow the instructions in the [generate a wallet_token](/digital-wallet-widgets/wallettoken.md) section of this guide.  

### 4. Error/Exceptions Mapping

The following describes Flypay exceptions that can be thrown. 

```Kotlin
FetchingUrlException(error: ApiErrorResponse) : FlyPayException(error.displayableMessage)
WebViewException(code: Int?, displayableMessage: String) : FlyPayException(displayableMessage)
CancellationException(displayableMessage: String) : FlyPayException(displayableMessage)
UnknownException(displayableMessage: String) : FlyPayException(displayableMessage)
```

| Exception                 | Description                                                                                   | Error Model       |
| :------------------------ | :-------------------------------------------------------------------------------------------- | :---------------- |
| FetchingUrlException      |  Exception thrown when there is an error fetching the URL for Flypay.                         |  FlyPayError      |
| WebViewException          |  Exception thrown when there is an error while communicating with a WebView.                  |  FlyPayError      |
| CancellationException     |  Exception thrown when there is a cancellation error related to Flypay.                       |  FlyPayError      |
| UnknownException          |  Exception thrown when there is an unknown error related to Flypay.                           |  FlyPayError      |



