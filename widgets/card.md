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
CardDetailsSheetView(isPresented: Binding<Bool>,
                     gatewayId: String,
                     onCompletion: Binding<String>,
                     onFailure: Binding<String>)
``` 

The following is an example of a SwiftUI View:

```Swift
struct CardDetailsWidgetView: View {
    @State var cardToken: String = ""
    @State var isSheetPresented = false
    @State var error: String = ""

    var body: some View {
        NavigationStack {
            ScrollView {
                HStack {
                    Button("Launch card sheet") {
                        isSheetPresented = true
                    }
                    CardDetailsSheetView(
                        isPresented: $isSheetPresented,
                        gatewayId: "<insert gateway id>",
                        onCompletion: $cardToken,
                        onFailure: $error)
                }
            }
        }
    }
}
```

### 2. Parameter definitions

#### MobileSDK.CardDetailsSheetView

| Name          | Definition                         | Type | Mandatory/Optional                           |
| ------------- | ---------------------------------- | ------|-------------------------------------------  |
 isPresented  |  Binding that is used for control if the sheet is presented on the screen |   | Mandatory          |
| gatewayId    |  Gateway ID that the merchant needs to input into the sheet to allow for tokenisation          | Swift.String     | Mandatory          |
| onCompletion |  Returns the card token to the app once the user is finished with inputing info into the sheet |   | Mandatory          |
| onFailure    |  Returns the error message to the app in case the tokenisation process fails                   |   | Mandatory          |

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
