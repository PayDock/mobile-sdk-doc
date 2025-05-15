# Coles Pay Widget

The Coles Pay Widget integrates with Coles Pay using a WebView component. This handles the communication between Coles Pay and the SDK. Once completed, the WebView component returns the `ColesPayOrderId`.

![Coles Pay View](/img/ColesPay.png)

## iOS

## Android

## How to use the ColesPayWidget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `ColesPayWidget` composable in your application. The widget facilitates the payment using Coles Pay services.

The following sample code demonstrates the definition of the `ColesPayWidget`:

```Kotlin
fun ColesPayWidget(
    modifier: Modifier = Modifier,
    enabled: Boolean = true,
    clientId: String,
    token: (onTokenReceived: (String) -> Unit) -> Unit,
    loadingDelegate: WidgetLoadingDelegate? = null,
    completion: (Result<String>) -> Unit
) {...}
```

The following sample code demonstrates how you can use the widget in your application:

```Kotlin
// Initialize the ColesPayWidget
ColesPayWidget(
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
    result.onSuccess { colesPayOrderId ->
        // Handle success - Update UI or perform actions
        Log.d("ColesPayWidget", "Payment successful. Order ID: $colesPayOrderId")
    }.onFailure { exception ->
        // Handle failure - Show error message or take appropriate action
        Log.e("ColesPayWidget", "Payment failed. Error: ${exception.message}")
    }
}
```

### 2. Parameter definitions

This subsection describes the various parameters required by the `ColesPayWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `ColesPayWidget`.

#### ColesPayWidget

| Name                  | Definition                                                                        | Type                                              | Mandatory/Optional |
| :-------------------- | :-------------------------------------------------------------------------------- | :------------------------------------------------ | :----------------- |
| modifier              |  Compose modifier for container modifications                                     | `androidx.compose.ui.Modifier`                    | Optional           |
| enabled               |  A boolean to enable or disable the payment button                                | Boolean                                           | Optional           |
| clientId              |  A merchant supplied client ID                                                    | String                                            | Mandatory          |
| token                 |  A callback to obtain the wallet token asynchronously                             | `(onTokenReceived: (String) -> Unit) -> Unit`     | Mandatory          |
| loadingDelegate       |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown.                     | `WidgetLoadingDelegate`                           | Optional          |
| completion            |  Result callback with the *ColesPayOrderId* if successful, or error if not.         | `(Result<ChargeResponse>) -> Unit`                | Mandatory          |

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

The `completion` callback is invoked after the payment operation is completed. It receives a `Result<String>` where String represents the *ColesPayOrderId* if the payment is successful. The callback handles the outcome of the payment operation. To obtain the `wallet_token`, follow the instructions in the [generate a wallet_token](/digital-wallet-widgets/wallettoken.md) section of this guide.  

### 4. Error/Exceptions Mapping

The following describes Coles Pay exceptions that can be thrown. 

```Kotlin
FetchingUrlException(error: ApiErrorResponse) : ColesPayException(error.displayableMessage)
WebViewException(code: Int?, displayableMessage: String) : ColesPayException(displayableMessage)
CancellationException(displayableMessage: String) : ColesPayException(displayableMessage)
ParseException(displayableMessage: String, val errorBody: String?) : ColesPayException(displayableMessage)
UnknownException(displayableMessage: String) : ColesPayException(displayableMessage)
```

| Exception                 | Description                                                                                   | Error Model         |
| :------------------------ | :-------------------------------------------------------------------------------------------- | :------------------ |
| FetchingUrlException      |  Exception thrown when there is an error fetching the URL for Coles Pay.                      |  ColesPayError      |
| WebViewException          |  Exception thrown when there is an error while communicating with a WebView.                  |  ColesPayError      |
| CancellationException     |  Exception thrown when there is a cancellation error related to Coles Pay.                    |  ColesPayError      |
| ParseException            |  Exception thrown when parsing of data from an API call, typically JSON.                      |  ColesPayError      |
| UnknownException          |  Exception thrown when there is an unknown error related to Coles Pay.                        |  ColesPayError      |



