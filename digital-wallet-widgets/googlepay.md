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
@Composable
fun GooglePayWidget(
    modifier: Modifier = Modifier,
    config: GooglePayWidgetConfig,
    appearance: GooglePayWidgetAppearance = GooglePayAppearanceDefaults.appearance(),
    tokenRequest: (tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit,
    loadingDelegate: WidgetLoadingDelegate? = null,
    completion: (Result<ChargeResponse>) -> Unit
)  {...}
```

The following sample code example demonstrates how to use the widget in your application:

```Kotlin
// Initialize the GooglePayWidget
GooglePayWidget(
    modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp), // optional
    config = GooglePayWidgetConfig(
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
    ),
    appearance = currentOrDefaultAppearance,
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
    }
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

| Name                  | Definition                                                                               | Type                                                                        | Mandatory/Optional |
| :-------------------- | :--------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------- | :----------------- |
| modifier              |  Compose modifier for container modifications                                            | `androidx.compose.ui.Modifier`                                              | Optional           |
| config                |  The configuration details required for the Google Pay widget                            | `GooglePayWidgetConfig`                                                     | Mandatory          |
| appearance            |  Customization options for the visual appearance of the widget                           | `GooglePayWidgetAppearance`                                                 | Optional           |
| tokenRequest          |  A callback to obtain the wallet token result asynchronously                             | `tokenRequest: (tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit`  | Mandatory          |
| completion            |  Result callback with the Charge creation API response if successful, or error if not.   | `(Result<ChargeResponse>) -> Unit`                                          | Mandatory          |


#### GooglePayWidgetConfig

| Name                  | Definition                                                                     | Type                                     | Mandatory/Optional |
| --------------------- | ------------------------------------------------------------------------------ | ---------------------------------------- |------------------  |
| isReadyToPayRequest   |  GooglePay request for determining if a user is considered ready to pay.       | `JSONObject` → `IsReadyToPayRequest`     | Mandatory          |
| paymentRequest        |  GooglePay request for providing necessary information to support a payment.   | `JSONObject` → `PaymentDataRequest`      | Mandatory          |

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

**Parent sealed class for exceptions thrown when there's an error related to Google Pay integration.**

| Exception                             | Description                                                                                                                                  | Key Properties / Parameters |
| :------------------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------- |
| `CapturingChargeException`            | Exception thrown when there is an error capturing the charge for Google Pay. The displayable message is derived from the error response.       | `error: ApiErrorResponse`   |
| `InitialisationException`           | Exception thrown when there is an initialization error related to Google Pay.                                                                | `displayableMessage: String`|
| `ResultException`                   | Exception thrown when there is a result error related to Google Pay from Paydock's side after Google Pay.                                      | `displayableMessage: String`|
| `CancellationException`             | Exception thrown when there is a user-initiated cancellation from Paydock's UI (e.g. close button). This is distinct from `SDKException.CancelledBySdk`. | `displayableMessage: String`|
| `ParseException`                      | Represents an exception that occurs during the parsing of data from an API call, typically JSON.                                             | `displayableMessage: String`, `errorBody: String?` |
| `InitialisationWalletTokenException`  | Exception thrown when there is an error during the initialisation of the Google Pay wallet token.                                            | `displayableMessage: String`|
| `UnknownException`                  | Exception thrown when there is an unknown error related to Google Pay that doesn't fit into other more specific categories.                  | `displayableMessage: String`|

---

### `SDKException` (Sealed Child of `GooglePayException`)

**Represents exceptions that originate directly from the Google Pay SDK's status codes. Each specific exception within this sealed class maps to a `CommonStatusCode`.**

