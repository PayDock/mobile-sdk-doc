# PayPal Vault Widget

The MobileSDK provides a PayPal Vault Widget that integrates with the PayPal SDK. This handles the communication between PayPal and the SDK to authenticate and manage PayPal Vault. Once completed, the component returns the OTT (One Time Token) result.

> **Note**:
>
> Vaulting a PayPal account will allow you to charge the account in the future without requiring your customer to be present during the transaction or re-authenticate with PayPal when they are present during the transaction.
> 
> Vaulting creates a PayPal pre-approved payment between you and the customer, displayed in the customer's account profile on PayPal.com.

## iOS

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `PayPalSavePaymentSourceWidget` view in your application. The widget facilitates the card linking process using PayPal services.

> **Note**:
>
> When the customer taps the link button for the PayPal Vault flow, the SDK handles the gateway communication between Paydock and PayPal before initialising the PayPal client web flow.

The following sample code demonstrates the definition of the `PayPalSavePaymentSourceWidget`:

```Swift
    PayPalSavePaymentSourceWidget(
        viewState: ViewState?,
        config: PayPalVaultConfig,
        loadingDelegate: WidgetLoadingDelegate?,
        appearance: PayPalVaultAppearance = PayPalVaultAppearance(),
        completion: @escaping (Result<PayPalVaultResult, PayPalVaultError>) -> Void)
    {...}
```

The following sample code example demonstrates the usage within your application:

```Swift
let config = PayPalVaultConfig(accessToken: "your_access_token", gatewayId: "your_gateway_id", actionText: nil)
PayPalSavePaymentSourceWidget(
    viewState: ViewState(state: .disabled),
    config: config,
    loadingDelegate = DELEGATE_INSTANCE, // Delegate class to handle loading) 
    appearance: PayPalVaultAppearance()) { result in
    switch result {
    case let .success(payPalVaultResult):
        // Handle success
    case let .failure(error):
        // Handle failure
    }
}
```

### 2. Parameter Definitions

This subsection describes the various parameters required by the `PayPalSavePaymentSourceWidget` view. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `PayPalSavePaymentSourceWidget`.

#### PayPalSavePaymentSourceWidget

| Name                  | Definition                                                                                        | Type                                                   | Mandatory/Optional |
| :-------------------- | :------------------------------------------------------------------------------------------------ | :----------------------------------------------------- | :----------------- |
| viewState             |  View options that are two way fields to alter view state                                         | ViewState                                              | Optional           |
| config                |  The configuration for PayPal Vault widget.                                                       | `PayPalVaultConfig`                                    | Mandatory          |
| loadingDelegate       |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown.  | `WidgetLoadingDelegate`                                | Optional           |
| appearance            |  Object for visual customization of the widget.                                                   | `MobileSDK.PayPalVaultAppearance`                      | Optional           |
| completion            |  Result callback when the PayPal Vault linking process completes either successful, or error.     | `Result<PayPalVaultResult, PayPalVaultError> -> Void`  | Mandatory          |

#### PayPalVaultConfig

This subsection describes the various parameters required by the `PayPalVaultConfig`.

The following sample code demonstrates the response structure:

```Swift
public struct PayPalVaultConfig {
    public let accessToken: String
    public let gatewayId: String
    public let actionText: String?
}
```

| Name           | Definition                                                                       | Type                       | Mandatory/Optional |
| :------------- | :------------------------------------------------------------------------------- | :------------------------- | :----------------- |
| accessToken    |  The OAuth access token required for authenticating API requests.                | String                     | Mandatory          |
| gatewayId      |  The PayPal gateway ID used to identify the payment gateway.                     | String                     | Mandatory          |
| actionText     |  The text to be displayed on the button. Defaults to "Link PayPal account".      | String                     | Optional           |
| icon           |  Type of the icon you want to display in the PayPal Vault button.                | Icon                       | Optional           |

#### PayPalVaultConfig.Icon

