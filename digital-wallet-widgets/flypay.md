# FlyPay Widget

The FlyPay Widget integrates with FlyPay using a WebView component. This handles the communication between FlyPay and the SDK. Once completed, the WebView component returns the `FlyPayOrderId`.

![FlyPay View](/img/FlyPay.png)

## iOS

## How to use the FlyPayWidget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `FlyPayWidget` SwiftUI view in your application. The widget facilitates the payment using FlyPay services.

The following sample code demonstrates the definition of the `FlyPayWidget`:

```Swift
FlyPayWidget(
    flyPayToken: (@escaping (String) -> Void) -> Void,
    completion: (Result<ChargeResponse, FlyPayError>) -> Void)
```

The following sample code demonstrates how you can use the widget in your application:

```Swift
struct FlyPayExampleView: View {
    @StateObject private var viewModel = FlyPayExampleVM()
    var body: some View {
        FlyPayWidget { onFlyPayButtonTap in
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
| flyPayToken           |  A callback to obtain the wallet token asynchronously                             | `(@escaping (String) -> Void) -> Void`            | Mandatory          |
| completion            |  Result callback with the *FlyPayOrderId* if successful, or error if not.         | `(Result<ChargeResponse, FlyPayError>) -> Void`   | Mandatory          |

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

This section provides a step-by-step guide on how to initialize and use the `FlyPayWidget` composable in your application. The widget facilitates the payment using FlyPay services.

The following sample code demonstrates the definition of the `FlyPayWidget`:

```Kotlin
fun FlyPayWidget(
    modifier: Modifier,
    token: (onTokenReceived: (String) -> Unit) -> Unit,
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
| token                 |  A callback to obtain the wallet token asynchronously                             | `(onTokenReceived: (String) -> Unit) -> Unit`     | Mandatory          |
| completion            |  Result callback with the *FlyPayOrderId* if successful, or error if not.       | `(Result<ChargeResponse>) -> Unit`                | Mandatory          |

### 3. Callback Explanation

#### Token Callback

The `token` callback obtains the wallet token asynchronously. It receives a callback function `(onTokenReceived: (String) -> Unit)` as a parameter, which you must invoke with the wallet token once it is obtained. 

#### Completion Callback

The `completion` callback is invoked after the payment operation is completed. It receives a `Result<String>` where String represents the *FlyPayOrderId* if the payment is successful. The callback handles the outcome of the payment operation. To obtain the `wallet_token`, follow the instructions in the [generate a wallet_token](/digital-wallet-widgets/wallettoken.md) section of this guide.  

### 4. Error/Exceptions Mapping

The following describes FlyPay exceptions that can be thrown. 

```Kotlin
FetchingUrlException(error: ApiErrorResponse) : FlyPayException(error.displayableMessage)
WebViewException(code: Int?, displayableMessage: String) : FlyPayException(displayableMessage)
CancellationException(displayableMessage: String) : FlyPayException(displayableMessage)
UnknownException(displayableMessage: String) : FlyPayException(displayableMessage)
```

| Exception                 | Description                                                                                   | Error Model       |
| :------------------------ | :-------------------------------------------------------------------------------------------- | :---------------- |
| FetchingUrlException      |  Exception thrown when there is an error fetching the URL for FlyPay.                         |  FlyPayError      |
| WebViewException          |  Exception thrown when there is an error while communicating with a WebView.                  |  FlyPayError      |
| CancellationException     |  Exception thrown when there is a cancellation error related to FlyPay.                       |  FlyPayError      |
| UnknownException          |  Exception thrown when there is an unknown error related to FlyPay.                           |  FlyPayError      |



