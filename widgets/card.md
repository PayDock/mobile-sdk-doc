# Card Tokenisation Widget

> 
>
>Securely collect card details and transform them into a One Time Token. You can use this Token with other Paydock API calls, such as Capture Payment, Save Card to Vault, and so on.

![Creditcard View](/img/Credit_card.png)

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


## Android

## How to use the CardDetailsWidget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `CardDetailsWidget` composable in your application. The widget tokenises card details.

The following sample code demonstrates the definition of the `CardDetailsWidget`:

```Kotlin
fun CardDetailsWidget(
    modifier: Modifier,
    enabled: Boolean,
    config: CardDetailsWidgetConfig,
    loadingDelegate: WidgetLoadingDelegate?,
    completion: (Result<CardResult>) -> Unit
) {...}
```

The following sample code example demonstrates how to use the widget in your application:

```Kotlin
// Initialize the CardDetailsWidget
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
    loadingDelegate = DELEGATE_INSTANCE, // Delegate class to handle loading
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
| config                  |  Configuration options for the card details widget                                                        |`CardDetailsWidgetConfig`       | Required           |
| loadingDelegate         |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown.          | `WidgetLoadingDelegate`        | Optional           |
| completion              |  Result callback with the card details tokenisation API response if successful, or error if not.          | `(Result<CardResult>) -> Unit` | Mandatory          |

#### CardDetailsWidgetConfig

| Name                     | Definition                                                                                       | Type                                               | Mandatory/Optional |
| ------------------------ | ------------------------------------------------------------------------------------------------ | -------------------------------------------------- |------------------  |
| gatewayId               |  Gateway ID used for the card tokenisation.                                                               | String                         | Optional           |
| actionText              |  The text to display on the action button (default is "Submit").                                          | String                         | Optional           |
| showCardTitle           |  A flag indicating whether to show the card title (default is true).                                      | Boolean                        | Optional           |
| collectCardholderName   |  A flag indicating whether to show the cardholder name input (default is true).                           | Boolean                        | Optional           |
| allowSaveCard           |  Configuration for showing the save card UI toggle.                                                       | `SaveCardConfig`               | Optional           |  

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
