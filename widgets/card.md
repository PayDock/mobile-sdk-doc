---
title: Card Tokenisation Widget
description: The Card Tokenisation Widget's prebuilt card form securely collects card details and transforms them into a One Time Token. You can use this Token with other Paydock API calls, such as Capture Payment, Save Card to Vault, and so on.
---

# {% $markdoc.frontmatter.title %}

{% $markdoc.frontmatter.description %}

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
| Name         | Definition                                                                                     | Type             | Mandatory/Optional |
| :----------- | :--------------------------------------------------------------------------------------------- | :--------------- | :----------------  |
| isPresented  |  Binding that is used for control if the sheet is presented on the screen                      | Binding<Bool>    | Mandatory          |
| gatewayId    |  Gateway ID that the merchant needs to input into the sheet to allow for tokenisation          | Swift.String     | Mandatory          |
| onCompletion |  Returns the card token to the app once the user is finished with inputing info into the sheet | Binding<String>  | Mandatory          |
| onFailure    |  Returns the error message to the app in case the tokenisation process fails                   | Binding<String>  | Mandatory          |

## Android

## How to use the CardDetailsWidget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `CardDetailsWidget` composable in your application. The widget performs tokenisation of card details.

The following sample code demonstrates the definition of the `CardDetailsWidget`:

```Kotlin
@Composable
fun CardDetailsWidget(
    modifier: Modifier,
    gatewayId: String?,
    completion: (Result<String>) -> Unit
) {...}
```

The following sample code example demonstrates the usage within your application:

```Kotlin
// Initialize the CardDetailsWidget
CardDetailsWidget(
    modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp), // optional
    gatewayId = GATEWAY_ID, // optional
    completion = { result ->
        result.onSuccess { token ->
            // Handle success - Update UI or perform actions
            Log.d("CardDetailsWidget", "Tokenisation successful. Card token: $token")
        }.onFailure { throwable ->
            // Handle failure - Show error message or take appropriate action
            Log.e("CardDetailsWidget", "Tokenisation failed. Error: ${exception.message}")
        }
    }
)
```

### 2. Parameter definitions

This subsection describes the various parameters required by the `CardDetailsWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `CardDetailsWidget`.

#### CardDetailsWidget
| Name                | Definition                                                                                                | Type                        | Mandatory/Optional |
| :------------------ | :-------------------------------------------------------------------------------------------------------- | :-------------------------- | :----------------  |
| modifier            |  Compose modifier for container modifications                                                             | `Modifier`                  | Optional           |
| gatewayId           |  Gateway ID used for the card tokenisation                                                                | String                      | Optional           |
| completion          |  Result callback with the card details tokenisation API response if successful, or error if not.          | `(Result<String>) -> Unit`  | Mandatory          |

### 3. Callback Explanation

#### Completion Callback

The `completion` callback is invoked after the card tokenisation operation is completed. It receives a `Result<String>` if the card details was tokenised successfully.
