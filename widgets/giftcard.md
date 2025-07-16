# Gift Card

> 
>
> Securely collect gift card details using a prebuilt form, before transforming them into a One Time Token. You can use this Token with other Paydock API calls, such as Capture Payment.

![Giftcard View](/img/Gift_Card.png) 

## iOS

## How to use the GiftCardView widget in iOS

### 1. Overview

The `GiftCardView` captures gift card details and facilitates the tokenisation of your card within your iOS app. This section provides a step-by-step guide on how to initialise and use the `GiftCardView` in your application.

The following sample code demonstrates how to use the `GiftCardView` composable in your application:


```Swift
GiftCardWidget(
    viewState: ViewState? = nil,
    config: GiftCardWidgetConfig,
    appearance: GiftCardWidgetAppearance = GiftCardWidgetAppearance(),
    loadingDelegate: WidgetLoadingDelegate? = nil,
    completion: @escaping (Result<GiftCardResult, GiftCardError>) -> Void)
    { ... }
```

The widget returns a token when the gift card is successfully tokenised. If there is an error, the widget returns an **Error** object that includes information about the cause of the failure.

The following is an example of a GiftCardView initialisation:

```Swift
struct GiftCardExampleView: View {
    var body: some View {
        GiftCardWidget(
            config: GiftCardWidgetConfig(accessToken: <your_access_token>, storePin: false),
            appearance: GiftCardWidgetAppearance()) { result in
                switch result {
                case .success(let giftCardResult): self.alertMessage = giftCardResult.token
                case .failure(let error): self.alertMessage = error.localizedDescription
            }
        }
    }
}
```

### 2. Parameter Definitions

#### MobileSDK.GiftCardView
| Name            | Definition                                                                                       | Type                                | Mandatory/Optional |
| :-----------    | :----------------------------------------------------------------------------------------------  | :---------------------------------- | :----------------  |
| viewState       |  View options that are two way fields to alter view state                                        | `ViewState`                         | Optional           |
| config          |  Configuration options for the gift card widget                                                  | `MobileSDK.GiftCardWidgetConfig`    | Mandatory          |
| appearance      |  Customization options for the visual appearance of the widget                                   | `GiftCardWidgetAppearance`          | Optional           |
| loadingDelegate |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown. | `WidgetLoadingDelegate`             | Optional           |
| completion      |  A completion block that returns result with token or error.                                     | `(Result<String, Error>) -> Void`   | Mandatory          |

#### MobileSDK.GiftCardWidgetConfig
| Name            | Definition                                                                                       | Type                              | Mandatory/Optional |
| :-----------    | :----------------------------------------------------------------------------------------------  | :-------------------------------- | :----------------  |
| storePin        |  A flag to be able to use a PIN value for the initial transaction.                               | Bool (default = true)             | Optional           |
| accessToken     |  The access token used for authentication with the backend service.                              | String                            | Mandatory          |

#### MobileSDK.GiftCardError

| Name                       | Description                                                                    | Error Result            |
| :------------------------ | :------------------------------------------------------------------------------ | :---------------------- |
| errorTokenisingCard       |  Error thrown when provided widget failed gift card tokenisation                |  ErrorRes               |
| unknownError              |  Error thrown when there is an unknown error related to gift card tokenisation  |  nil                    |

### 5. Widget Styling

Defines the visual appearance and layout attributes for the `GiftCardWidget`. This allows for extensive customisation of how the gift card input fields and action button are presented to the user.

#### Appearance Contract

The `GiftCardWidgetAppearance` class encapsulates all configurable style properties for the widget.

```Swift
public struct GiftCardWidgetAppearance {
    public var verticalSpacing: CGFloat
    public var horizontalSpacing: CGFloat
    public var title: Theme.TextAppearance
    public var textField: Theme.TextFieldAppearance
    public var actionButton: Theme.ButtonAppearance
    public var toolbarButton: Theme.ButtonAppearance
}
```

#### Default Appearance & Customisation

A default appearance is provided by `GlobalTheme` values for each of the subcomponents. You can use this as a starting point and customise specific attributes as needed.

##### Using Default Appearance

```Swift
    GiftCardWidget( 
        ...
        appearance: GiftCardWidgetAppearance = GiftCardWidgetAppearance() // Uses the default appearance
    )
```

##### Customising Appearance

You can create a custom `GiftCardWidgetAppearance` or modify the default one.

