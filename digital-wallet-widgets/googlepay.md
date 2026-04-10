# Google Pay Widget

The MobileSDK provides a GooglePayWidget composable widget that you can use to interact with GooglePay. The button initializes GooglePay, handles the processing of information, and generates an OTT token to be used in transaction processing. The final result is then returned as a callback.

![GooglePay View](/img/GPay.png)

## How to use the GooglePay widget

### 1. Overview

The GooglePay widget facilitates payments using GooglePay services. This section provides a step-by-step guide on how to initialize and use the `GooglePayWidget` composable in your application. The widget facilitates the payment using GooglePay services.

The following sample code demonstrates the definition of the `GooglePayWidget`:

```Kotlin
@Composable
fun GooglePayWidget(
    modifier: Modifier = Modifier,
    enabled: Boolean = true,
    config: GooglePayWidgetConfig,
    appearance: GooglePayWidgetAppearance = GooglePayAppearanceDefaults.appearance(),
    loadingDelegate: WidgetLoadingDelegate? = null,
    eventDelegate: WidgetEventDelegate? = null,
    completion: (Result<GooglePayResult>) -> Unit,
    fallbackUi: @Composable (() -> Unit)? = null
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
    loadingDelegate = DELEGATE_INSTANCE, // optional - Delegate class to handle loading
    eventDelegate = EVENT_DELEGATE_INSTANCE, // optional - Delegate class to handle events
) { result ->
    // Handle the result of the payment operation
    result.onSuccess { chargeResponse ->
        // Handle success - Update UI or perform actions
        Log.d("GooglePayWidget", "OTT token created successful.")
    }.onFailure { exception ->
        // Handle failure - Show error message or take appropriate action
        Log.e("GooglePayWidget", "Process failed. Error: ${exception.message}")
    }
}
```

### 2. Parameter definitions

This subsection describes the parameters required by the `GooglePayWidget` composable. It defines the purpose of each parameter and its significance in configuring the behavior of the `GooglePayWidget`.

#### GooglePayWidget

| Name                  | Definition                                                                                       | Type                                                               | Mandatory/Optional |
| :-------------------- | :----------------------------------------------------------------------------------------------- | :------------------------------------------------------------------| :----------------- |
| modifier              |  Compose modifier for container modifications.                                                   | `androidx.compose.ui.Modifier`                                     | Optional           |
| enabled               |  Controls the enabled state of this Widget.                                                      | Boolean                                                            | Optional           |
| config                |  The configuration details required for the Google Pay widget.                                   | `GooglePayWidgetConfig`                                            | Mandatory          |
| appearance            |  Customization options for the visual appearance of the widget.                                  | `GooglePayWidgetAppearance`                                        | Optional           |
| loadingDelegate       |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown. | `WidgetLoadingDelegate`                                            | Optional           |
| eventDelegate         |  Delegate for handling widget events such as button clicks.                                      | `WidgetEventDelegate`                                              | Optional           |
| completion            |  Result callback with the Google Pay result response if successful, or error if not.             | `(Result<GooglePayResult>) -> Unit`                                | Mandatory          |
| fallbackUi            |  A `Composable` that can be passed to show if Google Pay is not available.                       | Unit                                                               | Optional           |

#### GooglePayWidgetConfig

| Name                  | Definition                                                                     | Type                                     | Mandatory/Optional |
| --------------------- | ------------------------------------------------------------------------------ | ---------------------------------------- |------------------  |
| accessToken           |  The access token used for authentication with the backend service.            | String                                   | Mandatory          |
| serviceId             |  Service ID of Google Pay service registered with Paydock.                     | String                                   | Mandatory          |
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
      billingAddressRequired: Boolean = true,
      billingAddressParameters: GooglePayBillingAddressParameters? = null,
      phoneNumberRequired: Boolean = true
  ): GooglePayIsReadyToPayRequest

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
      billingAddressParameters: GooglePayBillingAddressParameters? = null,
      shippingAddressRequired: Boolean = false,
      allowedShippingCountryCodes: List<String>? = null,
      shippingAddressParameters: JSONObject? = null,
      emailRequired: Boolean = false,
      phoneNumberRequired: Boolean = false
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
    shippingAddressParameters = GooglePayShippingAddressParameters(
        allowedCountryCodes = listOf("US", "GB", "AU")
    )
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
| billingAddressRequired   |  Indicates whether billing address is required                                    | Boolean            | Optional          |
| billingAddressParameters |  Billing address requirements                                                     | Boolean            | Optional          |
| phoneNumberRequired      |  Is phone number required                                                         | Boolean            | Optional          |

