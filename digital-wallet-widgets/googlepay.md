# Google Pay Widget

The MobileSDK provides a GooglePayWidget composable widget that you can use to interact with GooglePay. The button initializes GooglePay, handles the payment requests, and finalizes the charge through Paydock. The final result is then returned as a callback.

![GooglePay View](/img/GPay.png)

## How to use the GooglePay widget

### 1. Overview

The GooglePay widget facilitates payments using GooglePay services. This section provides a step-by-step guide on how to initialize and use the `GooglePayWidget` composable in your application. The widget facilitates the payment using GooglePay services.

> **Note**:
>
> When the customer taps the Payment button for your Digital Wallet, the SDK triggers a callback to the Merchant app requesting a `wallet_token`. You must perform a wallet initialization request to receive your `wallet_token`. To do this, follow the instructions in the [generate a wallet_token](/digital-wallet-widgets/wallettoken.md) section of this guide.   

The following sample code demonstrates the definition of the `GooglePayWidget`:

```Kotlin
fun GooglePayWidget(
    modifier: Modifier,
    tokenRequest: (tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit,
    isReadyToPayRequest: JSONObject,
    paymentRequest: JSONObject,
    completion: (Result<ChargeResponse>) -> Unit
) {...}
```

The following sample code example demonstrates how to use the widget in your application:

```Kotlin
// Initialize the GooglePayWidget
GooglePayWidget(
    modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp), // optional
    tokenRequest = { callback ->
        // show widget loader
        tokenResult.onSuccess { result ->
            // Handle token success
            val walletToken = result.token // ... retrieve the token
        }.onFailure {
            // Handle token failure success
        }
    },
    isReadyToPayRequest = PaymentsUtil.createIsReadyToPayRequest(),
    paymentRequest = PaymentsUtil.createGooglePayRequest(
        amount = BigDecimal(AMOUNT),
        amountLabel = AMOUNT_LABEL,
        currencyCode = CURRENCY_CODE,
        countryCode = COUNTRY_CODE,
        merchantName = MERCHANT_NAME,
        merchantIdentifier = MERCHANT_IDENTIFIER,
        shippingAddressRequired = true,
        shippingAddressParameters = shippingAddressParameters
    )
) { result ->
    // Handle the result of the payment operation
    result.onSuccess { chargeResponse ->
        // Handle success - Update UI or perform actions
        Log.d("GooglePayWidget", "Payment successful.")
    }.onFailure { exception ->
        // Handle failure - Show error message or take appropriate action
        Log.e("GooglePayWidget", "Payment failed. Error: ${exception.message}")
    }
}
```

### 2. Parameter definitions

This subsection describes the parameters required by the `GooglePayWidget` composable. It defines the purpose of each parameter and its significance in configuring the behavior of the `GooglePayWidget`.

#### GooglePayWidget

| Name                  | Definition                                                                               | Type                                              | Mandatory/Optional |
| :-------------------- | :--------------------------------------------------------------------------------------- | :------------------------------------------------ | :----------------- |
| modifier              |  Compose modifier for container modifications                                            | `androidx.compose.ui.Modifier`                    | Optional           |
| tokenRequest          |  A callback to obtain the wallet token result asynchronously                      | `tokenRequest: (tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit`     | Mandatory          |
| isReadyToPayRequest   |  GooglePay request for determining if a user is considered ready to pay.                | `JSONObject` → `IsReadyToPayRequest`              | Mandatory          |
| paymentRequest        |  GooglePay request for providing necessary information to support a payment.            | `JSONObject` → `PaymentDataRequest`               | Mandatory          |
| completion            |  Result callback with the Charge creation API response if successful, or error if not.   | `(Result<ChargeResponse>) -> Unit`                | Mandatory          |

### 3. How to create GooglePay request objects 

Create Google Pay request objects and mappings, which are required by the `GooglePayWidget` widget. Simply use the `util` provided by the MobileSDK (`PaymentsUtil`) with the accompanying helper methods. 

