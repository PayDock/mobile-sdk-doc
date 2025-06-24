# Coles Pay Widget

The Coles Pay Widget integrates with Coles Pay using a WebView component. This handles the communication between Coles Pay and the SDK. Once completed, the WebView component returns the `ColesPayOrderId`.

![Coles Pay View](/img/Colespay.png)

## iOS

## How to use the ColesPayWidget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `ColesPayWidget` SwiftUI view in your application. The widget facilitates the payment using Coles Pay services.

The following sample code demonstrates the definition of the `ColesPayWidget`:

```Swift
ColesPayWidget(
    clientId: String,
    colesPayToken: @escaping (_ colesPayToken: @escaping (String) -> Void) -> Void,
    completion: @escaping (Result<String, ColesPayError>) -> Void))
```

The following sample code demonstrates how you can use the widget in your application:

```Swift
struct ColesPayExampleView: View {
    @StateObject private var viewModel = ColesPayExampleVM()
    var body: some View {
        ColesPayWidget(clientId: <merchantClientId>) { onColesPayButtonTap in
            // Example: Initiate Wallet Transaction to retrieve the wallet token
            viewModel.initializeWalletCharge(completion: onColesPayButtonTap)
        } completion: { result in
            switch result {
            case .success(let colesPayOrderId): // Handle success
            case .failure(let error): // Handle error
            }
        }
    }
}
```

### 2. Parameter definitions

This subsection describes the parameters required by the `ColesPayWidget` SwiftUI view. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `ColesPayWidget`.

#### ColesPayWidget

| Name                  | Definition                                                                        | Type                                                                 | Mandatory/Optional |
| :-------------------- | :-------------------------------------------------------------------------------- | :------------------------------------------------------------------- | :----------------- |
| clientId              |  A merchant supplied client ID                                                    | String                                                               | Mandatory          |
| colesPayToken         |  A callback to obtain the wallet token asynchronously                             | `@escaping (_ colesPayToken: @escaping (String) -> Void) -> Void`    | Mandatory          |
| completion            |  Result callback with the *ColesPayOrderId* if successful, or error if not.       | `(Result<String, ColesPayError>) -> Void`                              | Mandatory          |

### MobileSDK.ColesPayError

| Name                        | Description                                                            | Error Result            |
| :-------------------------- | :--------------------------------------------------------------------- | :---------------------- |
| errorFetchingColesPayOrder  |  Error thrown when fetching ColesPay order ID fails.                   |  ErrorRes               |
| colesPayUrlError            |  Error thrown when generating the ColesPay URL.                        |  nil                    |
| webViewFailed               |  Error thrown when there is an issue communicating with the WebView.   |  NSError                |
| transactionCanceled         |  Error thrown when user cancel the flow.                               |  nil                    |
| unknownError                |  Error thrown when there is an unknown error related to ColesPay.      |  nil                    |

### 3. Callback Explanation

#### Token Callback

> **Note**:
>
> The `colesPayToken` callback obtains the wallet token asynchronously. It receives a callback function `@escaping (_ colesPayToken: @escaping (String) -> Void) -> Void` as a parameter, which you must invoke with the `wallet_token` once it is obtained. To obtain the `wallet_token`, follow the instructions in the [generate a wallet_token](/digital-wallet-widgets/wallettoken.md) section of this guide.  

#### Completion Callback

The `completion` callback is invoked after the payment operation is completed. It receives a `Result<String, ColesPayError>` that contains the *ColesPayOrderId* if the payment is successful. The callback handles the outcome of the payment operation.


## Android

## How to use the ColesPayWidget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `ColesPayWidget` composable in your application. The widget facilitates the payment using Coles Pay services.

The following sample code demonstrates the definition of the `ColesPayWidget`:

```Kotlin
@Composable
fun ColesPayWidget(
    modifier: Modifier = Modifier,
    enabled: Boolean = true,
    config: ColesPayWidgetConfig,
    appearance: ColesPayWidgetAppearance = ColesPayWidgetAppearanceDefaults.appearance(),
    tokenRequest: (tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit,
    loadingDelegate: WidgetLoadingDelegate? = null,
    completion: (Result<String>) -> Unit
) {...}
```

