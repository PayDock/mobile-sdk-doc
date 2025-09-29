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
    appearance: PayPalWidgetAppearance = PayPalWidgetAppearance(),
    config: PayPalWidgetConfig
    loadingDelegate: WidgetLoadingDelegate?,
    tokenRequest: @escaping (_ tokenResult: @escaping (Result<WalletTokenResult, WalletTokenError>) -> Void) -> Void,
    completion: @escaping (Result<ChargeResponse, PayPalError>) -> Void)
    { ... }
```
In case of successful charge, the **PayPalView** returns a `ChargeResponse` that contains all the relevant information. In case of an error, the **PayPalError** object is returned with information regarding the failure.

The following is an example of a full PayPalView initialization:

```Swift
struct PayPalExampleView: View {
    var body: some View {
        VStack {
            PayPalWidget(
                appearance: PayPalWidgetAppearance(),
                config: getConfig()) { onPayPalButtonTap in
                viewModel.initializeWalletCharge(completion: onPayPalButtonTap)
            } completion: { result in
                switch result {
                case .success(let chargeResponse): // Handle successful charge response
                case .failure(let error): // Handle error
                }
            }
        }
    }
    
    private func getConfig() -> PayPalWidgetConfig {
        let accessToken = <Your access token>
        let gatewayId = <Your gateway ID>
        let config = PayPalWidgetConfig(accessToken: accessToken, gatewayId: gatewayId)
        return config
    }
}
```

### 2. Parameter Definitions

#### PayPalWidget

| Name             | Definition                                                                                       | Type                                                                                       | Mandatory/Optional |
| :--------------- | :----------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------- | :----------------- |
| viewState        |  View options that are two way fields to alter view state                                        | `ViewState`                                                                                | Optional           |
| appearance       |  Object for visual customization of the widget.                                                  | `MobileSDK.PayPalWidgetAppearance`                                                         | Optional           |
| config           |  Object for configuring the setup and behaviour of the widget.                                   | `MobileSDK.PayPalWidgetConfig`                                                             | Mandatory          |
| loadingDelegate  |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown. | `WidgetLoadingDelegate`                                                                    | Optional           |
| tokenRequest     |  A callback to obtain the wallet token asynchronously                                            | `(_ tokenResult: @escaping (Result<WalletTokenResult, WalletTokenError>) -> Void) -> Void` | Mandatory          |
| completion       |  Result callback with the Charge creation API response if successful, or error if not.           | `(Result<ChargeResponse, PayPalError>) -> Void` | Mandatory                                | Mandatory          |

The following definitions provide a more detailed overview of the parameters used in the Paypal initialisation, and the potential error messages that may occur.

#### MobileSDK.PayPalWidgetConfig

| Name                      | Description                                                                                                            | Error Result                                       |
| :------------------------ | :--------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------- |
| accessToken               |  The OAuth access token required for authenticating API requests to PayPal services.                                   |  String                                            |
| gatewayId                 |  The unique identifier for the payment gateway used to route transactions.                                             |  String                                            |
| requestShipping           |  A boolean indicating whether shipping information should be requested from the user during the PayPal checkout flow.  |  Bool                                              |
| fundingSource             |  Sets a preference for the desired payment option within the PayPal Widget.                                            |  PayPalWebPayments.PayPalWebCheckoutFundingSource  |

#### MobileSDK.ViewState

| Name                   | Definition                                                              | Type                                               | Mandatory/Optional |
| ---------------------- | ----------------------------------------------------------------------- | -------------------------------------------------- |------------------  |
| state                  |  Sets the state the widget should be placed in                          | Enum (disabled, none)                              | Mandatory          |

#### MobileSDK.ChargeResponse

| Name     | Definition                                       | Type          | Mandatory/Optional |
| :------- | :----------------------------------------------- | :------------ | :----------------  |
| status   |  Status of a PayPal payment after completion.    | Swift.String  | Mandatory          |
| amount   |  The amount of money that was charged            | Swift.Decimal | Mandatory          |
| currency |  Currency of the charge.                         | Swift.String  | Mandatory          |

#### MobileSDK.PayPalError

| Name                      | Description                                                                      | Error Result            |
| :------------------------ | :------------------------------------------------------------------------------- | :---------------------- |
| getPayPalClientId         |  Error thrown when there is error fetching the PayPal Client ID.                 |  nil                    |
| errorFetchingOrderId      |  Error thrown when there is an error fetching the Order ID for PayPal.           |  ErrorRes               |
| errorCapturingCharge      |  Error thrown when there is an error capturing the charge for PayPal.            |  ErrorRes               |
| userCancelled             |  Error thrown when user cancels the flow.                                        |  nil                    |
| initialisingWalletToken   |  Error thrown when wallet token initialization fails                             |  nil                    |
| sdkException              |  Error thrown error happens within the PayPal SDK                                |  String                 |
| unknownError              |  Error thrown when there is an unknown error related to PayPal.                  |  RequestError?          |

### 5. Widget Styling

Defines the visual appearance for specific elements within the `PayPalWidget`.
The primary visual element is the PayPal button, which has its own branding guidelines. This appearance configuration mainly focuses on the loading indicator displayed during interactions with the PayPal flow.

#### Appearance Contract

The `PayPalWidgetAppearance` class encapsulates the configurable style properties for the widget, currently centered on the loader.

```Swift
public struct PayPalWidgetAppearance: ActionButtonLoaderStylableAppearance {
    public var loader: Theme.ButtonLoader
}
```

#### Default Appearance & Customisation

A default appearance is provided by `GlobalTheme`. This configures a specific loader appearance intended to be visually compatible with the standard PayPal button.

##### Using Default Appearance

```Swift
    PayPalWidget( 
        ...
        appearance: PayPalWidgetAppearance = PayPalWidgetAppearance(),
    )
