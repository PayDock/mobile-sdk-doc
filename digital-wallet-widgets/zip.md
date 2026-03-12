# Zip

> 
>
> Enable customers to Buy Now and Pay Later with Zip. 

Use the Zip Widget to integrate with Zip payments. This handles the communication between Zip and the SDK via a WebView checkout flow. Once the transaction is completed, the widget returns the payment source token for further processing.


## iOS

## How to use the ZipWidget

### Overview

Follow this guide to initialize and then use the `ZipWidget` widget in your application. This widget enables you to use Zip in your Payment flow.

For reference: [Zip](https://zip.co/au)

> **Note**:
>
> The Zip checkout flow opens a WebView where the customer completes their payment with Zip. Upon successful completion, the widget returns a payment source token that can be used for further processing.

The code for the `ZipWidget` is as follows. None of the values are populated in this example as this is a definition.

```Swift
ZipWidget(
    viewState: ViewState? = nil,
    config: ZipWidgetConfig,
    appearance: ZipWidgetAppearance = ZipWidgetAppearance(),
    loadingDelegate: WidgetLoadingDelegate? = nil,
    eventDelegate: WidgetEventDelegate? = nil,
    completion: @escaping (Result<String, ZipError>) -> Void
) {...}
```

The following is the `ZipWidget` code populated with example values. This demonstrates how to use the widget in your application: 


```Swift
ZipWidget(
    config: ZipWidgetConfig(
        accessToken: "YOUR-ACCESS-TOKEN",
        gatewayId: "YOUR-GATEWAY-ID",
        amount: 100.0,
        currency: "AUD",
        firstName: "John",
        lastName: "Doe",
        email: "john.doe@example.com",
        phone: "+61400000000",
        items: [
            ZipWidgetConfig.Item(name: "Test Item", amount: "100.00", quantity: 1)
        ]
    ),
    appearance: ZipWidgetAppearance(buttonStyle: .whiteOnBlack)
) { result in
    switch result {
    case .success(let paymentSourceToken):
        // Handle success with the payment source token
        viewModel.handleSuccess(token: paymentSourceToken)
    case .failure(let error):
        // Handle error - access error.customMessage for user-friendly message
        viewModel.handleError(message: error.customMessage)
    }
}
```

### 2. Parameter Definitions

This subsection describes the parameters required by the `ZipWidget` view. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `ZipWidget`.

#### ZipWidget

| Name            | Definition                                                                                         | Type                                        | Mandatory/Optional |
| :-------------- | :------------------------------------------------------------------------------------------------- | :------------------------------------------ | :----------------- |
| viewState       | Controls the enabled/disabled state of the widget.                                                 | `ViewState?`                                | Optional           |
| config          | Configuration for the Zip payment.                                                                 | `ZipWidgetConfig`                           | Mandatory          |
| appearance      | Object for visual customization of the widget.                                                     | `ZipWidgetAppearance`                       | Optional           |
| loadingDelegate | Delegate control of showing loaders to this instance. When set, internal loaders are not shown.    | `WidgetLoadingDelegate?`                    | Optional           |
| eventDelegate   | Delegate for handling widget events such as button clicks.                                         | `WidgetEventDelegate?`                      | Optional           |
| completion      | Result callback with the payment source token if successful, or a `ZipError` if unsuccessful.      | `(Result<String, ZipError>) -> Void`        | Mandatory          |

#### ZipWidgetConfig

| Name         | Definition                                                           | Type           | Mandatory/Optional |
| :----------- | :------------------------------------------------------------------- | :------------- | :----------------- |
| accessToken  | Access token for authenticating with the Paydock API.                | `String`       | Mandatory          |
| gatewayId    | Gateway ID for processing Zip payments.                              | `String`       | Mandatory          |
| amount       | Total transaction amount.                                            | `Decimal`      | Mandatory          |
| currency     | Currency code (e.g., "AUD") for the transaction.                     | `String`       | Mandatory          |
| firstName    | Shopper's first name.                                                | `String?`      | Optional           |
| lastName     | Shopper's last name.                                                 | `String?`      | Optional           |
| email        | Shopper's email address.                                             | `String?`      | Optional           |
| phone        | Shopper's phone number.                                              | `String?`      | Optional           |
| tokenize     | Whether to tokenize the payment method. Default: `true`.             | `Bool?`        | Optional           |
| gender       | Shopper's gender ("male", "female", or other).                       | `String?`      | Optional           |
| dateOfBirth  | Date of birth in "YYYY-MM-DD" format.                                | `String?`      | Optional           |
| shippingType | Shipping type (e.g., "delivery", "pickup").                          | `String?`      | Optional           |
| billing      | Billing address information.                                         | `Address?`     | Optional           |
| shipping     | Shipping address information.                                        | `Address?`     | Optional           |
| items        | List of items in the order.                                          | `[Item]?`      | Optional           |
| statistics   | Customer statistics for fraud prevention.                            | `Statistics?`  | Optional           |

#### ZipWidgetConfig.Item

| Name      | Definition                                  | Type      | Mandatory/Optional |
| :-------- | :------------------------------------------ | :-------- | :----------------- |
| name      | Name of the item.                           | `String`  | Mandatory          |
| amount    | Price of the item.                          | `String`  | Mandatory          |
| quantity  | Quantity of the item.                       | `Int`     | Mandatory          |
| reference | Reference or description for the item.      | `String?` | Optional           |

#### ZipWidgetConfig.Address

| Name      | Definition                                  | Type      | Mandatory/Optional |
| :-------- | :------------------------------------------ | :-------- | :----------------- |
| firstName | First name associated with the address.     | `String?` | Optional           |
| lastName  | Last name associated with the address.      | `String?` | Optional           |
| line1     | First line of the street address.           | `String?` | Optional           |
| line2     | Second line of the street address.          | `String?` | Optional           |
| city      | City name.                                  | `String?` | Optional           |
| state     | State or province.                          | `String?` | Optional           |
| postcode  | Postal or ZIP code.                         | `String?` | Optional           |
| country   | ISO country code (e.g., "AU").              | `String?` | Optional           |

#### ZipWidgetConfig.Statistics

Customer statistics for fraud prevention purposes.

| Name               | Definition                                      | Type      | Mandatory/Optional |
| :----------------- | :---------------------------------------------- | :-------- | :----------------- |
| accountCreated     | Date when the account was created (YYYY-MM-DD). | `String?` | Optional           |
| salesTotalNumber   | Total number of sales.                          | `String?` | Optional           |
| salesTotalAmount   | Total amount of sales.                          | `String?` | Optional           |
| salesAvgValue      | Average value of sales.                         | `String?` | Optional           |
| salesMaxValue      | Maximum value of sales.                         | `String?` | Optional           |
| refundsTotalAmount | Total amount of refunds.                        | `String?` | Optional           |
| previousChargeback | Whether there was a previous chargeback.        | `String?` | Optional           |
| currency           | Currency code.                                  | `String?` | Optional           |
| lastLogin          | Date of last login (YYYY-MM-DD).                | `String?` | Optional           |

### 3. Callback Explanation

#### Completion Callback

The `completion` callback is invoked after the Zip payment operation is completed. It receives a `Result<String, ZipError>`:

- **Success**: Returns the payment source token as a `String` that can be used for further processing (e.g., creating a charge).
- **Failure**: Returns a `ZipError` with details about what went wrong. Use `error.customMessage` to get a user-friendly error message.

### 4. Error Handling

The following describes `ZipError` cases that can be returned:

| Error Case               | Description                                                      | Associated Data                |
| :----------------------- | :--------------------------------------------------------------- | :----------------------------- |
| errorFetchingZipUrl      | Error thrown when fetching the Zip checkout URL.                 | `ErrorRes`                     |
| errorCapturingCharge     | Error thrown when completing the charge has failed.              | `ErrorRes`                     |
| invalidCheckoutUrl       | Received an invalid or unsupported URL.                          | None                           |
| webViewFailed            | WebView loading failure.                                         | `NSError`                      |
| transactionCanceled      | User canceled the Zip transaction.                               | `checkoutId: String?`          |
| transactionDeclined      | Zip declined the transaction.                                    | `checkoutId: String?`          |
| transactionReferred      | Transaction requires manual review (referred).                   | `checkoutId: String?`          |
| unexpectedStatus         | Unexpected status received from Zip.                             | `status: String?, checkoutId: String?` |
| initialisingWalletToken  | Error initializing the wallet token.                             | `reason: String`               |
| unknownError             | Unknown or unspecified error.                                    | `RequestError?`                |

Each `ZipError` provides a `customMessage` property that returns a user-friendly error message suitable for display.

### 5. Widget Styling

Defines the visual appearance for the `ZipWidget`. The widget displays a Zip-branded button following Zip's official brand guidelines.

#### Appearance Contract

The `ZipWidgetAppearance` struct encapsulates the configurable style properties for the widget.

```Swift
public struct ZipWidgetAppearance {
    public var buttonStyle: ZipButtonStyle
    public var loader: Theme.ButtonLoader
    public let cornerRadius: CGFloat  // Fixed at 8.0 per brand guidelines
}
```

#### Default Appearance & Customisation

A default appearance is provided using `.whiteOnBlack` button style. The corner radius is fixed at 8.0 per Zip brand guidelines.

##### Using Default Appearance

```Swift
ZipWidget( 
    config: config,
    appearance: ZipWidgetAppearance()  // Uses the default appearance
) { result in
    // Handle result
}
```

##### Customising Appearance

You can create a custom `ZipWidgetAppearance` by specifying a different button style and loader configuration.

```Swift
struct MyCustomZipScreen: View { 
    private func myCustomAppearance() -> ZipWidgetAppearance {
        let loader = Theme.ButtonLoader(spinnerColor: .white)
        let appearance = ZipWidgetAppearance(
            buttonStyle: .blackOnWhite,
            loader: loader
        )
        return appearance
    }
    
    var body: some View {
        ZipWidget( 
            config: config,
            appearance: myCustomAppearance()
        ) { result in
            // Handle result
        }
    }
}
```

#### Style Attributes

The following attributes can be configured within `ZipWidgetAppearance`:

| Name          | Description                                                                                           | Type                  | Default Value                  |
|---------------|-------------------------------------------------------------------------------------------------------|-----------------------|--------------------------------|
| `buttonStyle` | The button style following Zip's brand guidelines.                                                    | `ZipButtonStyle`      | `.whiteOnBlack`                |
| `loader`      | Defines the appearance of the loading indicator shown when the widget is processing.                  | `Theme.ButtonLoader`  | Auto-adapted to button style   |
| `cornerRadius`| The corner radius of the button. Fixed per Zip brand guidelines and cannot be customized.             | `CGFloat`             | `8.0`                          |

#### ZipButtonStyle Options

| Style           | Description                                        | Background Color      | Border           |
|-----------------|----------------------------------------------------|-----------------------|------------------|
| `.whiteOnBlack` | White Zip logo on dark purple background (default) | `#1A0826`             | None             |
| `.blackOnWhite` | Colored Zip logo on off-white background           | `#FFFFFA`             | 1px black border |

---

**Note:**
* The button styles follow Zip's official brand guidelines. Refer to the [Zip website](https://zip.co/au) for branding guidelines and requirements.
* The `cornerRadius` is fixed at 8.0 and cannot be changed to maintain brand compliance.

### 6. WidgetLoadingDelegate

This `loadingDelegate` allows the calling app to take control of the internal widget loading states. When set, internal loaders will not be shown. 
It defines methods to handle the start and finish of a loading process.

```Swift
protocol WidgetLoadingDelegate {
    // Called when a widget's loading process starts.
    func loadingDidStart()

    // Called when a widget's loading process finishes.
    func loadingDidFinish()
}
```

### 7. WidgetEventDelegate

This `eventDelegate` allows the calling app to receive notifications of user interactions within the widget, such as button clicks. This is useful for analytics and tracking purposes.

```Swift
protocol WidgetEventDelegate {
    /**
     * Called when a widget event occurs.
     *
     * @param event The event that occurred, containing the event type and properties.
     */
    func widgetEvent(event: WidgetEvent)
}
```

##### Zip Events

The Zip Widget triggers the following events:

**Zip Button Click Event** - Triggered when the Zip payment button is clicked:

```json
{
  "type": "Button",
  "properties": {
    "name": "ZipCheckoutButton",
    "action": "click"
  }
}
```

## Android

## How to use the ZipWidget

### 1. Overview

This section provides a step-by-step guide on how to initialise and use the `ZipWidget` composable in your Android application. The widget opens a Zip WebView checkout flow and, on success, returns a payment source token that can be used with other Paydock APIs (for example, to create a charge).

For reference: [Zip](https://zip.co/au)

> **Note**:
>
> The Zip checkout flow opens an in-app WebView activity where the customer completes their payment with Zip. When checkout is completed, the widget exchanges the Zip checkout token for a Paydock payment source token and returns it via the `completion` callback.

### 2. Widget definition

The following snippet shows the definition of the `ZipWidget` composable in Android:

```Kotlin
@Composable
fun ZipWidget(
    modifier: Modifier = Modifier,
    enabled: Boolean = true,
    config: ZipWidgetConfig,
    appearance: ZipWidgetAppearance = ZipWidgetAppearanceDefaults.appearance(),
    loadingDelegate: WidgetLoadingDelegate? = null,
    eventDelegate: WidgetEventDelegate? = null,
    completion: (Result<ZipResult>) -> Unit
)
```

### 3. Example usage

The example below demonstrates how to configure and use the `ZipWidget` in your payment flow:

```Kotlin
@Composable
fun ZipCheckoutScreen(
    accessToken: String,
    gatewayId: String,
    onPaymentSourceTokenReceived: (String) -> Unit,
    onError: (Throwable) -> Unit
) {
    val config = ZipWidgetConfig(
        accessToken = accessToken,
        gatewayId = gatewayId,
        amount = BigDecimal("100.00"),
        currency = "AUD",
        firstName = "John",
        lastName = "Doe",
        email = "john.doe@example.com"
    )

    ZipWidget(
        modifier = Modifier
            .fillMaxWidth()
            .height(ButtonAppearanceDefaults.ButtonHeight),
        config = config,
        appearance = ZipWidgetAppearanceDefaults.appearance(),
        loadingDelegate = object : WidgetLoadingDelegate {
            override fun widgetLoadingDidStart() {
                // Show loading indicator
            }

            override fun widgetLoadingDidFinish() {
                // Hide loading indicator
            }
        },
        eventDelegate = object : WidgetEventDelegate {
            override fun widgetEvent(event: Event) {
                // Handle analytics or logging for widget events
            }
        }
    ) { result ->
        result.onSuccess { zipResult ->
            onPaymentSourceTokenReceived(zipResult.token)
        }.onFailure { throwable ->
            onError(throwable)
        }
    }
}
```

### 4. Parameter definitions

This subsection describes the parameters required by the `ZipWidget` composable and its configuration models. It explains the purpose of each parameter and its significance in configuring the widget's behaviour.

#### ZipWidget

| Name            | Definition                                                                                                  | Type                          | Mandatory/Optional |
| :-------------- | :---------------------------------------------------------------------------------------------------------- | :---------------------------- | :----------------- |
| modifier        | Compose modifier for sizing, padding, and other layout adjustments.                                        | `Modifier`                    | Optional           |
| enabled         | Controls the enabled state of this widget. When `false`, the button is disabled and non-interactive.       | `Boolean`                     | Optional           |
| config          | Configuration for the Zip payment session.                                                                 | `ZipWidgetConfig`             | Mandatory          |
| appearance      | Customisation options for the visual appearance of the widget.                                             | `ZipWidgetAppearance`         | Optional           |
| loadingDelegate | Delegate that allows the host app to control loading indicators. When set, internal loaders are not shown. | `WidgetLoadingDelegate?`      | Optional           |
| eventDelegate   | Delegate for handling widget events, such as the Zip checkout button click.                                | `WidgetEventDelegate?`        | Optional           |
| completion      | Result callback invoked when the Zip payment flow finishes, with a `ZipResult` or an error.                | `(Result<ZipResult>) -> Unit` | Mandatory          |

#### ZipWidgetConfig

The `ZipWidgetConfig` data class defines the configuration required to initialise a Zip payment session.

| Name          | Definition                                                          | Type                          | Mandatory/Optional |
| :------------ | :------------------------------------------------------------------ | :---------------------------- | :----------------- |
| accessToken   | Access token used for authenticating with the Paydock API.         | `String`                      | Mandatory          |
| gatewayId     | Gateway ID for processing Zip payments.                            | `String`                      | Mandatory          |
| amount        | Total transaction amount.                                          | `BigDecimal`                  | Mandatory          |
| currency      | Currency code (for example, `"AUD"`) for the transaction.         | `String`                      | Mandatory          |
| firstName     | Shopper's first name.                                              | `String`                      | Mandatory          |
| lastName      | Shopper's last name.                                               | `String`                      | Mandatory          |
| email         | Shopper's email address.                                           | `String`                      | Mandatory          |
| phone         | Shopper's phone number.                                            | `String?`                     | Optional           |
| tokenize      | Whether to tokenize the payment method. Default value is `true`.   | `Boolean`                     | Optional           |
| gender        | Shopper's gender (for example, `"male"`, `"female"`, or other).   | `String?`                     | Optional           |
| dateOfBirth   | Date of birth in `YYYY-MM-DD` format.                              | `String?`                     | Optional           |
| shippingType  | Shipping type (for example, `"delivery"`, `"pickup"`).             | `String?`                     | Optional           |
| billing       | Billing address information.                                       | `ZipWidgetConfig.Address?`    | Optional           |
| shipping      | Shipping address information.                                      | `ZipWidgetConfig.Address?`    | Optional           |
| items         | List of items in the order.                                        | `List<ZipWidgetConfig.Item>?` | Optional           |
| statistics    | Customer statistics for fraud prevention.                          | `ZipWidgetConfig.Statistics?` | Optional           |

#### ZipWidgetConfig.Address

| Name      | Definition                                 | Type     | Mandatory/Optional |
| :-------- | :----------------------------------------- | :------- | :----------------- |
| firstName | First name associated with the address.    | `String?`| Optional           |
| lastName  | Last name associated with the address.     | `String?`| Optional           |
| line1     | First line of the street address.          | `String?`| Optional           |
| line2     | Second line of the street address.         | `String?`| Optional           |
| city      | City name.                                 | `String?`| Optional           |
| state     | State or province.                         | `String?`| Optional           |
| postcode  | Postal or ZIP code.                        | `String?`| Optional           |
| country   | ISO country code (for example, `"AU"`).    | `String?`| Optional           |

#### ZipWidgetConfig.Item

| Name      | Definition                             | Type     | Mandatory/Optional |
| :-------- | :------------------------------------- | :------- | :----------------- |
| name      | Name of the item.                      | `String` | Mandatory          |
| amount    | Price of the item as a string value.   | `String` | Mandatory          |
| quantity  | Quantity of the item.                  | `Int`    | Mandatory          |
| reference | Reference or description for the item. | `String?`| Optional           |

#### ZipWidgetConfig.Statistics

Customer statistics for fraud prevention purposes.

| Name               | Definition                                       | Type     | Mandatory/Optional |
| :----------------- | :----------------------------------------------- | :------- | :----------------- |
| accountCreated     | Date when the account was created (`YYYY-MM-DD`).| `String?`| Optional           |
| salesTotalNumber   | Total number of sales.                           | `String?`| Optional           |
| salesTotalAmount   | Total amount of sales.                           | `String?`| Optional           |
| salesAvgValue      | Average value of sales.                          | `String?`| Optional           |
| salesMaxValue      | Maximum value of sales.                          | `String?`| Optional           |
| refundsTotalAmount | Total amount of refunds.                         | `String?`| Optional           |
| previousChargeback | Whether there was a previous chargeback.         | `String?`| Optional           |
| currency           | Currency code.                                   | `String?`| Optional           |
| lastLogin          | Date of last login (`YYYY-MM-DD`).               | `String?`| Optional           |

### 5. Callback explanation

#### Completion callback

The `completion` callback is invoked after the Zip payment operation is completed. It receives a `Result<ZipResult>`:

- **Success**: Returns a `ZipResult` whose `token` property is the payment source token that can be used for further processing (for example, creating a charge).
- **Failure**: Returns an exception (typically a `ZipException` subtype) describing what went wrong. You can use its `displayableMessage` (from `SdkException`) to show a user-friendly error message.

### 6. Error/Exceptions mapping

The following describes Zip exceptions that can be thrown during the Android Zip flow. All of them extend `ZipException`, which itself extends `SdkException`:

```Kotlin
sealed class ZipException(displayableMessage: String) : SdkException(displayableMessage) {
    data class FetchingCheckoutUrlException(val error: ApiErrorResponse) : ZipException(error.displayableMessage)
    data class CreatingPaymentSourceTokenException(val error: ApiErrorResponse) : ZipException(error.displayableMessage)
    class WebViewException(val code: Int? = null, displayableMessage: String) : ZipException(displayableMessage)
    class CancellationException(val checkoutId: String? = null, displayableMessage: String = MobileSDKConstants.ZipConfig.Errors.CANCELLATION_ERROR) : ZipException(displayableMessage)
    class DeclinedException(val checkoutId: String? = null, displayableMessage: String = MobileSDKConstants.ZipConfig.Errors.DECLINED_ERROR) : ZipException(displayableMessage)
    class ReferredException(val checkoutId: String? = null, displayableMessage: String = MobileSDKConstants.ZipConfig.Errors.REFERRED_ERROR) : ZipException(displayableMessage)
    class UnexpectedStatusException(val status: String?, val checkoutId: String? = null, displayableMessage: String = \"Unexpected Zip status: \${status ?: \"unknown\"}\") : ZipException(displayableMessage)
    class ParseException(displayableMessage: String, val errorBody: String? = null) : ZipException(displayableMessage)
    class UnknownException(displayableMessage: String = MobileSDKConstants.ZipConfig.Errors.UNKNOWN_ERROR) : ZipException(displayableMessage)
}
```

| Exception                             | Description                                                                                               | Key properties              |
| :------------------------------------ | :-------------------------------------------------------------------------------------------------------- | :-------------------------- |
| `FetchingCheckoutUrlException`        | Thrown when there is an error fetching the Zip checkout URL.                                             | `error: ApiErrorResponse`   |
| `CreatingPaymentSourceTokenException` | Thrown when there is an error creating the Zip payment source token.                                     | `error: ApiErrorResponse`   |
| `WebViewException`                    | Thrown when there is an error in the WebView (for example, network or HTTP error) during Zip checkout.  | `code: Int?`                |
| `CancellationException`              | Thrown when the Zip transaction was cancelled (for example, the user closed the WebView or sheet).      | `checkoutId: String?`       |
| `DeclinedException`                  | Thrown when Zip declines the transaction.                                                                | `checkoutId: String?`       |
| `ReferredException`                  | Thrown when the transaction requires manual review (referred).                                           | `checkoutId: String?`       |
| `UnexpectedStatusException`          | Thrown when an unexpected status is returned from Zip.                                                   | `status: String?`, `checkoutId: String?` |
| `ParseException`                     | Thrown when there is an error parsing a Zip-related API response.                                        | `errorBody: String?`        |
| `UnknownException`                   | Thrown when an unknown error occurs that does not fit into other categories.                             | —                            |

> **Invalid checkout URL**:
>
> The `ZipWidget` validates the checkout URL before launching the WebView. If the URL is null, blank, or does not start with `http://` or `https://`, the widget reports a WebView error using `MobileSDKConstants.ZipConfig.Errors.INVALID_URL`. Handle this case in your `completion` callback like any other failure.

### 7. Widget styling

Defines the visual appearance for the Android `ZipWidget`. The widget displays a Zip-branded button following Zip's official brand guidelines.

#### Appearance contract

The `ZipWidgetAppearance` class encapsulates the configurable style properties for the Android widget:

```Kotlin
@Immutable
class ZipWidgetAppearance(
    val buttonStyle: ZipButtonStyle,
    val loader: LoaderAppearance
)
```

#### Default appearance and customisation

A default appearance is provided by `ZipWidgetAppearanceDefaults.appearance()`, which uses the `WHITE_ON_BLACK` Zip button style and configures the loader colour to match.

##### Using the default appearance

```Kotlin
ZipWidget(
    config = config,
    appearance = ZipWidgetAppearanceDefaults.appearance()
) { result ->
    // Handle result
}
```

##### Customising appearance

You can create a custom `ZipWidgetAppearance` by changing the `ZipButtonStyle` or by providing a custom `LoaderAppearance`:

```Kotlin
@Composable
fun MyCustomZipScreen(config: ZipWidgetConfig) {
    val customAppearance = ZipWidgetAppearanceDefaults.appearance().copy(
        buttonStyle = ZipButtonStyle.BLACK_ON_WHITE,
        loader = LoaderAppearanceDefaults.appearance().copy(
            color = ZipButtonStyle.BLACK_ON_WHITE.loaderColor
        )
    )

    ZipWidget(
        config = config,
        appearance = customAppearance
    ) { result ->
        // Handle result
    }
}
```

#### Style attributes

The following attributes can be configured within `ZipWidgetAppearance`:

| Name          | Description                                                                                   | Type               | Default value                                    |
| :------------ | :-------------------------------------------------------------------------------------------- | :----------------- | :----------------------------------------------- |
| `buttonStyle` | The button style following Zip's brand guidelines.                                            | `ZipButtonStyle`   | `ZipButtonStyle.WHITE_ON_BLACK`                  |
| `loader`      | Defines the appearance of the loading indicator shown when the widget is processing/loading. | `LoaderAppearance` | `LoaderAppearanceDefaults.appearance()` with `color` set to match `buttonStyle.loaderColor` |

#### ZipButtonStyle options

`ZipButtonStyle` defines the supported branded button styles:

| Style                   | Description                                          | Background colour | Border                |
| :---------------------- | :--------------------------------------------------- | :---------------- | :-------------------- |
| `WHITE_ON_BLACK`        | White Zip logo on a dark purple/black background.   | `#1A0826`         | None                  |
| `BLACK_ON_WHITE`        | Coloured Zip logo on an off-white background.       | `#FFFFFA`         | 1 dp black border     |

The loader colour automatically adapts to the selected style (`loaderColor`).

### 8. WidgetLoadingDelegate

This `loadingDelegate` allows the calling app to take control of the internal widget loading states. When set, internal loaders are not shown.

```Kotlin
interface WidgetLoadingDelegate {
    // Called when a widget's loading process starts.
    fun widgetLoadingDidStart()

    // Called when a widget's loading process finishes.
    fun widgetLoadingDidFinish()
}
```

You can use this delegate in combination with the `enabled` flag to synchronise external loading indicators with the widget's flow.

### 9. WidgetEventDelegate

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

#### Zip events

The Android Zip Widget triggers the following event:

**Zip Checkout Button Click Event** — Triggered when the Zip checkout button is clicked:

```json
{
  "type": "Button",
  "properties": {
    "name": "ZipCheckoutButton",
    "action": "click"
  }
}
```

| Property              | Description                                        | Type    | Optional/Required |
| :-------------------- | :------------------------------------------------- | :------ | :---------------- |
| `type`                | The type of UI element that triggered the event.  | String  | Required          |
| `properties.name`     | The name identifier of the specific element.      | String  | Required          |
| `properties.action`   | The action performed on the element.              | String  | Required          |