```Swift
struct MyCustomGiftCardScreen: View { 
    private func myCustomAppearance() -> GiftCardWidgetAppearance {
        let title = Theme.TextAppearance.init(text: .init(font: .init(name: "CustomFont", size: 20), isUnderlined: true, underlineColor: .black))
        let textField = Theme.TextFieldAppearance.init(colors: .init(active: .blue), dimensions: .init(cornerRadius: 20.0))
        let actionButton = Theme.ButtonAppearance.init(colors: .init(background: .blue))
        let appearance = GiftCardWidgetAppearance(
            verticalSpacing: 10,
            horizontalSpacing: 16,
            title: title,
            textField: textField,
            actionButton: actionButton)
        return appearance
    }
    
    var body: some View {
            GiftCardWidget( 
            ...
            appearance: GiftCardWidgetAppearance = myCustomAppearance()
            ...
        )
    }
}
```

#### Style Attributes

The following attributes can be configured within `GiftCardWidgetAppearance`:

| Name                | Description                                                                                                | Type                           | Default Value              |
|---------------------|------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|
| `verticalSpacing`   | The vertical space between the row of input fields (card number, PIN) and the action button below it.      | `CGFloat`                      | `16.0`                     |
| `horizontalSpacing` | The horizontal space between the card number input field and the PIN input field within their shared row.  | `CGFloat`                      | `6.0`                      |
| `textField`         | Defines the appearance of the card number and PIN input text fields.                                       | `TextFieldAppearance`          | `GlobalTheme.textField`    |
| `actionButton`      | Defines the appearance of the primary submit button.                                                       | `ButtonAppearance`             | `GlobalTheme.actionButton` |
| `toolbarButton`     | Defines the appearance of the  submit button.                                                              | `ButtonAppearance`             | `GlobalTheme.toolbarButton`|

---

**Note:**
*   The `TextFieldAppearance` and `ButtonAppearance` themselves would have their own detailed documentation explaining their configurable attributes (like colors, typography, borders, etc.). This documentation focuses on how they are composed within the `GiftCardWidgetAppearance`.

## Android

## How to use the GiftCardWidget in Android

### 1. Overview

This section provides a step-by-step guide on how to initialise and use the `GiftCardWidget` composable in your application. The widget performs tokenisation of gift card details.

The following sample code demonstrates the definition of the `GiftCardWidget`:

```Kotlin
@Composable
fun GiftCardWidget(
    modifier: Modifier = Modifier,
    enabled: Boolean = true,
    config: GiftCardWidgetConfig,
    appearance: GiftCardWidgetAppearance = GiftCardAppearanceDefaults.appearance(),
    loadingDelegate: WidgetLoadingDelegate? = null,
    completion: (Result<String>) -> Unit,
) {...}
```

The following sample code example demonstrates the usage within your application:

```Kotlin
// Initialise the GiftCardWidget
GiftCardWidget(
    modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp), // optional
    config = GiftCardWidgetConfig(
        accessToken = BuildConfig.WIDGET_ACCESS_TOKEN,
        storePin = true
    ),
    appearance = currentOrDefaultAppearance, // optional
    completion = { result ->
        result.onSuccess { token ->
            // Handle success - Update UI or perform actions
            Log.d("GiftCardWidget", "Tokenisation successful. Card token: $token")
        }.onFailure { throwable ->
            // Handle failure - Show error message or take appropriate action
            Log.e("GiftCardWidget", "Tokenisation failed. Error: ${exception.message}")
        }
    }
)
```

### 2. Parameter definitions

