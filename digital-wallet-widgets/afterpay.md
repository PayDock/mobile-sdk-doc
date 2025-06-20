# Afterpay

> 
>
> Enable customers to Buy Now and Pay Later with Afterpay. 

Use the Afterpay Widget to integrate with the Afterpay SDK. This handles the communication between Afterpay and the SDK. Once the transaction is completed, the widget returns the charge result.


## iOS

## How to use the AfterpayWidget

### Overview

Follow this guide to initialize and then use the `AfterpayWidget` widget in your application. This widget enables you to use Afterpay in your Payment flow.

For reference: [Afterpay SDK](https://github.com/afterpay/sdk-ios)

> **Note**:
>
> When the customer taps the Payment button in your Digital Wallet, the SDK triggers a callback to the Merchant app requesting a `wallet_token`. You must perform a wallet initialization request to receive your `wallet_token`. To do this, follow the instructions in the [generate a wallet_token](overview.md) section of this guide.

The code for the `AfterpayWidget` is as follows. None of the values are populated in this example as this is a definition.



```Swift
AfterpayyWidget(
    configuration: AfterpaySdkConfig,
    afterPayToken: @escaping (_ afterPayToken: @escaping (String) -> Void) -> Void,
    selectAddress: ((_ address: ShippingAddress, _ provideShippingOptions: ([ShippingOption]) -> Void) -> Void)?,
    selectShippingOption: ((_ shippingOption: ShippingOption, _ provideShippingOptionUpdateResult: (ShippingOptionUpdate?) -> Void) -> Void)?,
    buttonWidth: CGFloat,
    completion: @escaping (Result<ChargeResponse, AfterpayError>) -> Void)
) {...}
```

The following is the `AfterpayWidget` code populated with example values. This demonstrates how to use the widget in your application: 


```Swift
AfterpayWidget(
    configuration: viewModel.getAfterpayConfig(),
    afterPayToken: { onAfterpayButtonTap in
        viewModel.initializeWalletCharge(completion: onAfterpayButtonTap)
    }, selectAddress: { address, provideShippingOptions in
        // Based on the received `address` provide the available shipping options using `provideShippingOptions` completion block
    }, selectShippingOption: { shippingOption, provideShippingOptionUpdateResult in
        // Based on the received `shippingOption` provide the updated shipping option update if needed using `provideShippingOptions` completion block
    },
    buttonWidth: 360.0) { result in
        switch result {
        case .success(let chargeData): // Handle success with the provided charge data
        case .failure(let error): // Handle error
    }
}
```

### 2. Parameter Definitions

This subsection describes the parameters required by the `AfterpayWidget` view. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `AfterpayWidget`.

#### AfterpayWidget

| Name                  | Definition                                                                                           | Type                                                                                                                  | Mandatory/Optional |
| :-------------------- | :--------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------- | :----------------- |
| configuration         |  Configuration for the Afterpay SDK.                                                                 | `AfterpaySdkConfig`                                                                                                   | Mandatory          |
| afterPayToken         |  A callback to obtain the wallet token asynchronously.                                               | `(_ afterPayToken: @escaping (String) -> Void) -> Void`                                                               | Mandatory          |
| selectAddress         |  A callback to handle selection of shipping address.                                                 | `(_ address: ShippingAddress, _ provideShippingOptions: ([ShippingOption]) -> Void) -> Void`                          | Optional           |
| selectShippingOption  |  A callback to handle the selection for the shipping options.                                        | `(_ shippingOption: ShippingOption, _ provideShippingOptionUpdateResult: (ShippingOptionUpdate?) -> Void) -> Void`    | Optional           |
| buttonWidth           |  Sets preferred widget button size based on the provided width.                                      | `CGFloat`                                                                                                             | Optional           |
| completion            |  Result callback with the Charge creation API response if successful, or an error if unsuccessful.   | `(Result<ChargeResponse, AfterpayError>) -> Void)`                                                                    | Mandatory          |

#### AfterpaySdkConfig

| Name           | Definition                                                   | Type                      | Mandatory/Optional    |
| :------------- | :----------------------------------------------------------- | :------------------------ | :-------------------- |
| buttonTheme    |  The theme settings for the Afterpay payment button.         | `ButtonTheme`             | Optional              |
| config         |  The main configuration settings for Afterpay.               | `AfterpayConfiguration`   | Mandatory             |
| environment    |  Environment of the afterpay widget.                         | `Environment`             | Mandatory             |
| options        |  Additional checkout options for Afterpay.                   | `CheckoutOptions`         | Optional              |

#### ButtonTheme

| Name           | Definition                                                   | Type                                        | Mandatory/Optional    |
| :------------- | :----------------------------------------------------------- | :------------------------------------------ | :-------------------- |
| buttonType     |  The text displayed on the payment button.                   | Enum: `Afterpay.ButtonKind`                 | Optional              |
| colorScheme    |  The color scheme of the payment button.                     | Enum: `Afterpay.ColorScheme`                | Optional              |

#### AfterpayConfiguration

| Name           | Definition                                                       | Type       | Mandatory/Optional    |
| :------------- | :--------------------------------------------------------------- | :--------- | :-------------------- |
| minimumAmount  |  The minimum transaction amount allowed.                         | String     | Optional              |
| maximumAmount  |  The maximum transaction amount allowed.                         | String     | Mandatory             |
| currency       |  The currency for transactions.                                  | String     | Mandatory             |
| language       |  The language for localization, default is device's language.    | String     | Optional              |
| country        |  The country for localization, default is device's country.      | String     | Optional              |

#### CheckoutOptions

| Name                              | Definition                                                       | Type       | Mandatory/Optional    |
| :-------------------------------- | :--------------------------------------------------------------- | :--------- | :-------------------- |
| pickup                            |  Indicates whether pickup option is enabled.                     | String     | Optional              |
| buyNow                            |  Indicates whether buy now option is enabled.                    | String     | Optional              |
| shippingOptionRequired            |  Indicates whether shipping option is required.                  | String     | Optional              |
| enableSingleShippingOptionUpdate  |  Indicates whether single shipping option update is enabled.     | String     | Optional              |

#### ChargeResponse

| Name           | Definition                                | Type                 |
| :------------- | :---------------------------------------- | :------------------- |
| status         |  Charge result status as a string         | String               |
| amount         |  Charge amount that was charged           | Decimal              |
| currency       |  Charge currency that was used            | String               |

#### MobileSDK.AfterpayError

| Name                       | Description                                                             | Error Result    |
| :------------------------- | :---------------------------------------------------------------------- | :-------------- |
| errorFetchingAfterpayUrl   |  Error thrown when fetching Afterpay widget URL                         | ErrorRes        |
| errorCapturingCharge       |  Error thrown when capturing the charge has failed                      | ErrorRes        |
| errorCancelingTransaction  |  Error thrown when canceling the transaction has failed                 | ErrorRes        |
| transactionCanceled        |  Error thrown when the transaction was canceled due to failure or user  | nil             |
| unknownError               |  Error thrown when an unknown error has occurred                        | nil             |


### 3. Callback Explanation

#### Token Callback

The `token` callback obtains the wallet token asynchronously. It receives a callback function `(_ afterPayToken: @escaping (String) -> Void) -> Void` as a parameter, and this is invoked with the wallet token once it is obtained.

#### Select Address Callback

The `selectAddress` callback handles the selection of shipping address changes and selections. It receives a callback function `((_ address: ShippingAddress, _ provideShippingOptions: ([ShippingOption]) -> Void) -> Void)?` as a parameter.
This is used in conjunction with the Afterpay SDK `CheckoutV2Handler.onShippingAddressDidChange` which is invoked when the `ShippingAddress` changes and the selected address can be updated.

#### Select Shipping Options Callback

The `selectShippingOption` callback is used to handle the selection of shipping options. It receives a callback function `((_ shippingOption: ShippingOption, _ provideShippingOptionUpdateResult: (ShippingOptionUpdate?) -> Void) -> Void)?` as a parameter.
This is used in conjunction with the Afterpay SDK `CheckoutV2Handler.onShippingOptionDidChange` which is invoked when `ShippingOption` changes and the selected option can be updated.

#### Completion Callback

The `completion` callback is invoked after the payment operation is completed. It receives a `(Result<ChargeResponse, AfterpayError>) -> Void` if the payment is successful. The callback handles the outcome of the payment operation.

## Android 

## How to use the AfterpayWidget

### 1. Overview

Follow this guide to initialize and then use the `AfterpayWidget` widget in your application. This widget enables you to use Afterpay in your Payment flow.

For reference: [Afterpay SDK](https://github.com/afterpay/sdk-android)

> **Note**:
>
> When the customer taps the Payment button for your Digital Wallet, the SDK triggers a callback to the Merchant app requesting a `wallet_token`. You must perform a wallet initialization request to receive your `wallet_token`. To do this, follow the instructions in the [generate a wallet_token](overview.mdx) section of this guide.  

The code for the `AfterpayWidget` is as follows. None of the values are populated in this example as this is a definition.

```Kotlin
fun AfterpayWidget(
    modifier: Modifier,
    config: AfterpaySDKConfig,
    enabled: Boolean,
    tokenRequest: (tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit,
    selectAddress: (address: BillingAddress, provideShippingOptions: (List<AfterpayShippingOption>) -> Unit) -> Unit,
    selectShippingOption: (
        shippingOption: AfterpayShippingOption,
        provideShippingOptionUpdateResult: (AfterpayShippingOptionUpdate?) -> Unit
    ) -> Unit,
    loadingDelegate: WidgetLoadingDelegate?,
    completion: (Result<ChargeResponse>) -> Unit
) {...}
```

The following is the `AfterpayWidget` code populated with example values. This demonstrates how to use the widget in your application: 

```Kotlin
// Initialize the AfterpaySDKConfig
val configuration = AfterpaySDKConfig(
    config = AfterpaySDKConfig.AfterpayConfiguration(
        maximumAmount = "100",
        currency = AU_CURRENCY_CODE,
        language = "en",
        country = AU_COUNTRY_CODE
    ),
    options = AfterpaySDKConfig.CheckoutOptions(
        shippingOptionRequired = true,
        enableSingleShippingOptionUpdate = true
    )
)
// Initialize the AfterpayWidget
AfterpayWidget(
    modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp),
   tokenRequest = { callback ->
        // show widget loader
        tokenResult.onSuccess { result ->
            // Handle token success
            val walletToken = result.token // ... retrieve the token
        }.onFailure {
            // Handle token failure success
        }
    },
    config = configuration,
    selectAddress = { _, provideShippingOptions ->
        val currency = Currency.getInstance(configuration.config.currency)
        val shippingOptions = listOf(
            AfterpayShippingOption(
                "standard",
                "Standard",
                "",
                currency,
                "0.00".toBigDecimal(),
                "50.00".toBigDecimal(),
                "0.00".toBigDecimal(),
            ),
            AfterpayShippingOption(
                "priority",
                "Priority",
                "Next business day",
                currency,
                "10.00".toBigDecimal(),
                "60.00".toBigDecimal(),
                null,
            )
        )
        provideShippingOptions(shippingOptions)
    },
    selectShippingOption = { shippingOption, provideShippingOptionUpdateResult ->
        val currency = Currency.getInstance(configuration.config.currency)
        // if standard shipping was selected, update the amounts
        // otherwise leave as is by passing null
        val result: AfterpayShippingOptionUpdate? =
            if (shippingOption.id == "standard") {
                AfterpayShippingOptionUpdate(
                    "standard",
                    currency,
                    "0.00".toBigDecimal(),
                    "50.00".toBigDecimal(),
                    "2.00".toBigDecimal(),
                )
            } else {
                null
            }
        provideShippingOptionUpdateResult(result)
    }
) { result ->
    result.onSuccess {
        Log.d("[AfterpayWidget]", "Success: $it")
    }.onFailure {
        val error = it.toError()
        Log.d("[AfterpayWidget]", "Failure: ${error.displayableMessage}")
    }
}
```

### 2. Parameter Definitions

This subsection describes the parameters required by the `AfterpayWidget` composable. It provides information on the purpose of each parameter and the parameters' significance in configuring the behavior of the `AfterpayWidget`.

#### AfterpayWidget

| Name                  | Definition                                                                               | Type                                                                                                                                         | Mandatory/Optional |
| :-------------------- | :--------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------- | :----------------- |
| modifier              |  Compose modifier for container modifications.                                           | `androidx.compose.ui.Modifier`                                                                                                               | Optional           |
| config                |  The configuration for the Afterpay SDK.                                                 | `AfterpaySDKConfig`                                                                                                                          | Mandatory          |
| enabled               |  A boolean to enable or disable the payment button.                                      | Boolean                                                                                                                 | Optional           |
| tokenRequest          |  A callback to obtain the wallet token result asynchronously                      | `tokenRequest: (tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit`     | Mandatory          |
| selectAddress         |  A callback to handle selection of shipping address.                                     | `(address: BillingAddress, provideShippingOptions: (List<AfterpayShippingOption>) -> Unit) -> Unit`                                          | Optional           |
| selectShippingOption  |  A callback to handle selection of shipping option.                                      | `selectShippingOption: (shippingOption: AfterpayShippingOption, provideShippingOptionUpdateResult: (AfterpayShippingOptionUpdate?) -> Unit)` | Optional           |
| loadingDelegate       |  Delegate control of showing loaders to this instance. When set, internal loaders are not shown.                           | `WidgetLoadingDelegate`                                                                             | Optional          |
| completion            |  Result callback with the Charge creation API response if successful, or error if not.   | `(Result<ChargeResponse>) -> Unit`                                                                                                           | Mandatory          |

#### AfterpaySDKConfig

| Name           | Definition                                                   | Type                      | Mandatory/Optional    |
| :------------- | :----------------------------------------------------------- | :------------------------ | :-------------------- |
| buttonTheme    |  The theme settings for Afterpay payment button.             | `ButtonTheme`             | Optional              |
| config         |  The main configuration settings for Afterpay.               | `AfterpayConfiguration`   | Mandatory             |
| options        |  Additional checkout options for Afterpay.                   | `CheckoutOptions`         | Optional              |

#### ButtonTheme

| Name           | Definition                                                   | Type                                        | Mandatory/Optional    |
| :------------- | :----------------------------------------------------------- | :------------------------------------------ | :-------------------- |
| buttonText     |  The text displayed on the payment button.                   | Enum: `AfterpayPaymentButton.ButtonText`    | Optional              |
| colorScheme    |  The color scheme of the payment button.                     | Enum: `AfterpayColorScheme`                 | Optional              |

#### AfterpayConfiguration

| Name           | Definition                                                       | Type       | Mandatory/Optional    |
| :------------- | :--------------------------------------------------------------- | :--------- | :-------------------- |
| minimumAmount  |  The minimum transaction amount allowed.                         | String     | Optional              |
| maximumAmount  |  The maximum transaction amount allowed.                         | String     | Mandatory             |
| currency       |  The currency for transactions.                                  | String     | Mandatory             |
| language       |  The language for localization, default is device's language.    | String     | Optional              |
| country        |  The country for localization, default is device's country.      | String     | Optional              |

#### CheckoutOptions

| Name                              | Definition                                                       | Type       | Mandatory/Optional    |
| :-------------------------------- | :--------------------------------------------------------------- | :--------- | :-------------------- |
| pickup                            |  Indicates whether pickup option is enabled.                     | String     | Optional              |
| buyNow                            |  Indicates whether buy now option is enabled.                    | String     | Mandatory             |
| shippingOptionRequired            |  Indicates whether shipping option is required.                  | String     | Mandatory             |
| enableSingleShippingOptionUpdate  |  Indicates whether single shipping option update is enabled.     | String     | Optional              |

#### BillingAddress

| Name          | Definition                                                                           | Type       | Mandatory/Optional                           |
| :------------ | :----------------------------------------------------------------------------------- | :--------- | :------------------------------------------  |
| name          |  The name associated with the billing address                                        | String     | Optional to provide and for the user         |
| addressLine1  |  The first line of the billing address, typically containing street information      | String     | Optional to provide / Mandatory for the user |
| addressLine2  |  The optional second line of the billing address, often used for additional details  | String     | Optional to provide and for the user         |
| city          |  The city or locality of the billing address                                         | String     | Optional to provide / Mandatory for the user |
| state         |  The state or region of the billing address                                          | String     | Optional to provide / Mandatory for the user |
| postalCode    |  The Postal or ZIP code of the billing address                                       | String     | Optional to provide / Mandatory for the user |
| country       |  The country of the billing address                                                  | String     | Optional to provide / Mandatory for the user |
| phoneNumber   |  The phone number associated with the billing address                                | String     | Optional to provide and for the user         |

#### AfterpayShippingOption

Represents a shipping option available for Afterpay transactions.

| Name           | Definition                                                   | Type          | Mandatory/Optional    |
| :------------- | :----------------------------------------------------------- | :------------ | :-------------------- |
| id             |  The unique identifier of the shipping option.               | String        | Mandatory             |
| name           |  The name of the shipping option.                            | String        | Mandatory             |
| description    |  The description of the shipping option.                     | String        | Mandatory             |
| currency       |  The currency of the shipping option.                        | `Currency`    | Mandatory             |
| shippingAmount |  The shipping amount associated with the shipping option.    | `BigDecimal`  | Mandatory             |
| orderAmount    |  The order amount associated with the shipping option.       | `BigDecimal`  | Mandatory             |
| taxAmount      |  The tax amount associated with the shipping option.         | `BigDecimal`  | Optional              |

#### AfterpayShippingOptionUpdate

Represents an update to a shipping option for Afterpay transactions.

| Name           | Definition                                                           | Type          | Mandatory/Optional    |
| :------------- | :------------------------------------------------------------------- | :------------ | :-------------------- |
| id             |  The unique identifier of the shipping option.                       | String        | Mandatory             |
| currency       |  The currency of the shipping option.                                | `Currency`    | Mandatory             |
| shippingAmount |  The updated shipping amount associated with the shipping option.    | `BigDecimal`  | Mandatory             |
| orderAmount    |  The updated order amount associated with the shipping option.       | `BigDecimal`  | Mandatory             |
| taxAmount      |  The updated tax amount associated with the shipping option.         | `BigDecimal`  | Optional              |


#### ChargeResponse

This subsection outlines the structure of the result or response object returned by the `AfterpayWidget` composable. It details the format and components of the object, enabling you to handle the response effectively within your application.

The following sample code demonstrates the response structure:

```Kotlin
data class ChargeResponse(
    val status: Int, 
    val resource: ChargeResource
) {

    data class ChargeResource(
        val type: String,
        val data: ChargeData?
    )
    
    data class ChargeData(
        val status: String,
        val id: String,
        val amount: BigDecimal,
        val currency: String
    )
}
```

| Name           | Definition                                                                       | Type                       | 
| :------------- | :------------------------------------------------------------------------------- | :------------------------- | 
| status         |  Charge result HTTP status code                                                  | Int                        | 
| resource       |  Charge resource containing data related to charge                               | `ChargeResponse.Resource`  |

#### ChargeResponse.Resource

| Name           | Definition                                                                                      | Type                       | 
| :------------- | :---------------------------------------------------------------------------------------------- | :------------------------- | 
| type           |  Charge result HTTP status code                                                                 | String                     | 
| data           |  Charge data result containing actual data regarding charge ie. amount, currency etc            | `ChargeResponse.Data`      |

#### ChargeResponse.ChargeData

| Name           | Definition                                | Type                 | 
| :------------- | :---------------------------------------- | :------------------- | 
| status         |  Charge result status as a string         | String               | 
| amount         |  Charge amount that was charged           | `BigDecimal`         |
| currency       |  Charge currency that was used            | String               |

### 3. Callback Explanation

#### Token Callback

The `token` callback obtains the wallet token result asynchronously. It receives a callback function `tokenRequest: (tokenResult: (Result<WalletTokenResult>) -> Unit) -> Unit` as a parameter, which you must invoke with the result of the wallet token API request once it is obtained. 

The `WalletTokenResult` acts as a token wrapper containing the actual token result.

#### Select Address Callback

The `selectAddress` callback handles the selection of shipping address changes and selections. It receives a callback function `(address: BillingAddress, provideShippingOptions: (List<AfterpayShippingOption>) -> Unit) -> Unit` as a parameter.
This is used in conjunction with the Afterpay SDK `AfterpayCheckoutV2Handler.onShippingAddressDidChange` which is invoked when `ShippingAddress` changes and the selected address can be updated.

#### Select Shipping Options Callback

The `selectShippingOption` callback handles the selection of shipping options. It receives a callback function `(shippingOption: AfterpayShippingOption, provideShippingOptionUpdateResult: (AfterpayShippingOptionUpdate?) -> Unit) -> Unit` as a parameter.
This is used in conjunction with the Afterpay SDK `AfterpayCheckoutV2Handler.onShippingOptionDidChange` which is invoked when `ShippingOption` changes and the selected option can be updated.

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

The `completion` callback is invoked after the payment operation is completed. It receives a `Result<ChargeResponse>` if the payment is successful. The callback handles the outcome of the payment operation.

> **Note**:
>
> If an error does occur within the Afterpay process, it would first return an `Result.failure` with the error exception. But it will also return a `Result.success` after completing the charge returning a `ChargeResponse` with the failed/declined result.

### 4. Error/Exceptions Mapping

The following describes Afterpay exceptions that can be thrown. 

```Kotlin
FetchingUrlException(error: ApiErrorResponse) : AfterpayException(error.displayableMessage)
CapturingChargeException(error: ApiErrorResponse) : AfterpayException(error.displayableMessage)
TokenException(displayableMessage: String) : AfterpayException(displayableMessage)
ConfigurationException(displayableMessage: String) : AfterpayException(displayableMessage)
CancellationException(displayableMessage: String) : AfterpayException(displayableMessage)
InvalidResultException(displayableMessage: String) : AfterpayException(displayableMessage)
InitialisationWalletTokenException(displayableMessage: String, val errorBody: String?) : AfterpayException(displayableMessage)
UnknownException(displayableMessage: String) : AfterpayException(displayableMessage)
```

| Exception                 | Description                                                                                   | Error Model       |
| :------------------------ | :-------------------------------------------------------------------------------------------- | :---------------- |
| FetchingUrlException      |  Exception thrown when there is an error fetching the URL for Afterpay.                       |  AfterpayError    |
| CapturingChargeException  |  Exception thrown when there is an error capturing the charge for Afterpay.                   |  AfterpayError    |
| TokenException            |  Exception thrown when there is an error with the Afterpay token.                             |  AfterpayError    |
| ConfigurationException    |  Exception thrown when there is a configuration error related to Afterpay.                    |  AfterpayError    |
| CancellationException     |  Exception thrown when there is a cancellation error related to Afterpay.                     |  AfterpayError    |
| InvalidResultException    |  Exception thrown when there is an invalid intent result error related to Afterpay.           |  AfterpayError    |
| InitialisationWalletTokenException            |  Exception thrown when the wallet token result returns a failure result.  |  AfterpayError    |
| UnknownException          |  Exception thrown when there is an unknown error related to Afterpay.                         |  AfterpayError    |

