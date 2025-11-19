# Card Tokenisation Widget

> 
>
>Securely collect card details and transform them into a One Time Token. You can use this Token with other Paydock API calls, such as Capture Payment, Save Card to Vault, and so on.

![Creditcard View](/img/Credit_card.png)

## Supported Card Schemes

The Card Tokenisation Widget supports the following card schemes:

| Card Scheme   | Display Name              |
|---------------|---------------------------|
| AMEX          | American Express          |
| AUSBC         | Australian Bank Card      |
| DINERS        | Diners Club               |
| DISCOVER      | Discover                  |
| JAPCB         | JCB                       |
| MASTERCARD    | MasterCard                |
| SOLO          | Solo                      |
| VISA          | Visa                      |
| UNIONPAY      | UnionPay International    |

You can limit validation to specific schemes by configuring the `supportedSchemes` parameter in your widget configuration. This allows you to restrict which card types your application will accept based on your business requirements.

## iOS

## How to use the Card Tokenisation widget in iOS

### 1. Overview

The Card Tokenisation Widget collects your customer's payment information. This is a standalone widget that manages required inputs and validations. 

When the card information is input, the widget tokenises the card data, returning the One Time Token to the merchant.

The Card Details Widget is displayed as a SwiftUI sliding bottom sheet.

View details are as follows:

```Swift
CardDetailsWidget(viewState: ViewState? = nil
                  config: CardDetailsWidgetConfig,
                  loadingDelegate: WidgetLoadingDelegate? = nil,
                  completion: @escaping (Result<CardResult, CardDetailsError>) -> Void)
``` 

The following is an example of a SwiftUI View:

```Swift
struct CardDetailsWidgetView: View {
    var body: some View {
        NavigationStack {
            ScrollView {
                CardDetailsWidget(
                    viewState: nil,
                    config: getConfig(),
                    appearance: CardDetailsWidgetAppearance = CardDetailsWidgetAppearance(),
                    loadingDelegate: nil,
                    completion: { result in
                        switch result {
                        case .success(let result): // handle success
                        case .failure(let error): // handle failure
                        }
                })
            }
        }
    }
    
    private func getConfig() -> CardDetailsWidgetConfig {
        let privacyPolicyConfig = SaveCardConfig.PrivacyPolicyConfig(
            privacyPolicyText: "<insert link text>",
            privacyPolicyURL: "<insert link URL string>")
        
        let saveCardConfig = SaveCardConfig(
            consentText: "<insert consent text>",
            privacyPolicyConfig: privacyPolicyConfig)
        
        let supportedSchemesConfig = SupportedSchemesConfig(
            supportedSchemes: [.amex, .visa], // optional
            enableValidation: true)
        
        let config = CardDetailsWidgetConfig(
            gatewayId: "<insert gateway id>", // optional
            accessToken: "<access token>", // mandatory
            actionText: "<override default action text>", // optional
            showCardTitle: true, // whether to show card title view
            collectCardholderName: true, // whether to show cardholder name field
            allowSaveCard: saveCardConfig,
            schemeSupport: supportedSchemesConfig)
        
        return config
    }
}
```

The below table describes the various inline validation errors linked to the `CardDetailsWidget`.

| Field           | Error Message                                                                                       | Validation                                        |
| --------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| Cardholder name | "*Card number is in the wrong field!*"                                                              | Luhn Algorithm Failure                            |
| Cardholder name | "*Invalid name*"                                                                                    | Empty Field                                       |
| Card number     | "*Invalid card number*"                                                                             | Empty Field                                       |
| Card number     | "*Invalid card number*"                                                                             | Field contains non-number                         |
| Card number     | "*Invalid card number*"                                                                             | Luhn Algorithm Failure                            |
| Card number     | "*Card type not accepted*"                                                                          | Card scheme not supported                         |
| Expiry          | “*Invalid expiry date*”                                                                             | Empty Field                                       |
| Expiry          | “*Invalid expiry date*”                                                                             | Field contains non-number                         |
| Expiry          | “*Invalid expiry date*”                                                                             | Field matches expected size                       |
| Expiry          | "*Card expired*"                                                                                    | Date expired                                      |
| Security code   | "*Invalid security code*"                                                                           | Empty Field                                       |
| Security code   | "*Invalid security code*"                                                                           | Field contains non-number                         |
| Security code   | "*Invalid security code*"                                                                           | Field matches expected size (based on code type)  |

