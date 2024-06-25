---
title: Error Handling
---

## Overview

Errors in the SDK are tailored for each widget. Each widget contains its own set of Errors/Exceptions for different errors that may occur while using the widget.

## iOS

### 1. Overview

For iOS all the errors received from the widgets are based on that Widget error model, which conforms to the generic Error protocol.

This document describes the error handling mechanism for the widgets. Each operation in the widget returns a result in the form of Result<SuccessModel, ErrorModel>, where SuccessModel indicates a successful operation and the ErrorModel encapsulates any errors that might occur.

>
>
>Each widget based error can be found on its respective pages.


### 2. Error definitions

The iOS SDK uses Error as its base error type as it is used in conjunction with the `Result<SuccessModel, ErrorModel>`. Some of the error cases can contain an `ErrorRes` object within that is received from the backend with further descriptions of the error.

Each error case includes a `customMessage` property that provides a user-friendly description of the error. You can use this property to display error messages to the user or for logging purposes.

The following is a general error model that is received from the backend for all API related errors:

#### ErrorRes

| Property        | Type          | Description                                                |
| :-------------- | :------------ | :--------------------------------------------------------- |
| status          | Int           | The HTTP status code of the response.                      |
| error           | ErrorObj?     | An object containing the error message and code.           |
| resource        | Resource?     | An object containing the type of the resource involved.    |
| errorSummary    | ErrorSummary? | An object summarizing the error details.                   |

### ErrorObj

| Property | Type    | Description                 |
| :------- | :------ | :-------------------------- |
| message  | String? | The error message.          |
| code     | String? | The error code.             |

### Resource

| Property | Type    | Description                 |
| :------- | :------ | :-------------------------- |
| type     | String? | The type of the resource.   |

#### ErrorSummary

| Property                | Type           | Description                                     |
| :---------------------- | :------------- | :---------------------------------------------- |
| message                 | String?        | A summary message of the error.                 |
| code                    | String?        | A summary code of the error.                    |
| statusCode              | String?        | The HTTP status code as a string.               |
| statusCodeDescription   | String?        | A description of the HTTP status code.          |
| details                 | ErrorDetails?  | An object containing detailed information about the error. |

#### ErrorDetails

| Property                       | Type      | Description                                              |
| :----------------------------- | :-------- | :------------------------------------------------------- |
| gatewaySpecificCode            | String?   | A code specific to the gateway indicating the error.     |
| gatewaySpecificDescription     | String?   | A description specific to the gateway indicating the error. |
| messages                       | [String]? | A list of additional error messages.                     |


## Android

### 1. Overview

Alongside the widget based errors/exceptions, there are generic errors that can be thrown such as connection errors, serialization exceptions and so on.

> 
>
>Each widget based error/exception can be found on its respective page.

### 2. Exception definitions

> 
>
>The Android SDK uses exceptions as its base error type as these are used in conjunction with the `kotlin.Result<T>`.

#### Exception Utils

#### Generic Exception Mapping

Generic exceptions that can be thrown are as follows:

| Exception               | Description                                                                                                                                                      | Error Model                     |
| :---------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------ |
| SocketTimeoutException  |  Signals that a timeout has occurred on a socket read or accept.                                                                                                 |  ConnectionError.Timeout        |
| IOException             |  Signals that an I/O exception of some sort has occurred. This class is the general class of exceptions produced by failed or interrupted I/O operations.        |  ConnectionError.IOError        |
| UnknownHostException    |  Thrown to indicate that the IP address of a host could not be determined.                                                                                       |  ConnectionError.UnknownHost    |
| SerializationException  |  A generic exception indicating the problem in serialization or deserialization process.                                                                         |  ErrorModel.SerializationError  |
| [Catch All]             |  A generic catch all mapping for any unknown exception not catered for specifically                                                                              |  ErrorModel.UnknownError        |

The Mobile SDK provides some utility helpers to map the result exception to an `ErrorModel`, enabling easy mapping to display the exception message based on the type.

```Kotlin
// Util function to map exception to ErrorModel
fun Throwable.toError(): ErrorModel

// Util property to retrieve exception message
val ErrorModel?.displayableMessage: String

// Usage
.onFailure {
    val error = it.toError()
    Log.d("[WidgetFailure]", "Failure: ${error.displayableMessage}")
}
```

#### Exception Handling Options

The following breaks down 3 possible ways to retrieve and handle the exception result.

#### Option 1

This approach uses Kotlin's result handling capabilities to process success and failure cases. 

It leverages onSuccess and onFailure extension functions on the result. In the event of a failure, it checks if the exception is of type WidgetException and logs a custom error message based on the 
specific WidgetException subtype. Generic exceptions are also handled and converted to a custom error 
message for logging.

```Kotlin
// Option 1: Default exception handling
completion = { result ->
    result.onSuccess {
        ...
    }.onFailure { exception: Throwable ->
        if (exception is [WidgetException]) {
            val errorMessage = when (exception) {
                is [WidgetException].ApiException -> exception.error.displayableMessage
                is [WidgetException].UnknownException -> exception.message
            }
            Log.d("[WidgetFailure]", "Failure: $errorMessage")
        } else {
            // Handle generic exceptions
            val error = exception.toError()
            Log.d("[WidgetFailure]", "Failure: ${error.displayableMessage}")
        }
    }
}
```

##### Option 2
In this method, a try block is used to directly obtain the result using the `getOrThrow` extension function, which can 
throw specific exceptions. If a WidgetException is thrown, it checks the specific type and handles it accordingly. 

Additionally, a generic catch block handles any other exceptions, converting them to custom error messages for logging purposes.

```Kotlin
// Option 2: Use try catch block with getOrThrow extension function
completion = { result ->
    try {
        val widgetResult: [Type] = result.getOrThrow([WidgetException]::class)
    } catch (exception: [WidgetException]) {
        when (exception) {
            is [WidgetException].ApiException -> TODO()
            is [WidgetException].UnknownException -> TODO()
        }
    } catch (e: Exception) {
        // Catch generic exceptions
        val error = e.toError()
        Log.d("[WidgetFailure]", "Failure: ${error.displayableMessage}")
    }
}
```

#### Option 3

This method processes the result using the `onSuccess` and a custom `onFailure` extension function, similar to Option 1, 
but with a focus on handling a specific exception type, WidgetException. If such an exception occurs, it checks the specific subtype and provides appropriate handling. Additionally, it uses `recoverCatching` to 
handle and log generic exceptions that may not be related to WidgetException.


```Kotlin
// Option 3: Use onFailure extension function to specify Exception
completion = { result ->
    result.onSuccess {
        ...
    }.onFailure([WidgetException]::class) { exception: [WidgetException] ->
            when (exception) {
                is [WidgetException].ApiException -> TODO()
                is [WidgetException].UnknownException -> TODO()
            }
    }.recoverCatching {
        // Catch generic exceptions
        val error = it.toError()
        Log.d("[WidgetFailure]", "Failure: ${error.displayableMessage}")
    }
}
```