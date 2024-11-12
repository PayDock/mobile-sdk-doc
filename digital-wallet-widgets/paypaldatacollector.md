# PayPal Data Collector

The MobileSDK provides a PayPal Data Collector Util that integrates with the PayPal Fraud SDK. This handles the communication between PayPal and the SDK to collect device info. Once initialised, the util can collect the `clientMetadataId` from the MagnesSDK.

> **Note**:
>
> DataCollector enables you to collect data about a customer's device and correlate it with a session identifier on your server.


## iOS

## Android

## How to use the PayPalDataCollector

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `PayPalDataCollectorUtil` util in your application.

The following sample code demonstrates the definition of the `PayPalDataCollectorUtil`:

```Kotlin
class PayPalDataCollectorUtil private constructor(
    val config: PayPalDataCollectorConfig
) {
    companion object {
        @Throws(PayPalDataCollectorException::class)
        suspend fun initialise(
            config: PayPalDataCollectorConfig,
        ): PayPalDataCollectorUtil {
            ...
            // returns instance if successful
            // else throws exception
        }
    }
}
```

The following sample code example demonstrates the usage within your application:

```Kotlin
var payPalDataCollectorUtil: PayPalDataCollectorUtil? by remember { mutableStateOf(null) }
LaunchedEffect(Unit) {
    try {
        payPalDataCollectorUtil = PayPalDataCollectorUtil.initialise(
            PayPalDataCollectorConfig(
                accessToken = [access_token],
                gatewayId = [pay_pal_gateway_id]
            )
        )
    } catch (e: PayPalDataCollectorException) {
        // handle failure
    }
}
val result = payPalDataCollectorUtil?.collectDeviceInfo(context)
```

### 2. Parameter Definitions

This subsection describes the various parameters required by the `PayPalDataCollectorUtil` util. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `PayPalDataCollectorUtil`.

#### PayPalSavePaymentSourceWidget

| Name                  | Definition                                                                                     | Type                                              | Mandatory/Optional |
| :-------------------- | :--------------------------------------------------------------------------------------------- | :------------------------------------------------ | :----------------- |
| config                |  The configuration for PayPal Data Collector util.                                             | `PayPalDataCollectorConfig`                       | Mandatory          |

#### PayPalDataCollectorConfig

This subsection describes the various parameters required by the `PayPalDataCollectorConfig`.

The following sample code demonstrates the response structure:

```Kotlin
data class PayPalDataCollectorConfig(
    val accessToken: String,
    val gatewayId: String
)
```

| Name           | Definition                                                                       | Type                       | Mandatory/Optional |
| :------------- | :------------------------------------------------------------------------------- | :------------------------- | :----------------- |
| accessToken    |  The OAuth access token required for authenticating API requests.                | String                     | Mandatory          |
| gatewayId      |  The PayPal gateway ID used to identify the payment gateway.                     | String                     | Mandatory          |

#### 3. Error/Exceptions Mapping

The following describes `PayPalDataCollectorUtil` exceptions that can be thrown. 

> **Note**:
>
> Any exception that would be thrown, would only be related to the initalisation of the PayPal SDK DataCollector. 
> This is to prevent the util class from being initialised if the PayPal SDK util cannot be initialised.

```Kotlin
InitialisationClientIdException(error: ApiErrorResponse) : PayPalDataCollectorException(error.displayableMessage)
UnknownException(displayableMessage: String) : PayPalDataCollectorException(displayableMessage)
```

| Exception                         | Description                                                                                   | Error Model                 |
| :-------------------------------- | :-------------------------------------------------------------------------------------------- | :-------------------------- |
| InitialisationClientIdException   |  Exception thrown when trying to retrieve the clientId to initialise the DataCollector        |  PayPalDataCollectorError   |
| UnknownException                  |  Exception thrown when an unknown error occurs in PayPalDataCollector initialisation.         |  PayPalDataCollectorError   |