This subsection describes the various parameters required by the `GiftCardWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `GiftCardWidget`.

#### GiftCardWidget

| Name                | Definition                                                                                                | Type                        | Mandatory/Optional |
| :------------------ | :-------------------------------------------------------------------------------------------------------- | :-------------------------- | :----------------  |
| modifier            |  Compose modifier for container modifications                                                             | `Modifier`                  | Optional           |
| enabled             |  Controls the enabled state of this Widget.                                                               | Boolean                     | Optional           |
| config              |  Configuration options for the gift card widget                                                           | `GiftCardWidgetConfig`      | Mandatory          |
| appearance          |  Customization options for the visual appearance of the widget                                            | `GiftCardWidgetAppearance`  | Optional           |
| loadingDelegate     |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown.          | `WidgetLoadingDelegate`     | Optional           |
| completion          |  Result callback with the gift card details tokenisation API response if successful, or error if not.     | `(Result<String>) -> Unit`  | Mandatory          |

#### GiftCardWidgetConfig

| Name                     | Definition                                                                                       | Type                        | Mandatory/Optional |
| ------------------------ | ------------------------------------------------------------------------------------------------ | --------------------------- |------------------  |
| accessToken              |  The access token used for authentication with the backend service.                              | String                      | Mandatory          |
| storePin                 |  A flag to be able to use a PIN value for the initial transaction.                               | String (default = true)     | Optional           |

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

#### Completion Callback

The `completion` callback is invoked after the gift card tokenisation operation is completed. It receives a `Result<String>` if the gift card details was tokenised successfully.

### 4. Error/Exceptions Mapping

The following describes Gift Card Detail exceptions that can be thrown. 

```Kotlin
TokenisingCardException(error: ApiErrorResponse) : GiftCardException(error.displayableMessage)
UnknownException(displayableMessage: String) : GiftCardException(displayableMessage)
```

| Exception                 | Description                                                                            | Error Model      |
| :------------------------ | :------------------------------------------------------------------------------------- | :--------------- |
| TokenisingCardException   |  Exception thrown when there is an error tokenising a gift card.                       |  GiftCardError   |
| UnknownException          |  Exception thrown when there is an unknown error related to Gift Card Details.         |  GiftCardError   |

### 5. Widget Styling

Defines the visual appearance and layout attributes for the `GiftCardWidget`. This allows for extensive customisation of how the gift card input fields and action button are presented to the user.

#### Appearance Contract

The `GiftCardWidgetAppearance` class encapsulates all configurable style properties for the widget.

```Kotlin
@Immutable
class GiftCardWidgetAppearance(
    val verticalSpacing: Dp,
    val horizontalSpacing: Dp,
    val textField: TextFieldAppearance,
    val actionButton: ButtonAppearance,
)
```

#### Default Appearance & Customisation

A default appearance is provided by `GiftCardAppearanceDefaults`. You can use this as a starting point and customise specific attributes as needed.

##### Using Default Appearance


```Kotlin
    GiftCardWidget( 
        ...
        appearance = GiftCardAppearanceDefaults.appearance() // Uses the default appearance
    )
```

##### Customising Appearance

You can create a custom `GiftCardWidgetAppearance` or modify the default one.

```Kotlin
@Composable 
fun MyCustomGiftCardScreen() { 
    // Create appearance by using provided defaults, with custom changes
    val customAppearance = GiftCardAppearanceDefaults.appearance().copy( 
        verticalSpacing = 12.dp,
        horizontalSpacing = 8.dp, 
        textField = TextFieldAppearanceDefaults.appearance().copy( // Example: Customizing text field text color // textColor = MyAppColors.onSurfaceVariant ),
        actionButton = ButtonAppearanceDefaults.filledButtonAppearance().copy( // Example: Customizing button background color // containerColor = MyAppColors.primary, // contentColor = MyAppColors.onPrimary ) 
    )

    // Alternatively, create entirely from scratch:
    val completelyCustomAppearance = GiftCardWidgetAppearance(
        verticalSpacing = 10.dp,
        horizontalSpacing = 10.dp,
        textField = TextFieldAppearanceDefaults.outlineAppearance().copy(
            // Example: Customizing text field border
            // unfocusedBorderColor = MyAppColors.outline
        ),
        actionButton = ButtonAppearanceDefaults.textButtonAppearance().copy(
            // Example: Customizing text button content color
            // contentColor = MyAppColors.tertiary
        )
    )

    GiftCardWidget(
        ...
        appearance = customAppearance, // Use your custom appearance
    )
}
```

#### Style Attributes

The following attributes can be configured within `GiftCardWidgetAppearance`:

| Name                | Description                                                                                                | Type                                                        | Default Value (from `GiftCardAppearanceDefaults`) |
|---------------------|------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|---------------------------------------------------|
| `verticalSpacing`   | The vertical space between the row of input fields (card number, PIN) and the action button below it.      | `androidx.compose.ui.unit.Dp`                               | `WidgetDefaults.Spacing` (e.g., 16.dp)            |
| `horizontalSpacing` | The horizontal space between the card number input field and the PIN input field within their shared row.  | `androidx.compose.ui.unit.Dp`                               | `WidgetDefaults.Spacing` (e.g., 16.dp)            |
| `textField`         | Defines the appearance of the card number and PIN input text fields.                                       | `TextFieldAppearance`         | `TextFieldAppearanceDefaults.appearance().copy(singleLine = true)` |
| `actionButton`      | Defines the appearance of the primary submit button.                                                       | `ButtonAppearance`             | `ButtonAppearanceDefaults.filledButtonAppearance()` |

---

**Note:**
*   The `TextFieldAppearance` and `ButtonAppearance` themselves would have their own detailed documentation explaining their configurable attributes (like colors, typography, borders, etc.). This documentation focuses on how they are composed within the `GiftCardWidgetAppearance`.
*   The example default values like `16.dp` for `WidgetDefaults.Spacing` are illustrative; use whatever your actual defaults are.
