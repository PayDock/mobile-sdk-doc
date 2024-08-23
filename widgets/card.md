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
CardDetailsWidget(gatewayId: String?,
                accessToken: String,
                actionText: String,
                showCardTitle: Bool,
                allowSaveCard: SaveCardConfig?,
                completion: @escaping (Result<CardResult, CardDetailsError>) -> Void)
``` 

The following is an example of a SwiftUI View:

```Swift
struct CardDetailsWidgetView: View {
    @State var isSheetPresented = false
    @State var showAlert = false
    @State var alertMessage = ""

    var body: some View {
        NavigationStack {
            ScrollView {
                HStack {
                    Button("Launch card sheet") {
                        isSheetPresented = true
                    }
                    CardDetailsWidget(
                        gatewayId: "<insert gateway id>", // optional
                        accessToken: "<insert access token>" // mandatory
                        allowSaveCard: SaveCardConfig(consentText: "Remember this card for next time.", privacyPolicyConfig: SaveCardConfig.PrivacyPolicyConfig(privacyPolicyText: "Read our privacy policy", privacyPolicyURL: "https://www.google.com")),
                        completion: { result in
                            switch result {
                            case .success(let token): // Handle token
                            case .failure(let error): // Handle error
                            }
                            showAlert = true
                    })
                }
            }
        }
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

| Name          | Definition                                                                                       | Type                                               | Mandatory/Optional |
| ------------- | ------------------------------------------------------------------------------------------------ | -------------------------------------------------- |------------------  |
| gatewayId     |  Gateway ID that the merchant can input into the widget to allow for tokenisation                | String                                             | Optional           |
| accessToken   |  The access token used for authentication with the backend service.                              | String                                             | Mandatory          |
| actionText    |  Text in the main action button that initiates tokenisation                                      | String                                             | Mandatory          |
| showCardTitle |  Shows or hides internal widget main title label                                                 | Bool                                               | Mandatory          |
| allowSaveCard |  Configures the widget component that allows the user to toggle the switch for desired question  | SaveCardConfig                                     | Optional           |
| completion    |  Completion handler that returns success or failure depending on the widget outcome              | `(Result<CardResult, CardDetailsError>) -> Void)`  | Mandatory          |

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
@Composable
fun CardDetailsWidget(
    modifier: Modifier,
    accessToken: String,
    gatewayId: String?,
    actionText: String,
    showCardTitle: Boolean,
    allowSaveCard: SaveCardConfig?,
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
    accessToken = ACCESS_TOKEN, // required
    gatewayId = GATEWAY_ID, // optional
    allowSaveCard = SaveCardConfig(
        privacyPolicyConfig = SaveCardConfig.PrivacyPolicyConfig(
            privacyPolicyURL = "https://www.privacypolicy.com"
        )
    ),
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
| Card number     | "*Invalid card number*"                                                                             | Exceeds max length (19)                           |
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
| Name                | Definition                                                                                                | Type                           | Mandatory/Optional |
| :------------------ | :-------------------------------------------------------------------------------------------------------- | :----------------------------- | :----------------  |
| modifier            |  Compose modifier for container modifications.                                                            | `Modifier`                     | Optional           |
| accessToken         |  The access token used for authentication with the backend service.                                       | String                         | Mandatory          |
| gatewayId           |  Gateway ID used for the card tokenisation.                                                               | String                         | Optional           |
| actionText          |  The text to display on the action button (default is "Submit").                                          | String                         | Optional           |
| showCardTitle       |  A flag indicating whether to show the card title (default is true).                                      | Boolean                        | Optional           |
| allowSaveCard       |  Configuration for showing the save card UI toggle.                                                       | `SaveCardConfig`               | Optional           |
| completion          |  Result callback with the card details tokenisation API response if successful, or error if not.          | `(Result<CardResult>) -> Unit` | Mandatory          |

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

#### CardResult
| Name                | Definition                                                                                           | Type             | Mandatory/Optional |
| :------------------ | :--------------------------------------------------------------------------------------------------- | :--------------- | :----------------  |
| token               |  The token associated with the card.                                                                 | String           | Mandatory          |
| saveCard            |  A boolean flag indicating whether the card should be saved for future use. (default is false).      | String           | Optional           |

### 3. Callback Explanation

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