| Exception                             | Description                                                                                                                                   | Key Parameters                  |
| :------------------------------------ | :-------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------ |
| `  ↳ CancelledBySdk`                 | Corresponds to `CommonStatusCodes.CANCELED`. The Google Pay flow was not completed successfully. This could be due to various reasons, not necessarily an explicit user cancellation. | `displayableMessage: String`, `statusCodeString: String` |
| `  ↳ NetworkError`                   | Corresponds to `CommonStatusCodes.NETWORK_ERROR`. A network issue prevented the Google Pay operation from completing.                           | `displayableMessage: String`, `statusCodeString: String` |
| `  ↳ Timeout`                        | Corresponds to `CommonStatusCodes.TIMEOUT`. The Google Pay operation timed out.                                                                | `displayableMessage: String`, `statusCodeString: String` |
| `  ↳ DeveloperError`                 | Corresponds to `CommonStatusCodes.DEVELOPER_ERROR`. An issue with the integration configuration or parameters. The displayable message should be user-friendly. | `displayableMessage: String`, `statusCodeString: String` |
| `  ↳ ServiceError`                   | Corresponds to generic errors like `CommonStatusCodes.INTERNAL_ERROR`, `CommonStatusCodes.ERROR`, `CommonStatusCodes.INTERRUPTED`. An unexpected or internal error occurred within the Google Pay services. | `displayableMessage: String`, `statusCodeString: String` |
| `  ↳ PlayServicesError`              | Corresponds to errors related to Google Play Services availability or account issues (e.g., `SERVICE_VERSION_UPDATE_REQUIRED`, `SERVICE_DISABLED`, etc.). | `displayableMessage: String`, `statusCodeString: String` |
| `  ↳ ResolutionFailed`               | Corresponds to `CommonStatusCodes.RESOLUTION_REQUIRED` when the resolution fails or cannot be launched.                                       | `displayableMessage: String`, `statusCodeString: String` |
| `  ↳ UnknownSdkException`            | For any other unhandled or unknown status codes from the Google Pay SDK.                                                                      | `displayableMessage: String`, `statusCodeString: String` |


### 9. Widget Styling

Defines the visual appearance for the `GooglePayWidget`. This focuses on the styling of the official Google Pay button (its corner radius and type) and the loading indicator displayed during payment processing.

#### Appearance Contract

The `GooglePayWidgetAppearance` class encapsulates the configurable style properties for the widget.

```Kotlin
@Immutable
class GooglePayWidgetAppearance(
    val cornerRadius: Dp,
    val type: ButtonType,
    val loader: LoaderAppearance
)
```

#### Default Appearance & Customisation

You can create a custom `GooglePayWidgetAppearance` by specifying the corner radius, button type, and loader appearance. The Google Pay button itself also adapts to light/dark system themes automatically.

##### Using Default Appearance


```Kotlin
    GooglePayWidget( 
        ...
        appearance = GooglePayAppearanceDefaults.appearance() // Uses the default appearance
    )
```

##### Customising Appearance

You can create a custom `GooglePayWidgetAppearance` by specifying the corner radius, button type, and loader appearance. The Google Pay button itself also adapts to light/dark system themes automatically.

```Kotlin
@Composable 
fun MyCustomGooglePayScreen() { 
    // Create appearance by using provided defaults, with custom changes
    val customAppearanceFromDefaults = GooglePayAppearanceDefaults.appearance().copy(
        cornerRadius = 0.dp, // Make it square 
        type = ButtonType.Checkout, // Change button type 
        loader = LoaderAppearanceDefaults.appearance().copy( 
            // Example: Change loader color color = Color.Blue 
        ) 
    )

    // Alternatively, create entirely from scratch:
    val completelyCustomAppearance = GooglePayWidgetAppearance(
        cornerRadius = 12.dp,
        type = ButtonType.Subscribe,
        loader = LoaderAppearance( // Assuming constructor or defaults exist
            color = Color.Green
            // ... other LoaderAppearance properties
        )
    )

    GooglePayWidget(
        ...
        appearance = customAppearanceFromDefaults, // Use your custom appearance
    )
}
```

#### Style Attributes

The following attributes can be configured within `GooglePayWidgetAppearance`:

 Name                | Description                                                                                              | Type                               | Default Value (from `GooglePayAppearanceDefaults`)   |
---------------------|----------------------------------------------------------------------------------------------------------|------------------------------------|-----------------------------------------------------------|
 `cornerRadius`      | The corner radius for the Google Pay button.                                                             | `androidx.compose.ui.unit.Dp`      | `ButtonAppearanceDefaults.ButtonCornerRadius`             |
 `type`              | The type of the Google Pay button, influencing its text (e.g., "Pay", "Buy", "Checkout", "Donate", "Order", "Subscribe"). See [Google Pay Brand Guidelines](https://developers.google.com/pay/api/web/guides/brand-guidelines#style). | `com.google.pay.button.ButtonType`                             | `ButtonType.Pay`                                                                                                 |
 `loader`            | Defines the appearance of the loading indicator shown when the widget is processing or loading content.  | `LoaderAppearance`         | `LoaderAppearanceDefaults.appearance()`           |

---

**Note:**
*   The `ButtonType` is from the official `com.google.pay.button` library. The button's theme (light/dark) is automatically handled based on `isSystemInDarkTheme()`.
*   Refer to the [Google Pay Brand Guidelines](https://developers.google.com/pay/api/web/guides/brand-guidelines) for proper usage of button types and other branding requirements
*   The `LoaderAppearance` would have its own detailed documentation.