The following sample code example demonstrates how to create request objects:

### 4. How to create an IsReadyToPayRequest

You can create a request to check if the user is ready to pay with GooglePay.

```Kotlin
object PaymentsUtil {

/**
  * Create a request to check if the user is ready to pay with Google Pay.
  */  
  fun createIsReadyToPayRequest(
      allowedCardAuthMethods: List<String> = ALLOWED_CARD_AUTH_METHODS,
      allowedCardNetworks: List<String> = ALLOWED_CARD_NETWORKS,
      billingAddressRequired: Boolean = true
  ): JSONObject {...}

  ... //Other helper methods
}
```

```Kotlin
PaymentsUtil.createIsReadyToPayRequest()
// OR 
PaymentsUtil.createIsReadyToPayRequest(billingAddressRequired = false)
```

### 5. How to create a PaymentDataRequest

You can create a GooglePay payment request based on provided paramaters. 

```Kotlin
object PaymentsUtil {

/**
  * Create a Google Pay payment request based on provided parameters.
  */  
  fun createGooglePayRequest(
      amount: BigDecimal, // required
      amountLabel: String, // required
      countryCode: String, // required
      currencyCode: String, // required
      merchantName: String? = null, // required
      merchantIdentifier: String, // required
      allowedCardAuthMethods: List<String> = ALLOWED_CARD_AUTH_METHODS,
      allowedCardNetworks: List<String> = ALLOWED_CARD_NETWORKS,
      billingAddressRequired: Boolean = true,
      shippingAddressRequired: Boolean = false,
      shippingAddressParameters: JSONObject? = null
  ): JSONObject {...}

  ... //Other helper methods
}
```

```Kotlin
// Showing required parameters
PaymentsUtil.createGooglePayRequest(
	amount = BigDecimal(1.00),
	amountLabel = "amount label",
	currencyCode = "GBP",
	countryCode = "UK",
	merchantIdentifier = "<merchant Id>"
)
// OR (customising optional parameters)
PaymentsUtil.createGooglePayRequest(
	... required parameters
    shippingAddressRequired = true,
    shippingAddressParameters = JSONObject().apply {
        put("phoneNumberRequired", true)
        put("allowedCountryCodes", JSONArray(listOf("US", "GB", "AU")))
    }
)
```

### 6. Request object definitions

The following references provide more detailed definitions of the IsReadyToPayRequest and PaymentDataRequest used in your GooglePay widget.