#### Cardholder name

"*Card number is in the wrong field!*" -> 
"*Invalid name*" -> Field is empty

### 2. Parameter definitions

#### MobileSDK.CardDetailsWidget

| Name                     | Definition                                                                                       | Type                                               | Mandatory/Optional |
| ------------------------ | ------------------------------------------------------------------------------------------------ | -------------------------------------------------- |------------------  |
| viewState                |  View options that are two way fields to alter view state                                        | ViewState                                          | Optional           |
| config                   |  Configuration options for the card details widget                                               | `CardDetailsWidgetConfig`                          | Required           |
| appearance               |  Customization options for the visual appearance of the widget                                   | `CardDetailsWidgetAppearance`                      | Optional           |
| loadingDelegate          |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown. | WidgetLoadingDelegate                              | Optional           |
| completion               |  Completion handler that returns success or failure depending on the widget outcome              | `(Result<CardResult, CardDetailsError>) -> Void)`  | Mandatory          |

#### MobileSDK.ViewState

| Name                   | Definition                                                              | Type                                               | Mandatory/Optional |
| ---------------------- | ----------------------------------------------------------------------- | -------------------------------------------------- |------------------  |
| state                  |  Sets the state the widget should be placed in                          | Enum (disabled, none)                              | Mandatory          |


#### MobileSDK.CardDetailsWidgetConfig

| Name                     | Definition                                                                                       | Type                                               | Mandatory/Optional |
| ------------------------ | ------------------------------------------------------------------------------------------------ | -------------------------------------------------- |------------------  |
| gatewayId                |  Gateway ID that the merchant can input into the widget to allow for tokenisation                | String                                             | Optional           |
| accessToken              |  The access token used for authentication with the backend service.                              | String                                             | Mandatory          |
| actionText               |  Text in the main action button that initiates tokenisation (default is "Submit")                | String                                             | Optional           |
| showCardTitle            |  A flag indicating whether to show the card title (default is true).                             | Boolean                                            | Optional           |
| collectCardholderName    |  A flag indicating whether to show the cardholder name input (default is true).                  | Boolean                                            | Optional           |
| allowSaveCard            |  Configures the widget component that allows the user to toggle the switch for desired question  | `SaveCardConfig`                                   | Optional           |
| schemeSupport            |  Configures the card scheme support and validation behavior                                      | `SupportedSchemesConfig`                           | Optional           |

#### MobileSDK.SaveCardConfig

| Name                   | Definition                                                              | Type                                               | Mandatory/Optional |
| ---------------------- | ----------------------------------------------------------------------- | -------------------------------------------------- |------------------  |
| consentText            |  Sets the text to the first line of the consent component               | String                                             | Optional           |
| privacyPolicyConfig    |  Sets the privacy policy for the second line of consent component       | MobileSDK.PrivacyPolicyConfig                      | Optional           |

#### MobileSDK.PrivacyPolicyConfig

| Name                   | Definition                                                              | Type       | Mandatory/Optional |
| ---------------------- | ----------------------------------------------------------------------- | ---------- |------------------  |
| privacyPolicyText      |  Sets the text of the tappable second line of the component             | String     | Mandatory          |
| privacyPolicyURL       |  Sets the URL link that opens in external browser on tap                | String     | Mandatory          |

#### MobileSDK.SupportedSchemesConfig

| Name                   | Definition                                                                   | Type                                               | Mandatory/Optional |
| ---------------------- | ---------------------------------------------------------------------------- | -------------------------------------------------- |------------------  |
| supportedSchemes       |  A unique set of supported card schemes                                      | `Set<CardScheme>`                                  | Optional           |
| enableValidation       |  A flag to determine if validation should run for the `supportedSchemes`     | Boolean                                            | Optional           |