Enum defining various icon types for the PayPalVault button

```Swift
public enum Icon {
    case none
    case defaultIcon
    case customIcon(image: Image)
}
```

| Name              | Definition                                               | Type           | Mandatory/Optional |
| :---------------- | :------------------------------------------------------- | :------------- | :----------------- |
| none              |  Vault button will not display any icon.                 | Enum           | Optional           |
| defaultIcon       |  Vault button will display the default Link icon         | Enum           | Optional           |
| customIcon(Image) |  Vault button will display the Image() that you provide  | Enum           | Optional           |

#### PayPalVaultResult

This subsection outlines the structure of the result or response object returned by the `PayPalSavePaymentSourceWidget` view. It details the format and components of the object, enabling you to handle the response effectively within your application.

The following sample code demonstrates the response structure:

```Swift
public struct PayPalVaultResult {
    public let token: String
    public let email: String

}
```

| Name           | Definition                                                   | Type                       | 
| :------------- | :----------------------------------------------------------- | :------------------------- | 
| token          |  The token generated from the PayPal Vault process.          | String                     | 
| email          |  The email associated with the user.                         | String                     | 

### 3. Callback Explanation

#### Completion Callback

The `completion` callback is invoked after the payment operation is completed. It receives a `Result<PayPalVaultResult, PayPalVaultError> -> Void` if the linking process is successful. The callback is used to handle the outcome of the PayPal Vault operation.

#### 4. Error/Exceptions Mapping

The following describes PayPal Vault exceptions that can be thrown. 

```Swift
public enum PayPalVaultError: Error {
    case createSetupToken(error: ErrorRes)
    case getPayPalClientId(error: ErrorRes)
    case createPaymentToken(error: ErrorRes)
    case sdkException(description: String)
    case userCancelled
    case unknownError(RequestError?)
}
```

> **Note**:
>
> sdkException: result of vaulting a PayPal payment method completes with an error.
> userCancelled: result of when a user cancels PayPal payment method vaulting.

| Exception                | Description                                                                        | Error Model            |
| :----------------------- | :--------------------------------------------------------------------------------- | :--------------------- |
| createSetupToken         |  Exception thrown when there is an error creating a setup token.                   |  PayPalVaultError      |
| getPayPalClientId        |  Exception thrown when there is an error retrieving the PayPal client ID.          |  PayPalVaultError      |
| createPaymentToken       |  Exception thrown when there is an error creating a payment token.                 |  PayPalVaultError      |
| sdkException             |  Exception thrown during PayPal SDK operations.                                    |  PayPalVaultError      |
| userCancelled            |  Exception thrown when user manually cancels the flow.                             |  PayPalVaultError      |
| unknownError             |  Exception thrown during the PayPal SDK  process.                                  |  PayPalVaultError      |

### 5. Widget Styling

Defines the visual appearance for specific elements within the `PayPalSavePaymentSourceWidget`.
This appearance configuration mainly focuses on the button customization used for initiating the PayPal Vault flow.

#### Appearance Contract

The `PayPalVaultAppearance` class encapsulates the configurable style properties for the widget, currently centered on the button.

```Swift
public struct PayPalVaultAppearance {
    public var actionButton: Theme.ButtonAppearance
}
```

#### Default Appearance & Customisation

A default appearance is provided by `GlobalTheme`. This configures a specific button appearance intended to be visually compatible with the resot of the SDK buttons.

##### Using Default Appearance

```Swift
    PayPalSavePaymentSourceWidget( 
        ...
        appearance: PayPalVaultAppearance = PayPalVaultAppearance(),
    )
```

##### Customising Appearance

You can create a custom `PayPalVaultAppearance` by providing a specific `Theme.ButtonAppearance` configuration if the default doesn't meet your needs or if you want to ensure consistency with other buttons in your app.

