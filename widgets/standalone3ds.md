# Standalone 3D Secure

> 
>
> Complete Standalone 3D Secure (3DS) challenges with the Paydock Standalone 3DS Widget. 

The 3DS Widget integrates with the Paydock JS client-sdk within a WebView component. Through this integration, your customer can authenticate the charge using the Standalone 3DS widget, which then communicates with the client-sdk, completes authentication and returns the charge event result.

## iOS

## How to use Standalone 3DS in your iOS application

### 1. Overview

### 2. Parameter definitions

## Android

## How to use the Standalone 3DS Widget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `Standalone3DSWidget` composable in your application. The widget performs verifying payment using 3DS service.

It validates the token to ensure it's valid and of the correct format before rending the UI.

The following sample code demonstrates the definition of the `Standalone3DSWidget`:

```Kotlin
@Composable
fun Standalone3DSWidget(
    token: String,
    completion: (Result<Standalone3DSResult>) -> Unit
) {...}
```

The following sample code example demonstrates the usage within your application:

```Kotlin
// Initialize the Standalone3DSWidget
Standalone3DSWidget(token = threeDSToken) { result ->
    result.onSuccess { threeDSResult ->
         // Handle success - Update UI or perform actions
        Log.d("Standalone3DSWidget", "Payment successful. $threeDSResult")
    }.onFailure { exception ->
        // Handle failure - Show error message or take appropriate action
        Log.e("Standalone3DSWidget", "Payment failed. Error: ${exception.message}")
    }
}
```

### 2. Parameter definitions

This subsection describes the various parameters required by the `Standalone3DSWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `Standalone3DSWidget`.

#### Standalone3DSWidget 

| Name                | Definition                                                                       | Type                                 | Mandatory/Optional |
| :------------------ | :------------------------------------------------------------------------------- | :----------------------------------- | :----------------  |
| token               |  The Standalone 3DS token used for Standalone 3DS widget initialization.                               | String                               | Mandatory          |
| completion          |  Result callback with the Standalone 3DS authentication if successful, or error if not.     | `(Result<Standalone3DSResult>) -> Unit`    | Mandatory          |

#### Standalone3DSResult

This subsection outlines the structure of the result or response object returned by the `Standalone3DSWidget` composable. It details the format and components of the object, enabling you to handle the response effectively within your application.

The following sample code demonstrates the response structure:

```Kotlin
data class Standalone3DSResult(
    val event: StandaloneEventType,
    val charge3dsId: String?
)
```

#### Definition

| Name                | Definition                                                                     | Type                        | 
| :------------------ | :----------------------------------------------------------------------------- | :-------------------------- | 
| event               |  The type of event that occurred during Standalone 3DS processing              | `StandaloneEventType`                 | 
| charge3dsId         |  The Charge ID associated with the 3DS transaction to return to the merchant   | String?                      | 

#### StandaloneEventType

| Name                   | Definition                                                         | 
| :--------------------- | :----------------------------------------------------------------- |
| CHARGE_AUTH_SUCCESS    |  Represents a successful 3DS charge authorization                  |
| CHARGE_AUTH_REJECT     |  Represents a rejected 3DS charge authorization                    |
| CHARGE_AUTH_CHALLENGE  |  Represents a 3DS charge authorization with a challenge            |
| CHARGE_AUTH_DECOUPLED  |  Represents a decoupled 3DS charge authorization                   |
| CHARGE_AUTH_INFO       |  Represents an informational event related to a 3DS charge         |
| CHARGE_ERROR           |  Represents an error event related to a 3DS charge                 |

### 3. Callback Explanation

#### Completion Callback

The `completion` callback is invoked after the Standalone 3DS is completed. It receives a `Result<Standalone3DSResult>` if the payment is authenticated. The callback handles the outcome of the payment operation.

### 4. Error/Exceptions Mapping

The following describes Standalone 3DS exceptions that can be thrown. 

```Kotlin
WebViewException(code: Int?, displayableMessage: String) : Standalone3DSException(displayableMessage)
InvalidTokenException(displayableMessage: String) : Standalone3DSException(displayableMessage)
EventMappingException(displayableMessage: String) : Standalone3DSException(displayableMessage)
```

| Exception               | Description                                                                              | Error Model          |
| :---------------------- | :--------------------------------------------------------------------------------------- | :------------------- |
| WebViewException        |  Exception thrown when there is an error while communicating with a WebView.             |  ThreeDSError        |
| InvalidTokenException   |  Exception thrown when the token is invalid and/or is of the incorrect format/type.      |  ThreeDSError        |
| EventMappingException   |   Exception thrown when there is an issue mapping a web event a SDK expected event.      |  ThreeDSError        |
