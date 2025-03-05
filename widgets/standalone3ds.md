# Standalone 3D Secure

> 
>
> Complete Standalone 3D Secure (3DS) challenges with the Paydock Standalone 3DS Widget. 

The 3DS Widget integrates with the Paydock JS client-sdk within a WebView component. Through this integration, your customer can authenticate the charge using the Standalone 3DS widget, which then communicates with the client-sdk, completes authentication and returns the charge event result.

## iOS

## How to use Standalone 3DS in your iOS application

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `Standalone3DSWidget` view in your application. The widget performs payment verification using 3DS service.

It validates the token to ensure it's valid and of the correct format before rending the UI.

The following sample code demonstrates the definition of the `Standalone3DSWidget`:

```Swift
Standalone3DSWidget(
    token: String,
    baseURL: URL?,
    completion: @escaping (Result<Standalone3DSResult, Standalone3DSError>) -> Void
) {...}
```

The following sample code example demonstrates the usage within your application:

```Swift
Standalone3DSWidget(
    token: <Your 3DS token>,
    baseURL: <Your base URL>,
    completion: { result in
        switch result {
        case .success(let result):
            // handle success
        case .failure(let error):
            // handle failure
        }
})
```

The widget returns an object that contains the status of the 3DS flow and the 3DS token.


### 2. Parameter definitions

#### Standalone3DSWidget 

| Name                | Definition                                                                       | Type                                                           | Mandatory/Optional |
| :------------------ | :------------------------------------------------------------------------------- | :------------------------------------------------------------- | :----------------  |
| token               |  The standalone 3DS token used for standalone 3DS widget initialisation.         | String                                                         | Mandatory          |
| baseURL             |  A URL that is used to resolve relative URLs within the webview.                 | URL                                                            | Optional           |
| completion          |  Result callback with the 3DS authentication if successful, or error if not.     | `(Result<Standalone3DSResult, Standalone3DSError>) -> Void`    | Mandatory          |

#### MobileSDK.Standalone3DSResult

| Name         | Definition                                                                      | Type                        | Mandatory/Optional |
| :----------- | :------------------------------------------------------------------------------ | :-------------------------- | :----------------  |
| event        |  Type of the event that happened in the 3DS flow                                | EventType                   | Mandatory          |
| charge3dsId  |  The Charge ID associated with the 3DS transaction to return to the merchant    | String                      | Mandatory          |

`EventType` enum represents all the possible outcomes of a 3DS flow allowing you to handle it.

#### MobileSDK.EventType

| Name                         | Definition                                                            | Type                          | Mandatory/Optional |
| :--------------------------- | :-------------------------------------------------------------------- | :---------------------------- | :----------------  |
| chargeAuthSuccess            |  Represents a successful 3DS charge authorization                     | EnumCase                      | Mandatory          |
| chargeAuthReject             |  Represents a rejected 3DS charge authorization                       | EnumCase                      | Mandatory          |
| chargeAuthChallenge          |  epresents a 3DS charge authorization with a challenge                | EnumCase                      | Mandatory          |
| chargeAuthDecoupled          |  Represents a decoupled 3DS charge authorization                      | EnumCase                      | Mandatory          |
| chargeAuthInfo               |  Represents an informational event related to a 3DS charge            | EnumCase                      | Mandatory          |
| chargeError                  |  Represents an error event related to a 3DS charge                    | EnumCase                      | Mandatory          |

### 3. Callback Explanation

#### Completion Callback

The `completion` callback is invoked after the Standalone 3DS is completed. It receives a `Result<Standalone3DSResult, Standalone3DSError>` if the payment is authenticated. The callback handles the outcome of the payment operation.

### 4. Error/Exceptions Mapping

The following describes Standalone 3DS exceptions that can be thrown. 

#### MobileSDK.Standalone3DSError

| Name                      | Description                                                                        | Error Result            |
| :------------------------ | :--------------------------------------------------------------------------------- | :---------------------- |
| webViewFailed             |  Error thrown when there is an error while communicating with a WebView.           |  NSError                |
| invalidToken              |  Exception thrown when the token is invalid and/or is of the incorrect format/type |  NSError                |
| mappingFailed             |  Exception thrown when there is an issue mapping a web event a SDK expected event. |  nil                    |

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