```

##### Customising Appearance

You can create a custom `PayPalWidgetAppearance` by providing a specific `LoaderAppearance` configuration if the default doesn't meet your needs or if you want to ensure consistency with other loaders in your app.

```Swift
@Composable 
struct MyCustomPayPalScreen: View { 
    private func myCustomAppearance() -> PayPalWidgetAppearance {
        let buttonLoader = Theme.ButtonLoader(spinnerColor: .blue)
        let buttonSize = PaymentButtonSize.ful
        let buttonColor = PayPalButton.gold
        let appearance = PayPalWidgetAppearance(loader: buttonLoader, buttonSize: buttonSize, buttonColor: buttonColor)
        return appearance
    }
    
    var body: some View {
        PayPalWidget( 
            ...
            appearance: PayPalWidgetAppearance = myCustomAppearance()
            ...
        )
    }
}
```

#### Style Attributes

|  Name                | Description                                                                                              | Type                                  | Default Value     |
| ---------------------|----------------------------------------------------------------------------------------------------------|---------------------------------------|-------------------|
| `buttonInsets`       | Defines the insets between the button edges and the content withing the button                           | `NSDirectionalEdgeInsets?`            | nil               |
| `buttonColor`        | Defines the color scheme of the button. Impacts both background and foreground colors.                   | `PaymentButtons.PayPalButton.Color`   | `.gold`           |
| `buttonEdges`        | Defines the type of the button corner radius.                                                            | `PaymentButtons.PaymentButtonEdges`   | `.softEdges`      |
| `buttonSize`         | Defines the size of the button and the content within it.                                                | `PaymentButtons.PaymentButtonSize`    | `.full`           |
| `buttonLabel`        | Defines the content within the button.                                                                   | `PaymentButtons.PayPalButton.Label?`  | nil               |
| `loader`             | Defines the appearance of the loading indicator shown when the widget is processing or loading content.  | `MobileSDK.Theme.ButtonLoader`        | `Color.onPrimary` |

---

**Note:**
*   The PayPal button itself follows strict branding guidelines from PayPal and is generally not customizable beyond what the PayPal SDK or web view provides.
*   The `ButtonLoader` itself would have its own detailed documentation explaining its configurable attributes.

## Android

## How to use the PayPalWidget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `PayPalWidget` composable in your application. The widget facilitates the payment using PayPal services.

> **Note**:
>
> When the customer taps the Payment button for your Digital Wallet, the SDK triggers a callback to the Merchant app requesting a `wallet_token`. You must perform a wallet initialization request to receive your `wallet_token`. To do this, follow the instructions in the [generate a wallet_token](/digital-wallet-widgets/wallettoken.md) section of this guide.  

The following sample code demonstrates the definition of the `PayPalWidget`:

```Kotlin
@Composable
fun PayPalWidget(
    modifier: Modifier = Modifier,
    enabled: Boolean = true,
    config: PayPalWidgetConfig,
    appearance: PayPalWidgetAppearance = PayPalAppearanceDefaults.appearance(),
    tokenRequest: (tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit,
    loadingDelegate: WidgetLoadingDelegate? = null,
    completion: (Result<ChargeResponse>) -> Unit,
)
```

The following sample code example demonstrates the usage within your application:

```Kotlin
// Initialize the PayPalWidget
PayPalWidget(
    modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp),
    enabled = true,
     config = PayPalWidgetConfig(
        accessToken = "your_widget_access_token",
        gatewayId = "your_paypal_gateway_id",
        requestShipping = false // Optional: set to false for digital goods
    ),
    appearance = PayPalAppearanceDefaults.appearance(), // Use default or custom appearance
    tokenRequest = { callback ->
        // Perform your wallet token request here
        viewModel.getWalletToken { tokenResult ->
            tokenResult.onSuccess { result ->
                // Handle token success
                callback(Result.success(WalletTokenResult(token = result.token)))
            }.onFailure { error ->
                // Handle token failure
                callback(Result.failure(error))
            }
        }
    },
    loadingDelegate = null, // Optional: delegate class to handle loading
) { result ->
    // Handle the result of the payment operation
    result.onSuccess { chargeResponse ->
        // Handle successful payment
        Log.d("PayPalWidget", "Payment successful: ${chargeResponse.resource.data?.amount}")
    }.onFailure { exception ->
        // Handle payment failure
        Log.e("PayPalWidget", "Payment failed: ${exception.message}")
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
| config                |  Configuration options for the PayPal widget (includes shipping preferences)                   | `PayPalWidgetConfig`                          | Mandatory          |
| appearance            |  Customization options for the visual appearance of the widget including button styling        | `PayPalWidgetAppearance`                      | Optional           |
| tokenRequest          |  A callback to obtain the wallet token result asynchronously                                    | `(tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit`     | Mandatory          |
| loadingDelegate       |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown.| `WidgetLoadingDelegate?`                       | Optional           |
| completion            |  Result callback with the Charge creation API response if successful, or error if not.          | `(Result<ChargeResponse>) -> Unit`            | Mandatory          |

#### PayPalWidgetConfig

This subsection describes the configuration parameters required by the `PayPalWidgetConfig`. This data class holds various configuration options that customize the behavior of the PayPal widget and manages PayPal transactions within the application.

The following sample code demonstrates the configuration structure:

```Kotlin
data class PayPalWidgetConfig(
    val accessToken: String,
    val gatewayId: String,
    val requestShipping: Boolean = true
)
```

| Name                  | Definition                                                                                      | Type                                          | Mandatory/Optional |
| :-------------------- | :---------------------------------------------------------------------------------------------- | :-------------------------------------------- | :----------------- |
| accessToken           | The OAuth access token required for authenticating API requests to PayPal services. This token ensures secure communication and authorizes operations on behalf of the user or merchant.                                                   | String                | Mandatory           |
| gatewayId             | The unique identifier for the payment gateway used to route transactions. This ID directs payments through the correct processing channels.                                                    | String                          | Mandatory           |
| requestShipping       | A boolean indicating whether shipping information should be requested from the user during the PayPal checkout flow. Setting this to `false` can streamline the checkout process for digital goods or services where shipping is not applicable.                | Boolean (default = `true`)                    | Optional           |

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

### 4. Error/Exceptions Mapping

The following describes PayPal exceptions that can be thrown. All exceptions extend from the base `PayPalException` class.

```Kotlin
sealed class PayPalException(displayableMessage: String) : SdkException(displayableMessage) {
    data class FetchingUrlException(val error: ApiErrorResponse) : PayPalException(error.displayableMessage)
    data class CapturingChargeException(val error: ApiErrorResponse) : PayPalException(error.displayableMessage)
    class WebViewException(val code: Int? = null, displayableMessage: String) : PayPalException(displayableMessage)
    class CancellationException(displayableMessage: String) : PayPalException(displayableMessage)
    class ParseException(displayableMessage: String, val errorBody: String?) : PayPalException(displayableMessage)
    class InitialisationWalletTokenException(displayableMessage: String) : PayPalException(displayableMessage)
    data class GetPayPalClientIdException(val error: ApiErrorResponse) : PayPalException(error.displayableMessage)
    class ConfigurationException(displayableMessage: String) : PayPalException(displayableMessage)
    class UnknownException(displayableMessage: String) : PayPalException(displayableMessage)
}
```

| Exception                        | Description                                                                                   |
| :------------------------------- | :-------------------------------------------------------------------------------------------- |
| FetchingUrlException             | Exception thrown when there is an error fetching the URL for PayPal.                         |
| CapturingChargeException         | Exception thrown when there is an error capturing the charge for PayPal.                     |
| WebViewException                 | Exception thrown when there is an error while communicating with a WebView.                  |
| CancellationException            | Exception thrown when there is a cancellation error related to PayPal.                       |
| ParseException                   | Exception thrown when there is an error parsing data from an API call (typically JSON).      |
| InitialisationWalletTokenException | Exception thrown when there is an error during the initialisation of the PayPal wallet token. |
| GetPayPalClientIdException       | Exception thrown when there is an error retrieving the PayPal Client ID.                     |
| ConfigurationException           | Exception thrown when a required configuration is missing or invalid for the PayPal flow.    |
| UnknownException                 | Exception thrown when there is an unknown error related to PayPal.                           |

### 5. Widget Styling

Defines the visual appearance for specific elements within the `PayPalWidget`.
With the migration to the PayPal web SDK, the widget now supports comprehensive button customization including color, label, and shape options, in addition to the loading indicator styling.

#### Appearance Contract

The `PayPalWidgetAppearance` class encapsulates the configurable style properties for the widget, including both the loader and PayPal button styling options.

```Kotlin
@Immutable
class PayPalWidgetAppearance(
    val loader: LoaderAppearance,
    val buttonColour: PayPalButtonColor,
    val buttonLabel: PayPalButtonLabel,
    val buttonShape: PaymentButtonShape,
) {
    fun copy(
        loader: LoaderAppearance = this.loader,
        paypalColor: PayPalButtonColor = this.buttonColour,
        paypalLabel: PayPalButtonLabel = this.buttonLabel,
        buttonShape: PaymentButtonShape = this.buttonShape,
    ): PayPalWidgetAppearance = PayPalWidgetAppearance(
        loader = loader.copy(),
        buttonColour = paypalColor,
        buttonLabel = paypalLabel,
        buttonShape = buttonShape,
    )
}
```

#### Default Appearance & Customisation

A default appearance is provided by `PayPalAppearanceDefaults`. This configures:
- Loader: Black color with button loader width
- Button color: Gold (PayPal's signature color)
- Button label: PayPal wordmark
- Button shape: Rounded corners

##### Using Default Appearance

```Kotlin
PayPalWidget( 
    ...
    appearance = PayPalAppearanceDefaults.appearance() // Uses the default appearance
)
```

The default appearance is configured as follows:

```Kotlin
object PayPalAppearanceDefaults {
    @Composable
    fun appearance(): PayPalWidgetAppearance = PayPalWidgetAppearance(
        loader = LoaderAppearanceDefaults.appearance()
            .copy(strokeWidth = ButtonAppearanceDefaults.ButtonLoaderWidth, color = Color.Black),
        buttonColour = PayPalButtonColor.GOLD,
        buttonLabel = PayPalButtonLabel.PAYPAL,
        buttonShape = PaymentButtonShape.ROUNDED,
    )
}
```

##### Customising Appearance

You can create a custom `PayPalWidgetAppearance` by modifying any combination of the loader, button color, label, or shape properties.

```Kotlin
@Composable 
fun MyCustomPayPalScreen() { 
    // Create appearance by customizing specific properties
    val customAppearance = PayPalAppearanceDefaults.appearance().copy(
        loader = LoaderAppearanceDefaults.appearance().copy(
            color = Color.Blue,
            strokeWidth = 3.dp
        ),
        paypalColor = PayPalButtonColor.BLUE,
        paypalLabel = PayPalButtonLabel.CHECKOUT,
        buttonShape = PaymentButtonShape.RECTANGLE
    )

    // Alternatively, create entirely from scratch:
    val completelyCustomAppearance = PayPalWidgetAppearance(
        loader = LoaderAppearanceDefaults.appearance().copy(
            color = Color.DarkGray,
            strokeWidth = 2.dp
        ),
        buttonColour = PayPalButtonColor.SILVER,
        buttonLabel = PayPalButtonLabel.PAY,
        buttonShape = PaymentButtonShape.ROUNDED
    )

    PayPalWidget(
        ...
        appearance = customAppearance, // Use your custom appearance
    )
}
```

#### Style Attributes

|  Name                | Description                                                                                              | Type                               | Default Value (from `PayPalAppearanceDefaults`)   |
| ---------------------|----------------------------------------------------------------------------------------------------------|------------------------------------|----------------------------------------------------|
| `loader`             | Defines the appearance of the loading indicator shown when the widget is processing or loading content.  | `LoaderAppearance`                 | Black color with button loader width              |
| `buttonColour`       | Sets the color scheme of the PayPal button.                                                             | `PayPalButtonColor`                | `PayPalButtonColor.GOLD`                           |
| `buttonLabel`        | Controls the text/label displayed on the PayPal button.                                                 | `PayPalButtonLabel`                | `PayPalButtonLabel.PAYPAL`                         |
| `buttonShape`        | Defines the shape/border radius of the PayPal button.                                                   | `PaymentButtonShape`               | `PaymentButtonShape.ROUNDED`                       |

#### PayPal Button Color Options

The `PayPalButtonColor` enum provides the following color options:
- `GOLD` - PayPal's signature gold color (recommended)
- `BLUE` - PayPal blue color scheme
- `SILVER` - Silver/gray color scheme
- `WHITE` - White background with colored text
- `BLACK` - Black background with white text

#### PayPal Button Label Options

The `PayPalButtonLabel` enum provides the following label options:
- `PAYPAL` - Shows only the PayPal wordmark
- `CHECKOUT` - Shows "Checkout with PayPal"
- `PAY` - Shows "Pay with PayPal"
- `BUYNOW` - Shows "Buy Now with PayPal"

#### Payment Button Shape Options

The `PaymentButtonShape` enum provides the following shape options:
- `ROUNDED` - Rounded corners (recommended)
- `RECTANGLE` - Sharp rectangular corners

---

**Note:**
*   The PayPal button styling follows PayPal's official design guidelines and uses the PayPal web SDK for rendering.
*   The `LoaderAppearance` itself would have its own detailed documentation explaining its configurable attributes.