```Swift
struct MyCustomPayPalCaultScreen: View { 
    private func myCustomAppearance() -> PayPalVaultAppearance {
        let buttonColors: Theme.ButtonColors = Theme.ButtonColors(background: .black, text: .white)
        let buttonDimensions: Theme.ButtonDimensions = Theme.ButtonDimensions(cornerRadius: 10)
        let button = Theme.ButtonAppearance(colors: buttonColors, dimensions: buttonDimensions)
        let appearance = PayPalVaultAppearance(button: button)
        return appearance
    }
    
    var body: some View {
        PayPalSavePaymentSourceWidget( 
            ...
            appearance: PayPalVaultAppearance = myCustomAppearance()
            ...
        )
    }
}
```

#### Style Attributes

|  Name                | Description                                                                                              | Type                               | Default Value              |
| ---------------------|----------------------------------------------------------------------------------------------------------|------------------------------------|----------------------------|
| `actionButton`       | Defines the appearance of the action button used for initialization of the vault flow.                   | `MobileSDK.Theme.ButtonAppearance` | `GlobalTheme.actionButton` |

---

**Note:**
*   The `actionButton` relies on the `ButtonAppearance` system. Refer to your internal documentation or implementation of `ButtonAppearance` for detailed customization options (e.g., colors, fonts, dimensions...).
*   The `PayPalVaultConfig` allows for optional `actionText` and `icon` parameters which directly affect the button's content, complementing the styling provided by `actionButton`.

## Android

## How to use the PayPalSavePaymentSourceWidget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `PayPalSavePaymentSourceWidget` composable in your application. The widget facilitates the card linking process using PayPal services.

> **Note**:
>
> When the customer taps the link button for the PayPal Vault flow, the SDK handles the gateway communication between Paydock and PayPal before initialising the PayPal client web flow.

The following sample code demonstrates the definition of the `PayPalSavePaymentSourceWidget`:

```Kotlin
@Composable
fun PayPalSavePaymentSourceWidget(
    modifier: Modifier = Modifier,
    enabled: Boolean = true,
    config: PayPalVaultConfig,
    appearance: PayPalPaymentSourceWidgetAppearance = PayPalPaymentSourceAppearanceDefaults.appearance(),
    loadingDelegate: WidgetLoadingDelegate? = null,
    eventDelegate: WidgetEventDelegate? = null,
    completion: (Result<PayPalVaultResult>) -> Unit,
) {...}
```

The following sample code example demonstrates the usage within your application:

```Kotlin
// Initialize the PayPalSavePaymentSourceWidget
PayPalSavePaymentSourceWidget(
        modifier = Modifier.padding(16.dp),
        enabled: Boolean, // optional
        config = PayPalVaultConfig(
            accessToken = [access_token],
            gatewayId = [pay_pal_gateway_id],
            actionText = "custom button text", // optional
            icon = ButtonIcon.Vector(Icons.Filled.Add) || ButtonIcon.DrawableRes(R.drawable.ic_add)// optional
        ),
        appearance = currentOrDefaultAppearance, // optional
        loadingDelegate = DELEGATE_INSTANCE, // optional - Delegate class to handle loading
        eventDelegate = EVENT_DELEGATE_INSTANCE, // optional - Delegate class to handle events
) { result ->
    // Handle the result of the operation
    result.onSuccess { token ->
        // Handle success - Update UI or perform actions
        Log.d("PayPalSavePaymentSourceWidget", "PayPal vault flow was successful.")
    }.onFailure { exception ->
        // Handle failure - Show error message or take appropriate action
        Log.e("PayPalSavePaymentSourceWidget", "PayPal vault flow failed. Error: ${exception.message}")
    }
}
```

### 2. Parameter Definitions

