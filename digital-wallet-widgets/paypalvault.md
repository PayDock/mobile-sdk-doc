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
    PayPalSavePaymentSourceWidget(viewState: ViewState?,
                                  config: PayPalVaultConfig,
                                  loadingDelegate: WidgetLoadingDelegate?,
                                  completion: @escaping (Result<PayPalVaultResult, PayPalVaultError>) -> Void)
    {...}
```

The following sample code example demonstrates the usage within your application:

```Swift
let config = PayPalVaultConfig(accessToken: "your_access_token", gatewayId: "your_gateway_id", actionText: nil)
PayPalSavePaymentSourceWidget(viewState: ViewState(state: .disabled),
                              config: config,
                              loadingDelegate = DELEGATE_INSTANCE, // Delegate class to handle loading) 
                              { result in
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

## Android

## How to use the PayPalSavePaymentSourceWidget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `PayPalSavePaymentSourceWidget` composable in your application. The widget facilitates the card linking process using PayPal services.

> **Note**:
>
> When the customer taps the link button for the PayPal Vault flow, the SDK handles the gateway communication between Paydock and PayPal before initialising the PayPal client web flow.

The following sample code demonstrates the definition of the `PayPalSavePaymentSourceWidget`:

```Kotlin
fun PayPalSavePaymentSourceWidget(
    modifier: Modifier = Modifier,
    enabled: Boolean,
    config: PayPalVaultConfig,
    loadingDelegate: WidgetLoadingDelegate?,
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
        loadingDelegate = DELEGATE_INSTANCE, // Delegate class to handle loading
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
| loadingDelegate       |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown.  | `WidgetLoadingDelegate`                           | Optional           |
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
> MissingParamsException: inidicates missing either the `client_id` and/or `setup_token` before initialising PayPal SDK. This is an edge case and is only used as a pre-caution.

| Exception                         | Description                                                                                   | Error Model            |
| :-------------------------------- | :-------------------------------------------------------------------------------------------- | :--------------------- |
| CreateSetupTokenException         |  Exception thrown when there is an error creating a setup token.                              |  PayPalVaultError      |
| GetPayPalClientIdException        |  Exception thrown when there is an error retrieving the PayPal client ID.                     |  PayPalVaultError      |
| CreatePaymentTokenException       |  Exception thrown when there is an error creating a payment token.                            |  PayPalVaultError      |
| PayPalSDKException                |  Exception thrown during PayPal SDK operations.                                               |  PayPalVaultError      |
| MissingParamsException            |  Exception thrown before initialising PayPal SDK if missing params.                           |  PayPalVaultError      |
| CancellationException             |  Exception thrown when cancelling PayPal vault flow.                                          |  PayPalVaultError      |
| UnknownException                  |  Exception thrown when an unknown error occurs in PayPal Vault operations.                    |  PayPalVaultError      |
