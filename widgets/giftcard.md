# Gift Card Tokenisation Widget

The Gift Card Tokenisation Widget securely collects gift card details using a prebuilt form, before transforming them into a One Time Token. You can use this Token with other Paydock API calls, such as Capture Payment.

## iOS

### How to use the GiftCardView widget in iOS

The `GiftCardView` captures gift card details and facilitates the tokenisation of your card within your iOS app. This section provides a step-by-step guide on how to initialize and use the `GiftCardView` in your application.

The following sample code demonstrates how to use the `GiftCardView` composable in your application:

```Swift
GiftCardWidget(
    storePin: Bool = true,
    completion: @escaping (Result<String, Error>) -> Void)
```

The widget returns a token when the gift card is successfully tokenised. If there is an error, the widget returns an **Error** object that includes information about the cause of the failure.

The following is an example of a GiftCardView initialisation:

```Swift
struct GiftCardExampleView: View {
    var body: some View {
        VStack {
            GiftCardWidget(storePin: storePin) { result in
                switch result {
                case .success(let token): // Handle token
                case .failure(let error): // Handle error
                }
            }
        }
    }
}
```

### Definitions

#### MobileSDK.GiftCardView

| Name       | Definition                                                        | Type                            | Mandatory/Optional |
| :--------- | :---------------------------------------------------------------- | :------------------------------ | :----------------- |
| storePin   | A flag to be able to use a PIN value for the initial transaction. | String (default = true)         | Optional           |
| completion | A completion block that returns result with token or error.       | (Result<String, Error>) -> Void | Mandatory          |

## Android

### How to use the GiftCardWidget

This section provides a step-by-step guide on how to initialize and use the `GiftCardWidget` composable in your application. The widget performs tokenisation of gift card details.

The following sample code demonstrates the definition of the `GiftCardWidget`:

```Kotlin
@Composable
fun GiftCardWidget(
    modifier: Modifier,
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

### Definitions

This subsection describes the various parameters required by the `GiftCardWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `GiftCardWidget`.

#### GiftCardWidget

| Name       | Definition                                                                                           | Type                       | Mandatory/Optional |
| :--------- | :--------------------------------------------------------------------------------------------------- | :------------------------- | :----------------- |
| modifier   | Compose modifier for container modifications                                                         | `Modifier`                 | Optional           |
| storePin   | A flag to be able to use a PIN value for the initial transaction.                                    | String (default = true)    | Optional           |
| completion | Result callback with the gift card details tokenisation API response if successful, or error if not. | `(Result<String>) -> Unit` | Mandatory          |

### Callback Explanation

#### Completion Callback

The `completion` callback is invoked after the gift card tokenisation operation is completed. It receives a `Result<String>` if the gift card details was tokenised successfully.