#### IsReadyToPayRequest (JSONObject): PaymentsUtil.createIsReadyToPayRequest()
Reference: [IsReadyToPayRequest](https://developers.google.com/pay/api/android/reference/request-objects#IsReadyToPayRequest)

| Name                     | Definition                                                                        | Type               | Mandatory/Optional |
| :----------------------- | :-------------------------------------------------------------------------------- | :----------------- | :----------------- |
| allowedCardAuthMethods   |  List of allowed card authentication methods ("PAN_ONLY", "CRYPTOGRAM_3DS")       | List<String>       | Optional          |
| allowedCardNetworks      |  List of allowed card networks ("AMEX", "DISCOVER", "JCB", "MASTERCARD", "VISA")  | List<String>       | Optional          |
| billingAddressRequired   |  Indicates whether billing address is required     | Boolean | Mandatory          | Boolean            | Optional          |

#### PaymentDataRequest (JSONObject): PaymentsUtil.createGooglePayRequest()
Reference: [PaymentDataRequest](https://developers.google.com/android/reference/com/google/android/gms/wallet/PaymentDataRequest)

| Name                        | Definition                                                                        | Type               | Mandatory/Optional |
| :-------------------------- | :-------------------------------------------------------------------------------- | :----------------- | :----------------- |
| amount                      |  The charge amount                                                                | BigDecimal         | Mandatory          |
| amountLabel                 |  The label for the payment amount                                                 | String             | Mandatory          |
| countryCode                 |  The country code                                                                 | String             | Mandatory          | 
| currencyCode                |  The currency code                                                                | String             | Mandatory          |
| merchantName                |  The merchant name                                                                | String             | Optional           |
| merchantIdentifier          |  The merchant identifier from Paydock                                             | String             | Mandatory          |
| allowedCardAuthMethods      |  List of allowed card authentication methods ("PAN_ONLY", "CRYPTOGRAM_3DS")       | List<String>       | Optional           |
| allowedCardNetworks         |  List of allowed card networks ("AMEX", "DISCOVER", "JCB", "MASTERCARD", "VISA")  | List<String>       | Optional           |
| billingAddressRequired      |  Indicates whether billing address is required                                    | Boolean            | Optional           |
| shippingAddressRequired     |  Indicates whether shipping address is required                                   | Boolean            | Optional           |
| shippingAddressParameters   |  Parameters for shipping address, if required.                                    | `JSONObject`       | Optional           |

#### 7. ChargeResponse

This subsection outlines the structure of the result or response object returned by the `GooglePayWidget` composable. It details the format and components of the object, enabling you to handle the response effectively within your application.

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
| data           |  Charge data result containing actual data regarding charge such as amount, currency, and so on.            | `ChargeResponse.Data`      |

#### ChargeResponse.ChargeData

| Name           | Definition                                | Type                 | 
| :------------- | :---------------------------------------- | :------------------- | 
| status         |  Charge result status as a string         | String               | 
| amount         |  Charge amount that was charged           | `BigDecimal`         |
| currency       |  Charge currency that was used            | String               |

### 7. Callback Explanation

#### Token Callback

The `token` callback obtains the wallet token result asynchronously. It receives a callback function `tokenRequest: (tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit` as a parameter, which you must invoke with the result of the wallet token API request once it is obtained. 

The `WalletTokenResult` acts as a token wrapper containing the actual token result.

#### Completion Callback

The `completion` callback is invoked after the payment operation is completed. It receives a `Result<ChargeResponse>` if the payment is successful. The callback is used to handle the outcome of the payment operation.

### 8. Error/Exceptions Mapping

The following describes the Google Pay exceptions that can be thrown. 

```Kotlin
CapturingChargeException(error: ApiErrorResponse) : GooglePayException(error.displayableMessage)
PaymentRequestException(exception: Exception?) : GooglePayException(exception?.message ?: defaultMessage)
InitialisationException(displayableMessage: String) : GooglePayException(displayableMessage)
ResultException(displayableMessage: String) : GooglePayException(displayableMessage)
CancellationException(displayableMessage: String) : GooglePayException(displayableMessage)
InitialisationWalletTokenException(displayableMessage: String, val errorBody: String?) : GooglePayException(displayableMessage)
UnknownException(displayableMessage: String) : GooglePayException(displayableMessage)
```

| Exception                 | Description                                                                                   | Error Model          |
| :------------------------ | :-------------------------------------------------------------------------------------------- | :------------------- |
| CapturingChargeException  |  Exception thrown when there is an error capturing the charge for Google Pay.                 |  GooglePayError      |
| PaymentRequestException   |  Exception thrown when there is an error during the payment request process for Google Pay.   |  GooglePayError      |
| InitialisationException   |  Exception thrown when there is an initialization error related to Google Pay.                |  GooglePayError      |
| ResultException           |  Exception thrown when there is a result error related to Google Pay.                         |  GooglePayError      |
| CancellationException     |  Exception thrown when there is a cancellation error related to Google Pay.                   |  GooglePayError      |
| InitialisationWalletTokenException            |  Exception thrown when the wallet token result returns a failure result.  |  GooglePayError      |
| UnknownException          |  Exception thrown when there is an unknown error related to Google Pay.                       |  GooglePayError      |

