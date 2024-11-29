# Gift Card

> 
>
> Securely collect gift card details using a prebuilt form, before transforming them into a One Time Token. You can use this Token with other Paydock API calls, such as Capture Payment.

![Giftcard View](/img/Gift_Card.png) 

## iOS

## How to use the GiftCardView widget in iOS

### 1. Overview

The `GiftCardView` captures gift card details and facilitates the tokenisation of your card within your iOS app. This section provides a step-by-step guide on how to initialize and use the `GiftCardView` in your application.

The following sample code demonstrates how to use the `GiftCardView` composable in your application:


```Swift
GiftCardWidget(
    storePin: Bool = true,
    accessToken: String,
    loadingDelegate: WidgetLoadingDelegate?,
    completion: @escaping (Result<String, Error>) -> Void)
```

The widget returns a token when the gift card is successfully tokenised. If there is an error, the widget returns an **Error** object that includes information about the cause of the failure.

The following is an example of a GiftCardView initialisation:

```Swift
struct GiftCardExampleView: View {
    var body: some View {
        VStack {
            GiftCardWidget(storePin: storePin, 
                           accessToken: "<insert access token>", 
                           loadingDelegate: <Delegate Instance>) { result in
                switch result {
                case .success(let token): // Handle token
                case .failure(let error): // Handle error
                }
            }
        }
    }
}
```

### 2. Parameter Definitions

#### MobileSDK.GiftCardView
| Name            | Definition                                                                                       | Type                              | Mandatory/Optional |
| :-----------    | :----------------------------------------------------------------------------------------------  | :-------------------------------- | :----------------  |
| storePin        |  A flag to be able to use a PIN value for the initial transaction.                               | String (default = true)           | Optional           |
| accessToken     |  The access token used for authentication with the backend service.                              | String                            | Mandatory          |
| loadingDelegate |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown. | WidgetLoadingDelegate             | Optional           |
| completion      |  A completion block that returns result with token or error.                                     | `(Result<String, Error>) -> Void` | Mandatory          |

#### MobileSDK.GiftCardError

| Name                       | Description                                                                    | Error Result            |
| :------------------------ | :------------------------------------------------------------------------------ | :---------------------- |
| errorTokenisingCard       |  Error thrown when provided widget failed gift card tokenisation                |  ErrorRes               |
| unknownError              |  Error thrown when there is an unknown error related to gift card tokenisation  |  nil                    |

## Android

## How to use the GiftCardWidget in Android

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `GiftCardWidget` composable in your application. The widget performs tokenisation of gift card details.

The following sample code demonstrates the definition of the `GiftCardWidget`:

```Kotlin
@Composable
fun GiftCardWidget(
    modifier: Modifier,
    accessToken: String,
    storePin: Boolean,
    completion: (Result<String>) -> Unit
) {...}
```

The following sample code example demonstrates the usage within your application:

```Kotlin
// Initialize the GiftCardWidget
GiftCardWidget(
    modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp), // optional
    accessToken = ACCESS_TOKEN, // required
    storePin: false, // optional (default: true)
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
| accessToken         |  The access token used for authentication with the backend service.                                       | String                      | Mandatory          |
| storePin            |  A flag to be able to use a PIN value for the initial transaction.                                        | String (default = true)     | Optional           |
| completion          |  Result callback with the gift card details tokenisation API response if successful, or error if not.     | `(Result<String>) -> Unit`  | Mandatory          |

### 3. Callback Explanation

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
