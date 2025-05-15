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