#### MobileSDK.CardDetailsError

| Name                       | Description                                                               | Error Result            |
| :------------------------ | :------------------------------------------------------------------------- | :---------------------- |
| errorTokenisingCard       |  Error thrown when provided widget failed card tokenisation                |  ErrorRes               |
| unknownError              |  Error thrown when there is an unknown error related to card tokenisation  |  nil                    |

### 3. Widget Styling

Defines the visual appearance and layout attributes for the `CardDetailsWidget`. This allows for extensive customisation of how the various elemets are displayed to the user.

#### Appearance Contract

The `CardDetailsWidgetAppearance` class encapsulates all configurable style properties for the widget.

```Swift
public struct CardDetailsWidgetAppearance {
    public var verticalSpacing: CGFloat
    public var horizontalSpacing: CGFloat
    public var title: Theme.TextAppearance
    public var textField: Theme.TextFieldAppearance
    public var actionButton: Theme.ButtonAppearance
    public var toolbarButton: Theme.ButtonAppearance
    public var toggle: Theme.ToggleAppearance
    public var toggleText: Theme.TextAppearance
    public var linkText: Theme.TextAppearance
}
```

#### Default Appearance & Customisation

A default appearance is provided by `GlobalTheme` values for each of the subcomponents. You can use this as a starting point and customise specific attributes as needed.

##### Using Default Appearance

```Swift
    CardDetailsWidget( 
        ...
        appearance: CardDetailsWidgetAppearance = CardDetailsWidgetAppearance() // Uses the default appearance
    )
```

##### Customising Appearance

You can create a custom `CardDetailsWidgetAppearance` or modify the default one.

```Swift
struct MyCustomCardDetailsScreen: View { 
    private func myCustomAppearance() -> CardDetailsWidgetAppearance {
        let title = Theme.TextAppearance.init(text: .init(font: .init(name: "CustomFont", size: 20), isUnderlined: true, underlineColor: .black))
        let textField = Theme.TextFieldAppearance.init(colors: .init(active: .blue), dimensions: .init(cornerRadius: 20.0))
        let actionButton = Theme.ButtonAppearance.init(colors: .init(background: .blue))
        let appearance = CardDetailsWidgetAppearance(
            verticalSpacing: 10,
            horizontalSpacing: 16,
            title: title,
            textField: textField,
            actionButton: actionButton)
        return appearance
    }
    
    var body: some View {
                CardDetailsWidget( 
            ...
            appearance: CardDetailsWidgetAppearance = myCustomAppearance()
            ...
        )
    }
}
```

#### Style Attributes

The following attributes can be configured within `CardDetailsWidgetAppearance`:

| Name                   | Description                                                                                                | Type                           | Default Value                     |
|------------------------|------------------------------------------------------------------------------------------------------------|--------------------------------|-----------------------------------|
| `verticalSpacing`      | The vertical space between the groups of different elements on the screen.                                 | `CGFloat`                      | `16.0`                            |
| `horizontalSpacing`    | The horizontal space between the card number input field and the PIN input field within their shared row.  | `CGFloat`                      | `8.0`                             |
| `title`                | Defines the appearance of the section titles on the screen.                                                | `TextAppearance`               | `GlobalTheme.title`               |
| `textField`            | Defines the appearance of the card number and PIN input text fields.                                       | `TextFieldAppearance`          | `GlobalTheme.textField`           |
| `actionButton`         | Defines the appearance of the primary submit button.                                                       | `ButtonAppearance`             | `GlobalTheme.actionButton`        |
| `toolbarButton`        | Defines the appearance of the keyboard accessory button.                                                   | `ButtonAppearance`             | `GlobalTheme.toolbarButton`       |
| `toggle`               | Defines the appearance of the toggle element.                                                              | `ToggleAppearance`             | `GlobalTheme.toggle`              |
| `toggleText`           | Defines the appearance of the text linked to the toggle.                                                   | `TextAppearance`               | `GlobalTheme.toggleText`          |
| `linkText`             | Defines the appearance of the link element in the UI.                                                      | `TextAppearance`               | `GlobalTheme.linkText`            |

