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

The following definitions provide a more detailed overview of the parameters used in the Paypal initialisation, and the potential error messages that may occur.

#### MobileSDK.ChargeResponse
| Name     | Definition                                       | Type          | Mandatory/Optional |
| :------- | :----------------------------------------------- | :------------ | :----------------  |
| status   |  Status of a PayPal payment after completion.    | Swift.String  | Mandatory          |
| amount   |  The amount of money that was charged            | Swift.Decimal | Mandatory          |
| currency |  Currency of the charge.                         | Swift.String  | Mandatory          |

#### MobileSDK.PayPalError
| Name          | Error message                                    | Type  |
| :------------ | :----------------------------------------------- | :---- |
| requestFailed |  Failure of SDK of internal rest API call.       | Error | 
| webViewFailed |  Internal error of the PayPal WebView widget.    | Error |

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
    modifier: Modifier,
    token: (onTokenReceived: (String) -> Unit) -> Unit,
    requestShipping: Boolean,
    completion: (Result<ChargeResponse>) -> Unit
) {...}
```

The following sample code example demonstrates the usage within your application:

```Kotlin
// Initialize the PayPalWidget
PayPalWidget(
    modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp), // optional
    token = { callback ->
        // Obtain Wallet Token asynchronously using the callback
        // Example: Initiate Wallet Transaction to retrieve the wallet token
        val walletToken = // ... retrieve the token
        callback(walletToken)
    },
    requestShipping = false //optional (default: true)
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

| Name                  | Definition                                                                               | Type                                              | Mandatory/Optional |
| :-------------------- | :--------------------------------------------------------------------------------------- | :------------------------------------------------ | :----------------- |
| modifier              |  Compose modifier for container modifications                                            | `androidx.compose.ui.Modifier`                    | Optional           |
| token                 |  A callback to obtain the wallet token asynchronously                                    | `(onTokenReceived: (String) -> Unit) -> Unit`     | Mandatory          |
| requestShipping       |  Flag passed to determine if PayPal will ask the user for their shipping address.        | Boolean (default = `true`)                        | Optional           |
| completion            |  Result callback with the Charge creation API response if successful, or error if not.   | `(Result<ChargeResponse>) -> Unit`                | Mandatory          |

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

The `token` callback is used to obtain the wallet token asynchronously. It receives a callback function `(onTokenReceived: (String) -> Unit)` as a parameter, which should be invoked with the wallet token once it's obtained.

#### Completion Callback

The `completion` callback is invoked after the payment operation is completed. It receives a `Result<ChargeResponse>` if the payment is successful. The callback is used to handle the outcome of the payment operation.

#### 4. Error/Exceptions Mapping

The following describes PayPal exceptions that can be thrown. 

```Kotlin
FetchingUrlException(error: ApiErrorResponse) : PayPalException(error.displayableMessage)
CapturingChargeException(error: ApiErrorResponse) : PayPalException(error.displayableMessage)
WebViewException(code: Int?, displayableMessage: String) : PayPalException(displayableMessage)
CancellationException(displayableMessage: String) : PayPalException(displayableMessage)
UnknownException(displayableMessage: String) : PayPalException(displayableMessage)
```

| Exception                 | Description                                                                                   | Error Model       |
| :------------------------ | :-------------------------------------------------------------------------------------------- | :---------------- |
| FetchingUrlException      |  Exception thrown when there is an error fetching the URL for PayPal.                         |  PayPalError      |
| CapturingChargeException  |  Exception thrown when there is an error capturing the charge for PayPal.                     |  PayPalError      |
| WebViewException          |  Exception thrown when there is an error while communicating with a WebView.                  |  PayPalError      |
| CancellationException     |  Exception thrown when there is a cancellation error related to PayPal.                       |  PayPalError      |
| UnknownException          |  Exception thrown when there is an unknown error related to PayPal.                           |  PayPalError      |