#### PaymentDataRequest (JSONObject): PaymentsUtil.createGooglePayRequest()
Reference: [PaymentDataRequest](https://developers.google.com/android/reference/com/google/android/gms/wallet/PaymentDataRequest)

| Name                        | Definition                                                                        | Type                                 | Mandatory/Optional |
| :-------------------------- | :-------------------------------------------------------------------------------- | :----------------------------------- | :----------------- |
| amount                      |  The charge amount                                                                | BigDecimal                           | Mandatory          |
| amountLabel                 |  The label for the payment amount                                                 | String                               | Mandatory          |
| countryCode                 |  The country code                                                                 | String                               | Mandatory          | 
| currencyCode                |  The currency code                                                                | String                               | Mandatory          |
| merchantName                |  The merchant name                                                                | String                               | Optional           |
| merchantIdentifier          |  The merchant identifier from Paydock                                             | String                               | Mandatory          |
| allowedCardAuthMethods      |  List of allowed card authentication methods ("PAN_ONLY", "CRYPTOGRAM_3DS")       | List<String>                         | Optional           |
| allowedCardNetworks         |  List of allowed card networks ("AMEX", "DISCOVER", "JCB", "MASTERCARD", "VISA")  | List<String>                         | Optional           |
| billingAddressRequired      |  Indicates whether billing address is required                                    | Boolean                              | Optional           |
| billingAddressParameters    |  Parameters for the billing address, if required                                  | `GooglePayBillingAddressParameters`  | Optional           |
| shippingAddressRequired     |  Indicates whether shipping address is required                                   | Boolean                              | Optional           |
| allowedShippingCountryCodes |  List of country codes allowed for shipping.                                      | List<String>                         | Optional           |
| shippingAddressParameters   |  Parameters for shipping address, if required.                                    | `GooglePayShippingAddressParameters` | Optional           |
| emailRequired               |  If email is required.                                                            | Boolean                              | Optional           |
| phoneNumberRequired         |  If phone number is required.                                                     | Boolean                              | Optional           |

#### 7. ChargeResponse

This subsection outlines the structure of the result or response object returned by the `GooglePayWidget` composable. It details the format and components of the object, enabling you to handle the response effectively within your application.

```Kotlin
data class GooglePayResult(
    val token: String?,
    val type: String,
    val email: String? = null,
    val billingAddress: GooglePayBillingAddress? = null,
    val shippingAddress: GooglePayBillingAddress? = null
)
```

| Name             | Definition                                                                       | Type                       | 
| :--------------- | :------------------------------------------------------------------------------- | :------------------------- | 
| token            |  The OTT token generated by Paydock for payment processing.                      | String                     | 
| type             |  The type of payment source associated with the token (e.g., "CARD").            | String                     |
| email            |  The email address associated with the Google Pay account, if provided.          | String                     |
| billingAddress   |  The billing address associated with the payment method, if provided.            | String                     |
| shippingAddress  |  The shipping address selected by the user, if provided.                         | String                     |

### 7. Callback Explanation

#### WidgetEventDelegate

This `eventDelegate` allows the calling app to receive notifications of user interactions within the widget, such as button clicks. This is useful for analytics and tracking purposes.

```Kotlin
interface WidgetEventDelegate {
    /**
     * Called when a widget event occurs.
     *
     * @param event The event that occurred, containing the event type and properties.
     */
    fun widgetEvent(event: Event)
}
```

##### Google Pay Events

The Google Pay Widget triggers the following events:

**Google Pay Start Checkout Event** - Triggered when the Google Pay checkout button is clicked:

```json
{
  "type": "Button",
  "properties": {
    "name": "GooglePayCheckoutButton",
    "action": "click"
  }
}
```

| Property | Description | Type | Optional/Required |
|----------|-------------|------|-------------------|
| `type` | The type of UI element that triggered the event | String | Required |
| `properties.name` | The name identifier of the specific element | String | Required |
| `properties.action` | The action performed on the element | String (Enum) | Required |

#### Completion Callback

The `completion` callback is invoked after the tokenisation operation is completed. It receives a `Result<GooglePayResult>` if a token is successfully generated. The callback is used to handle the outcome of the tokenisation operation.

### 8. Error/Exceptions Mapping

The following describes the Google Pay exceptions that can be thrown. 

**Parent sealed class for exceptions thrown when there's an error related to Google Pay integration.**

| Exception                          | Description                                                                                                                                              | Key Properties / Parameters |
| :--------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------- |
| `TokenisingGooglePayException`     | Exception thrown when there is an error creating a payment token for Google Pay. The displayable message is derived from the error response.             | `error: ApiErrorResponse`   |
| `InitialisationException`          | Exception thrown when there is an initialization error related to Google Pay.                                                                            | `displayableMessage: String`|
| `IsReadyToPayException`            | Exception thrown when the "Is Ready to Pay" check fails or returns false.                                                                                | `displayableMessage: String`|
| `ResultException`                  | Exception thrown when there is a result error related to Google Pay from Paydock's side after Google Pay.                                                | `displayableMessage: String`|
| `CancellationException`            | Exception thrown when there is a user-initiated cancellation from Paydock's UI (e.g. close button). This is distinct from `SDKException.CancelledBySdk`. | `displayableMessage: String`|
| `ParseException`                   | Represents an exception that occurs during the parsing of data from an API call, typically JSON.                                                         | `displayableMessage: String`, `errorBody: String?` |
| `UnknownException`                 | Exception thrown when there is an unknown error related to Google Pay that doesn't fit into other more specific categories.                              | `displayableMessage: String`|

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