---

**Note:**
*   The `TextAppearance`, `TextFieldAppearance`, `ButtonAppearance` and `ToggleAppearance` themselves would have their own detailed documentation explaining their configurable attributes (like colors, typography, borders, etc.). This documentation focuses on how they are composed within the `CardDetailsWidgetAppearance`.

## Android

## How to use the CardDetailsWidget

### 1. Overview

This section provides a step-by-step guide on how to initialise and use the `CardDetailsWidget` composable in your application. The widget tokenises card details.

The following sample code demonstrates the definition of the `CardDetailsWidget`:

```Kotlin
@Composable
fun CardDetailsWidget(
    modifier: Modifier = Modifier,
    enabled: Boolean = true,
    config: CardDetailsWidgetConfig,
    appearance: CardDetailsWidgetAppearance = CardDetailsAppearanceDefaults.appearance(),
    loadingDelegate: WidgetLoadingDelegate? = null,
    eventDelegate: WidgetEventDelegate? = null,
    completion: (Result<CardResult>) -> Unit
) {...}
```

The following sample code example demonstrates how to use the widget in your application:

```Kotlin
// Initialise the CardDetailsWidget
CardDetailsWidget(
    modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp), // optional
    enabled: Boolean, // optional
    config = CardDetailsWidgetConfig(
        accessToken = ACCESS_TOKEN, // required
        gatewayId = GATEWAY_ID, // optional
        actionText: "<override default action button text>",
        showCardTitle: true, // whether to show card title view
        collectCardholderName: true, // whether to show cardholder name field
        allowSaveCard = SaveCardConfig(
            privacyPolicyConfig = SaveCardConfig.PrivacyPolicyConfig(
                privacyPolicyURL = "https://www.privacypolicy.com"
            )
        ),
        schemeSupport = SupportedSchemeConfig(
            supportedSchemes = CardScheme.entries.toSet(),
            enableValidation = true // optional
        )
    ),
    appearance = currentOrDefaultAppearance, // optional
    loadingDelegate = DELEGATE_INSTANCE, // Delegate class to handle loading
    eventDelegate = EVENT_DELEGATE_INSTANCE, // Delegate class to handle events
    completion = { result ->
        result.onSuccess { cardResult ->
            // Handle success - Update UI or perform actions
            Log.d("CardDetailsWidget", "Tokenisation successful. Card token: ${cardResult.token}")
        }.onFailure { throwable ->
            // Handle failure - Show error message or take appropriate action
            Log.e("CardDetailsWidget", "Tokenisation failed. Error: ${exception.message}")
        }
    }
)
```

The below table describes the various inline validation errors linked to the `CardDetailsWidget`.

| Field           | Error Message                                                                                       | Validation                                        |
| --------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| Cardholder name | "*Card number is in the wrong field!*"                                                              | Luhn Algorithm Failure                            |
| Cardholder name | "*Invalid name*"                                                                                    | Default                                           |
| Card number     | "*Invalid card number*"                                                                             | Empty Field                                       |
| Card number     | "*Invalid card number*"                                                                             | Field contains non-number                         |
| Card number     | "*Invalid card number*"                                                                             | Luhn Algorithm Failure                            |
| Card number     | "*Card type not accepted*"                                                                          | Card scheme not supported                         |
| Expiry          | “*Invalid expiry date*”                                                                             | Empty Field                                       |
| Expiry          | “*Invalid expiry date*”                                                                             | Field contains non-number                         |
| Expiry          | “*Invalid expiry date*”                                                                             | Field matches expected size                       |
| Expiry          | "*Card expired*"                                                                                    | Date expired                                      |
| Security code   | "*Invalid security code*"                                                                           | Empty Field                                       |
| Security code   | "*Invalid security code*"                                                                           | Field contains non-number                         |
| Security code   | "*Invalid security code*"                                                                           | Field matches expected size (based on code type)  |

### 2. Parameter definitions

