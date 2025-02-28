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