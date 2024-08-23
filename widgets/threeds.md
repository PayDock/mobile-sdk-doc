# 3D Secure

> 
>
> Complete 3D Secure (3DS) challenges with the Paydock 3DS Widget. 

The 3DS Widget integrates with the Paydock JS client-sdk within a WebView component. Through this integration, your customer can authenticate the charge using the 3DS widget, which then communicates with the client-sdk, completes authentication and returns the charge event result. This also caters for both the Integrated 3DS and the Standalone 3DS flow.

## iOS

## How to use 3DS in your iOS application

### 1. Overview

The `ThreeDSWidget` validates and authenticates the 3DS charge using the created 3DS token from your iOS app. This section provides a step-by-step guide on how to initialize and use the `ThreeDSWidget` composable in your application.

The following sample code demonstrates the `ThreeDSWidget` composable definition, as well as how to use it in your application:

```Swift
ThreeDSWidget(
    token: String,
    baseURL: URL?,
    completionHandler: (Result<ThreeDSResult, ThreeDSError>) -> Void
```

The widget returns an object that contains the status of the 3DS flow and the 3DS token.

### 2. Parameter definitions

#### ThreeDSWidget 

| Name                | Definition                                                                       | Type                                               | Mandatory/Optional |
| :------------------ | :------------------------------------------------------------------------------- | :------------------------------------------------- | :----------------  |
| token               |  The 3DS token used for 3DS widget initialisation.                               | String                                             | Mandatory          |
| baseURL             |  A URL that is used to resolve relative URLs within the webview.                 | URL                                                | Optional           |
| completion          |  Result callback with the 3DS authentication if successful, or error if not.     | `(Result<ThreeDSResult, ThreeDSError>) -> Void`    | Mandatory          |

#### MobileSDK.ThreeDSResult
| Name         | Definition                                                                      | Type                        | Mandatory/Optional |
| :----------- | :------------------------------------------------------------------------------ | :-------------------------- | :----------------  |
| event        |  Type of the event that happened in the 3DS flow                                | EventType                   | Mandatory          |
| charge3dsId  |  The Charge ID associated with the 3DS transaction to return to the merchant    | String                      | Mandatory          |

`EventType` enum represents all the possible outcomes of a 3DS flow allowing you to handle it.

#### MobileSDK.EventType
| Name                | Definition                                                         | Type                        | Mandatory/Optional |
| :------------------ | :----------------------------------------------------------------- | :---------------------------- | :----------------  |
| chargeAuthSuccess   |  Represents a successful 3DS charge authorization                  | EnumCase                      | Mandatory          |
| chargeAuthReject    |  Represents a rejected 3DS charge authorization                    | EnumCase                      | Mandatory          |
| chargeAuthChallenge |  Represents a 3DS charge authorization with a challenge            | EnumCase                      | Mandatory          |
| chargeAuthDecoupled |  Represents a decoupled 3DS charge authorization                   | EnumCase                      | Mandatory          |
| chargeAuthInfo      |  Represents an informational event related to a 3DS charge         | EnumCase                      | Mandatory          |
| error               |  Represents an unknown or unrecognized event type                  | EnumCase                      | Mandatory          |

#### MobileSDK.ThreeDSError

| Name                       | Description                                                                     | Error Result            |
| :------------------------ | :------------------------------------------------------------------------------- | :---------------------- |
| webViewFailed             |  Error thrown when there is an error while communicating with a WebView.         |  NSError                |
| UnknownException          |  Error thrown when there is an unknown error related to 3DS.                     |  nil                    |


## Android

## How to use the 3DS Widget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `ThreeDSWidget` composable in your application. The widget performs verifying payment using 3DS service.

The following sample code demonstrates the definition of the `ThreeDSWidget`:

```Kotlin
@Composable
fun ThreeDSWidget(
    token: String,
    completion: (Result<ThreeDSResult>) -> Unit
) {...}
```

The following sample code example demonstrates the usage within your application:

```Kotlin
// Initialize the GiftCardWidget
ThreeDSWidget(token = threeDSToken) { result ->
    result.onSuccess { threeDSResult ->
         // Handle success - Update UI or perform actions
        Log.d("ThreeDSWidget", "Payment successful. $threeDSResult")
    }.onFailure { exception ->
        // Handle failure - Show error message or take appropriate action
        Log.e("ThreeDSWidget", "Payment failed. Error: ${exception.message}")
    }
}
```

### 2. Parameter definitions

This subsection describes the various parameters required by the `ThreeDSWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `ThreeDSWidget`.

#### ThreeDSWidget 

| Name                | Definition                                                                       | Type                                 | Mandatory/Optional |
| :------------------ | :------------------------------------------------------------------------------- | :----------------------------------- | :----------------  |
| token               |  The 3DS token used for 3DS widget initialization.                               | String                               | Mandatory          |
| completion          |  Result callback with the 3DS authentication if successful, or error if not.     | `(Result<ThreeDSResult>) -> Unit`    | Mandatory          |

#### ThreeDSResult

This subsection outlines the structure of the result or response object returned by the `ThreeDSWidget` composable. It details the format and components of the object, enabling you to handle the response effectively within your application.

The following sample code demonstrates the response structure:

```Kotlin
data class ThreeDSResult(
    val event: EventType,
    val charge3dsId: String
)
```

#### Definition

| Name                | Definition                                                                     | Type                        | 
| :------------------ | :----------------------------------------------------------------------------- | :-------------------------- | 
| event               |  The type of event that occurred during 3DS processing                         | `EventType`                 | 
| charge3dsId         |  The Charge ID associated with the 3DS transaction to return to the merchant   | String                      | 

#### EventType

| Name                   | Definition                                                         | 
| :--------------------- | :----------------------------------------------------------------- |
| CHARGE_AUTH_SUCCESS    |  Represents a successful 3DS charge authorization                  |
| CHARGE_AUTH_REJECT     |  Represents a rejected 3DS charge authorization                    |
| CHARGE_AUTH_CHALLENGE  |  Represents a 3DS charge authorization with a challenge            |
| CHARGE_AUTH_DECOUPLED  |  Represents a decoupled 3DS charge authorization                   |
| CHARGE_AUTH_INFO       |  Represents an informational event related to a 3DS charge         |
| CHARGE_ERROR           |  Represents an error event related to a 3DS charge                 |
| UNKNOWN                |  Represents an unknown or unrecognised event type (default)        |

### 3. Callback Explanation

#### Completion Callback

The `completion` callback is invoked after the 3DS is completed. It receives a `Result<ThreeDSResult>` if the payment is authenticated. The callback handles the outcome of the payment operation.

### 4. Error/Exceptions Mapping

The following describes 3DS exceptions that can be thrown. 

```Kotlin
ChargeErrorException(displayableMessage: String) : ThreeDSException(displayableMessage)
WebViewException(code: Int?, displayableMessage: String) : ThreeDSException(displayableMessage)
CancellationException(displayableMessage: String) : ThreeDSException(displayableMessage)
```

| Exception               | Description                                                                              | Error Model          |
| :---------------------- | :--------------------------------------------------------------------------------------- | :------------------- |
| ChargeErrorException    |  Exception thrown when there is an error during the charge process for 3D Secure.        |  ThreeDSError        |
| WebViewException        |  Exception thrown when there is an error while communicating with a WebView.             |  ThreeDSError        |
| CancellationException   |  Exception thrown when there is a cancellation error related to 3D Secure.               |  ThreeDSError        |