This subsection describes the parameters required by the `CardDetailsWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `CardDetailsWidget`.

#### CardDetailsWidget
| Name                    | Definition                                                                                                | Type                           | Mandatory/Optional |
| :------------------     | :-------------------------------------------------------------------------------------------------------- | :----------------------------- | :----------------  |
| modifier                |  Compose modifier for container modifications.                                                            | `Modifier`                     | Optional           |
| enabled                 |  Controls the enabled state of this Widget.                                                               | Boolean                        | Optional           |
| config                  |  Configuration options for the card details widget                                                        | `CardDetailsWidgetConfig`      | Mandatory          |
| appearance              |  Customization options for the visual appearance of the widget                                            | `CardDetailsWidgetAppearance`  | Optional           |
| loadingDelegate         |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown.          | `WidgetLoadingDelegate`        | Optional           |
| eventDelegate           |  Delegate for handling widget events such as button clicks, toggle interactions, and link clicks.         | `WidgetEventDelegate`          | Optional           |
| completion              |  Result callback with the card details tokenisation API response if successful, or error if not.          | `(Result<CardResult>) -> Unit` | Mandatory          |

#### CardDetailsWidgetConfig

| Name                     | Definition                                                                                       | Type                                               | Mandatory/Optional             |
| ------------------------ | ------------------------------------------------------------------------------------------------ | -------------------------------------------------- |------------------------------  |
| accessToken              |  Widget access token used for internal API communication                                         | String                                             | Mandatory                      |
| gatewayId                |  Gateway ID used for the card tokenisation.                                                      | String                                             | Optional                       |
| actionText               |  The text to display on the action button (default is "Submit").                                 | String                                             | Optional                       |
| showCardTitle            |  A flag indicating whether to show the card title (default is true).                             | Boolean                                            | Optional                       |
| collectCardholderName    |  A flag indicating whether to show the cardholder name input (default is true).                  | Boolean                                            | Optional                       |
| allowSaveCard            |  Configuration for showing the save card UI toggle.                                              | `SaveCardConfig`                                   | Optional                       |  
| schemeSupport            |  Configuration for supported card schemes and scheme validation behavior.                        | `SupportedSchemeConfig`                            | Optional                       |  

#### SaveCardConfig
| Name                | Definition                                                                                                   | Type                     | Mandatory/Optional |
| :------------------ | :----------------------------------------------------------------------------------------------------------- | :----------------------- | :----------------  |
| consentText         |  The text displayed to the user for card saving consent. (default is "Remember this card for next time.")    | `Modifier`               | Optional           |
| privacyPolicyConfig |  The configuration for the privacy policy, if applicable.                                                    | `PrivacyPolicyConfig`    | Optional           |

#### PrivacyPolicyConfig
| Name                | Definition                                                                                            | Type             | Mandatory/Optional |
| :------------------ | :---------------------------------------------------------------------------------------------------- | :--------------- | :----------------  |
| privacyPolicyText   |  The text displayed to the user for the privacy policy link. (default is "Read our privacy policy")   | String           | Optional           |
| privacyPolicyURL    |  The URL of the privacy policy document.                                                              | String           | Mandatory          |

#### SupportedSchemesConfig

| Name                   | Definition                                                                                    | Type                                               | Mandatory/Optional |
| ---------------------- | --------------------------------------------------------------------------------------------- | -------------------------------------------------- |------------------  |
| supportedSchemes       |  A unique set of supported card schemes                                                       | `Set<CardScheme>`                                  | Optional           |
| enableValidation       |  A flag to determine if validation should run for the `supportedSchemes` (default is false).  | Boolean                                            | Optional           |

#### CardResult
| Name                | Definition                                                                                           | Type             | Mandatory/Optional |
| :------------------ | :--------------------------------------------------------------------------------------------------- | :--------------- | :----------------  |
| token               |  The token associated with the card.                                                                 | String           | Mandatory          |
| saveCard            |  A boolean flag indicating whether the card should be saved for future use. (default is false).      | String           | Optional           |

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

This `eventDelegate` allows the calling app to receive notifications of user interactions within the widget, such as button clicks, toggle interactions, and link clicks. This is useful for analytics and tracking purposes.

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

##### Card Details Events

The Card Details Widget triggers the following events:

**Tokenisation Event** - Triggered when the tokenisation button is clicked:

```json
{
  "type": "Button",
  "properties": {
    "name": "TokenisationButton",
    "action": "click", 
    "text": "(Whatever is set by merchant)"
  }
}
```

**Toggle Event** - Triggered when the save card toggle is clicked:

```json
{
  "type": "Toggle",
  "properties": {
    "name": "SaveCardToggle",
    "action": "click",
    "state": "true / false"
  }
}
```

**TextLink Event** - Triggered when the privacy policy link is clicked:

```json
{
  "type": "LinkText", 
  "properties": {
    "name": "PrivacyPolicyLink",
    "action": "click",
    "url": "(Whatever is set by merchant)"
  }
}
```

| Property | Description | Type | Optional/Required |
|----------|-------------|------|-------------------|
| `type` | The type of UI element that triggered the event | String | Required |
| `properties.name` | The name identifier of the specific element | String | Required |
| `properties.action` | The action performed on the element | String (Enum) | Required |
| `properties.text` | The text content of the element (Button events only) | String | Optional |
| `properties.state` | The toggle state (Toggle events only) | Boolean | Required (Toggle only) |
| `properties.url` | The URL associated with the link (LinkText events only) | String | Required (LinkText only) |

#### Completion Callback

The `completion` callback is invoked after the card tokenisation operation is completed. It receives a `Result<CardResult>`. The `Result<CardResult>` contains the token and the toggled state flag of the user selection to save the card details.

### 4. Error/Exceptions Mapping

The following describes Card Details exceptions that can be thrown. 

```Kotlin
TokenisingCardException(error: ApiErrorResponse) : CardDetailsException(error.displayableMessage)
UnknownException(displayableMessage: String) : CardDetailsException(displayableMessage)
```

| Exception                 | Description                                                                       | Error Model         |
| :------------------------ | :-------------------------------------------------------------------------------- | :------------------ |
| TokenisingCardException   |  Exception thrown when there is an error tokenising a card.                       |  CardDetailsError   |
| UnknownException          |  Exception thrown when there is an unknown error related to Card Details.         |  CardDetailsError   |

### 5. Widget Styling

Defines the visual appearance and layout attributes for the `CardDetailsWidget`. This class allows for comprehensive customisation of how card input fields (number, expiry, CVV, cardholder name), title, "save card" toggle, and the action button are presented.

#### Appearance Contract

The `CardDetailsWidgetAppearance` class encapsulates all configurable style properties for the widget.
```Kotlin
@Immutable
class CardDetailsWidgetAppearance(
    val verticalSpacing: Dp,
    val horizontalSpacing: Dp,
    val textFieldVerticalSpacing: Dp,
    val textFieldHorizontalSpacing: Dp,
    val title: TextAppearance,
    val textField: TextFieldAppearance,
    val actionButton: ButtonAppearance,
    val toggle: ToggleAppearance,
    val toggleText: TextAppearance,
    val linkText: LinkTextAppearance
)
```

#### Default Appearance & Customisation

A default appearance is provided by `CardDetailsAppearanceDefaults`. This uses `MaterialTheme` values for a standard look and feel. You can use this as a starting point and customise specific attributes.

##### Using Default Appearance


```Kotlin
    CardDetailsWidget( 
        ...
        appearance = CardDetailsAppearanceDefaults.appearance() // Uses the default appearance
    )