The following sample code demonstrates how you can use the widget in your application:

```Kotlin
// Initialize the ColesPayWidget
ColesPayWidget(
    modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp), // optional
    config = ColesPayWidgetConfig(clientId = merchantClientId),
    appearance = currentOrDefaultAppearance,
    tokenRequest = { callback ->
        // show widget loader
        tokenResult.onSuccess { result ->
            // Handle token success
            val walletToken = result.token // ... retrieve the token
            Result.success(WalletTokenResult(token = walletToken))
        }.onFailure {
            // Handle token failure success
            Result.failure(it)
        }
    }
) { result ->
    // Handle the result of the payment operation
    result.onSuccess { colesPayOrderId ->
        // Handle success - Update UI or perform actions
        Log.d("ColesPayWidget", "Payment successful. Order ID: $colesPayOrderId")
    }.onFailure { exception ->
        // Handle failure - Show error message or take appropriate action
        Log.e("ColesPayWidget", "Payment failed. Error: ${exception.message}")
    }
}
```

### 2. Parameter definitions

This subsection describes the various parameters required by the `ColesPayWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `ColesPayWidget`.

#### ColesPayWidget

| Name                  | Definition                                                                                        | Type                                                                           | Mandatory/Optional |
| :-------------------- | :------------------------------------------------------------------------------------------------ | :----------------------------------------------------------------------------- | :----------------- |
| modifier              |  Compose modifier for container modifications                                                     | `androidx.compose.ui.Modifier`                                                 | Optional           |
| enabled               |  A boolean to enable or disable the payment button                                                | Boolean                                                                        | Optional           |
| config                |  The configuration details required for the Coles Pay widget                                      | `ColesPayWidgetConfig`                                                         | Mandatory          |
| appearance            |  Customization options for the visual appearance of the widget                                    | `ColesPayWidgetAppearance`                                                     | Optional           |
| tokenRequest          |  A callback to obtain the wallet token result asynchronously                                      | `tokenRequest: (tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit`     | Mandatory          |
| loadingDelegate       |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown.  | `WidgetLoadingDelegate`                                                        | Optional           |
| completion            |  Result callback with the *ColesPayOrderId* if successful, or error if not.                       | `(Result<ChargeResponse>) -> Unit`                                             | Mandatory          |

#### ColesPayWidgetConfig

| Name                  | Definition                                         | Type                        | Mandatory/Optional |
| --------------------- | -------------------------------------------------- | --------------------------- |------------------  |
| clientId              |  A merchant supplied client ID                     | String                      | Mandatory          |

### 3. Callback Explanation

#### Token Callback

The `token` callback obtains the wallet token result asynchronously. It receives a callback function `tokenRequest: (tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit` as a parameter, which you must invoke with the result of the wallet token API request once it is obtained. 

The `WalletTokenResult` acts as a token wrapper containing the actual token result.

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

The `completion` callback is invoked after the payment operation is completed. It receives a `Result<String>` where String represents the *ColesPayOrderId* if the payment is successful. The callback handles the outcome of the payment operation. To obtain the `wallet_token`, follow the instructions in the [generate a wallet_token](/digital-wallet-widgets/wallettoken.md) section of this guide.  

### 4. Error/Exceptions Mapping

The following describes Coles Pay exceptions that can be thrown. 

```Kotlin
FetchingUrlException(error: ApiErrorResponse) : ColesPayException(error.displayableMessage)
WebViewException(code: Int?, displayableMessage: String) : ColesPayException(displayableMessage)
CancellationException(displayableMessage: String) : ColesPayException(displayableMessage)
ParseException(displayableMessage: String, val errorBody: String?) : ColesPayException(displayableMessage)
InitialisationWalletTokenException(displayableMessage: String, val errorBody: String?) : ColesPayException(displayableMessage)
UnknownException(displayableMessage: String) : ColesPayException(displayableMessage)
```

| Exception                             | Description                                                                                   | Error Model         |
| :------------------------------------ | :-------------------------------------------------------------------------------------------- | :------------------ |
| FetchingUrlException                  |  Exception thrown when there is an error fetching the URL for Coles Pay.                      |  ColesPayError      |
| WebViewException                      |  Exception thrown when there is an error while communicating with a WebView.                  |  ColesPayError      |
| CancellationException                 |  Exception thrown when there is a cancellation error related to Coles Pay.                    |  ColesPayError      |
| ParseException                        |  Exception thrown when parsing of data from an API call, typically JSON.                      |  ColesPayError      |
| InitialisationWalletTokenException    |  Exception thrown when the wallet token result returns a failure result.                      |  ColesPayError      |
| UnknownException                      |  Exception thrown when there is an unknown error related to Coles Pay.                        |  ColesPayError      |


### 5. Widget Styling

Defines the visual appearance for the `ColesPayWidget`. This primarily involves customizing the "Pay with Coles Pay" image button and the loading indicator displayed during its operation.

#### Appearance Contract

The `ColesPayWidgetAppearance` class encapsulates the configurable style properties for the widget.

```Kotlin
@Immutable
class ColesPayWidgetAppearance(
    val imageButton: ImageButtonAppearance,
    val loader: LoaderAppearance
) 
```

#### Default Appearance & Customisation

A default appearance is provided by `ColesPayWidgetAppearanceDefaults`. This configures the image button with a specific pill shape and white ripple, and the loader with a white color, designed to match typical Coles Pay branding.

##### Using Default Appearance


```Kotlin
    ColesPayWidget( 
        ...
        appearance = ColesPayWidgetAppearanceDefaults.appearance() // Uses the default appearance
    )
```

##### Customising Appearance

You can create a custom `ColesPayWidgetAppearance` by providing specific `ImageButtonAppearance` and `LoaderAppearance` configurations.

```Kotlin
@Composable 
fun MyCustomColesPayScreen() { 
    // Create appearance by using provided defaults, with custom changes
    val customAppearance = defaultFromContext.copy( // Assuming .copy() exists as per context
        imageButton = ImageButtonDefaults.appearance().copy(
            // Example: Customizing ripple colour
            // rippleColor = Colors.Blue
        ),
        loader = LoaderAppearanceDefaults.appearance().copy(
            // Example: Customizing loader colour
            // color = Color.Red
        )
    )

    // Alternatively, create entirely from scratch:
    val completelyCustomAppearance = ColesPayWidgetAppearance(
        imageButton = ImageButtonAppearance( // Assuming constructor or defaults exist
            shape = RoundedCornerShape(percent = 20),
            rippleColor = Color.DarkGray,
            // ... other ImageButtonAppearance properties
        ),
        loader = LoaderAppearance( // Assuming constructor or defaults exist
            color = Color.Red,
            strokeWidth = 5.dp
            // ... other LoaderAppearance properties
        )
    )

    ColesPayWidget(
        ...
        appearance = customAppearanceFromDefaults, // Use your custom appearance
    )
}
```

#### Style Attributes

The following attributes can be configured within `ColesPayWidgetAppearance`:

 Name                | Description                                                                                              | Type                               | Default Value (from `ColesPayWidgetAppearanceDefaults`)   |
---------------------|----------------------------------------------------------------------------------------------------------|------------------------------------|-----------------------------------------------------------|
 `imageButton`       | Defines the appearance for fixed image button, such as interactive state.                                | `ImageButtonAppearance`            | `ColesPayWidgetAppearanceDefaults.appearance()`           |
 `loader`            | Defines the appearance of the loading indicator shown when the widget is processing or loading content.  | `LoaderAppearance`                 | `LoaderAppearanceDefaults.appearance()`                   |

---

**Note:**
*   The `ImageButtonAppearance` and `LoaderAppearance` themselves would have their own detailed documentation explaining their configurable attributes (like colors, shapes, typography if applicable, stroke width, etc.).*   