This subsection describes the various parameters required by the `PayPalSavePaymentSourceWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `PayPalSavePaymentSourceWidget`.

#### PayPalSavePaymentSourceWidget

| Name                  | Definition                                                                                        | Type                                              | Mandatory/Optional |
| :-------------------- | :------------------------------------------------------------------------------------------------ | :------------------------------------------------ | :----------------- |
| modifier              |  Compose modifier for container modifications                                                     | `androidx.compose.ui.Modifier`                    | Optional           |
| enabled               |  Controls the enabled state of this Widget.                                                       | Boolean                                           | Optional           |
| config                |  The configuration for PayPal Vault widget.                                                       | `PayPalVaultConfig`                               | Mandatory          |
| appearance            |  Customization options for the visual appearance of the widget                                    | `PayPalPaymentSourceWidgetAppearance`             | Optional           |
| loadingDelegate       |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown.  | `WidgetLoadingDelegate`                           | Optional           |
| eventDelegate         |  Delegate for handling widget events such as button clicks.                                       | `WidgetEventDelegate`                              | Optional           |
| completion            |  Result callback when the PayPal Vault linking process completes either successful, or error.     | `(Result<PayPalVaultResult>) -> Unit`             | Mandatory          |

#### PayPalVaultConfig

This subsection describes the various parameters required by the `PayPalVaultConfig`.

The following sample code demonstrates the response structure:

```Kotlin
data class PayPalVaultConfig(
    val accessToken: String,
    val gatewayId: String,
    val actionText: String? = null,
    val icon: ButtonIcon? = ButtonIcon.DrawableRes(R.drawable.ic_link)
)
```

| Name           | Definition                                                                       | Type                       | Mandatory/Optional |
| :------------- | :------------------------------------------------------------------------------- | :------------------------- | :----------------- |
| accessToken    |  The OAuth access token required for authenticating API requests.                | String                     | Mandatory          |
| gatewayId      |  The PayPal gateway ID used to identify the payment gateway.                     | String                     | Mandatory          |
| actionText     |  The text to be displayed on the button. Defaults to "Link PayPal account".      | String                     | Optional           |
| icon           |  The icon to display on the button. Allows for `ImageVector` or `DrawableRes`.   | `ButtonIcon`               | Optional           |


#### PayPalVaultResult

This subsection outlines the structure of the result or response object returned by the `PayPalSavePaymentSourceWidget` composable. It details the format and components of the object, enabling you to handle the response effectively within your application.

The following sample code demonstrates the response structure:

```Kotlin
data class PayPalVaultResult(
    val token: String
)
```

| Name           | Definition                                                                       | Type                       | 
| :------------- | :------------------------------------------------------------------------------- | :------------------------- | 
| token          |  The token generated from the PayPal Vault process.                              | String                     | 

### 3. Callback Explanation

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

##### PayPal Vault Events

The PayPal Vault Widget triggers the following events:

**PayPal Start Vault Event** - Triggered when the PayPal vault button is clicked:

```json
{
  "type": "Button",
  "properties": {
    "name": "PayPalVaultButton",
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

The `completion` callback is invoked after the payment operation is completed. It receives a `Result<PayPalVaultResult>` if the linking process is successful. The callback is used to handle the outcome of the PayPal Vault operation.

#### 4. Error/Exceptions Mapping

The following describes PayPal Vault exceptions that can be thrown. 

```Kotlin
CreateSetupTokenException(error: ApiErrorResponse) : PayPalVaultException(error.displayableMessage)
GetPayPalClientIdException(error: ApiErrorResponse) : PayPalVaultException(error.displayableMessage)
CreatePaymentTokenException(error: ApiErrorResponse) : PayPalVaultException(error.displayableMessage)
PayPalSDKException(code: Int, description: String) : PayPalVaultException(description)
CancellationException(displayableMessage: String) : PayPalVaultException(displayableMessage)
UnknownException(displayableMessage: String) : PayPalVaultException(displayableMessage)
```

> **Note**:
>
> PayPalSDKException: result of vaulting a PayPal payment method completes with an error.

| Exception                         | Description                                                                                   | Error Model            |
| :-------------------------------- | :-------------------------------------------------------------------------------------------- | :--------------------- |
| CreateSetupTokenException         |  Exception thrown when there is an error creating a setup token.                              |  PayPalVaultError      |
| GetPayPalClientIdException        |  Exception thrown when there is an error retrieving the PayPal client ID.                     |  PayPalVaultError      |
| CreatePaymentTokenException       |  Exception thrown when there is an error creating a payment token.                            |  PayPalVaultError      |
| PayPalSDKException                |  Exception thrown during PayPal SDK operations.                                               |  PayPalVaultError      |
| CancellationException             |  Exception thrown when cancelling PayPal vault flow.                                          |  PayPalVaultError      |
| UnknownException                  |  Exception thrown when an unknown error occurs in PayPal Vault operations.                    |  PayPalVaultError      |

### 4. Widget Styling

Defines the visual appearance for the `PayPalSavePaymentSourceWidget`. This primarily involves customizing the action button used to initiate the PayPal linking flow.

#### Appearance Contract

The `PayPalPaymentSourceWidgetAppearance` class encapsulates the configurable style properties for the widget's action button.

```Kotlin
@Immutable
class PayPalPaymentSourceWidgetAppearance(
    val actionButton: ButtonAppearance
)
```

#### Default Appearance & Customisation

A default appearance is provided by `PayPalPaymentSourceAppearanceDefaults`. This uses a standard outline button style provided by `ButtonAppearanceDefaults.outlineButtonAppearance()`.

##### Using Default Appearance


```Kotlin
    PayPalSavePaymentSourceWidget( 
        ...
        appearance = PayPalPaymentSourceAppearanceDefaults.appearance() // Uses the default appearance
    )
```

##### Customising Appearance

You can create a custom `PayPalPaymentSourceWidgetAppearance` by providing your own `ButtonAppearance` instance for the `actionButton`. This allows you to use different button styles (e.g., filled, text, icon) and customize their specific properties like colors, shapes, typography, etc., as supported by your `ButtonAppearance` implementation.

```Kotlin
@Composable 
fun MyCustomAfterpayScreen() { 
    // Create appearance by using provided defaults, with custom changes
    val customAppearanceFromDefaults = PayPalPaymentSourceAppearanceDefaults.appearance().copy(
        actionButton = ButtonAppearanceDefaults.filledButtonAppearance().copy(
            // typographyStyle = MaterialTheme.typography.labelLarge
        )
    )

    // Alternatively, create entirely from scratch:
    val customFilledButtonAppearance = PayPalPaymentSourceWidgetAppearance(
        actionButton = ButtonAppearanceDefaults.filledButtonAppearance().copy(
            // colors = ButtonDefaults.filledButtonColors(
            //     containerColor = Color.Blue,
            //     contentColor = Color.White
            // ),
            // shape = RoundedCornerShape(8.dp)
        )
    )


    PayPalSavePaymentSourceWidget(
        ...
        appearance = customAppearanceFromDefaults, // Use your custom appearance
    )
}
```

#### Style Attributes

The following attribute can be configured within `PayPalPaymentSourceWidgetAppearance`:

| Name           | Description                                                                                                                                                             | Type               | Default Value (from `PayPalPaymentSourceAppearanceDefaults`) |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|--------------------------------------------------------------|
| `actionButton` | Defines the appearance of the button used to initiate the PayPal account linking flow. Accepts any object conforming to the `ButtonAppearance` interface/class structure. | `ButtonAppearance` | `ButtonAppearanceDefaults.outlineButtonAppearance()`         |

---

**Note:**
*   The `actionButton` relies on the `ButtonAppearance` system. Refer to your internal documentation or implementation of `ButtonAppearance` (and its variants like `FilledButtonAppearance`, `OutlineButtonAppearance`, etc.) and `ButtonAppearanceDefaults` for detailed customization options (e.g., colors, typography, shape, icons).
*   The `PayPalVaultConfig` allows for optional `actionText` and `icon` parameters which directly affect the button's content, complementing the styling provided by `actionButton`.