```

##### Customising Appearance

You can create a custom `CardDetailsWidgetAppearance` or modify the default one using its `copy` method.

```Kotlin
@Composable 
fun MyCustomCardDetailsScreen() { 
    // Create appearance by using provided defaults, with custom changes
    val customAppearance = CardDetailsAppearanceDefaults.appearance().copy( 
        verticalSpacing = 12.dp, 
        title = TextAppearanceDefaults.appearance().copy( style = MaterialTheme.typography.headlineSmall.copy( // color = MyAppColors.primary ) ), 
        textField = TextFieldAppearanceDefaults.outlineAppearance().copy( // Example: Customizing text field label color // labelColor = MyAppColors.onSurfaceVariant ), 
        toggleText = TextAppearanceDefaults.appearance().copy( style = MaterialTheme.typography.labelMedium.copy( // color = MyAppColors.secondary ) ) 
    )

    // Alternatively, create entirely from scratch:
    val completelyCustomAppearance = CardDetailsWidgetAppearance(
        verticalSpacing = 10.dp,
        horizontalSpacing = 6.dp,
        title = TextAppearanceDefaults.appearance(/*...custom title params...*/),
        textField = TextFieldAppearanceDefaults.filledAppearance(/*...custom text field params...*/),
        actionButton = ButtonAppearanceDefaults.textButtonAppearance(/*...custom button params...*/),
        toggle = ToggleAppearanceDefaults.appearance(/*...custom toggle params...*/),
        toggleText = TextAppearanceDefaults.appearance(/*...custom toggle text params...*/),
        linkText = LinkTextAppearanceDefaults.appearance(/*...custom link text params...*/)
    )

    CardDetailsWidget(
        ...
        appearance = customAppearance, // Use your custom appearance
    )
}
```

#### Style Attributes

The following attributes can be configured within `CardDetailsWidgetAppearance`:

| Name                          | Description                                                                                                                                  | Type                                                            | Default Value (from `CardDetailsAppearanceDefaults`)                                                               |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| `verticalSpacing`             | The vertical space between major elements in the widget (e.g., between title and inputs, inputs and toggle, toggle and action button).         | `androidx.compose.ui.unit.Dp`                                   | `WidgetDefaults.Spacing`                                                                                           |
| `horizontalSpacing`           | The horizontal spacing within composite elements (e.g., between the two text fields in the expiry/CVV row if they were side-by-side).           | `androidx.compose.ui.unit.Dp`                                   | `WidgetDefaults.Spacing`                                                                                           |
| `textFieldVerticalSpacing`    | The vertical spacing between text input fields.                                                                                              | `androidx.compose.ui.unit.Dp`                                   | `WidgetDefaults.Spacing`                                                                                           |
| `textFieldHorizontalSpacing`  | The horizontal spacing between text input fields.                                                                                            | `androidx.compose.ui.unit.Dp`                                   | `WidgetDefaults.Spacing`                                                                                           |
| `title`                       | Defines the text appearance for the widget's main title (e.g., "Card Information").                                                          | `TextAppearance`               | `TextAppearanceDefaults.appearance()` with `bodyMedium` style and `onSurface` color.                               |
| `textField`                   | Defines the appearance for all card input text fields (card number, expiry, CVV, cardholder name).                                            | `TextFieldAppearance`       | `TextFieldAppearanceDefaults.appearance().copy(singleLine = true)`                                                 |
| `actionButton`                | Defines the appearance of the primary submit/action button.                                                                                  | `ButtonAppearance`           | `ButtonAppearanceDefaults.filledButtonAppearance()`                                                                |
| `toggle`                      | Defines the appearance of the switch or checkbox used for options like "Save card".                                                          | `ToggleAppearance`             | `ToggleAppearanceDefaults.appearance()`                                                                            |
| `toggleText`                  | Defines the text appearance for the label associated with the toggle (e.g., "Save this card for future payments").                             | `TextAppearance`             | `TextAppearanceDefaults.appearance()` with `bodyMedium` style.                                                     |
| `linkText`                    | Defines the text appearance for any interactive link elements within the widget (e.g., a "Learn More" link next to the "Save card" option).     | `LinkTextAppearance`           | `LinkTextAppearanceDefaults.appearance()`                                                                          |

---

**Note:**
*   The appearance types (`TextAppearance`, `TextFieldAppearance`, `ButtonAppearance`, `ToggleAppearance`, `LinkTextAppearance`) would each have their own detailed documentation.
