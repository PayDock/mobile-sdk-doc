# Click to Pay

> 
>
>Integrate the Mastercard Click to Pay solution into your Mobile checkout.

## Overview

Create a payment workflow for mobile checkout that includes Click to Pay (Built on EMV® Secure Remote Commerce standards), or manual card entry, with the Click to Pay widget. For a full overview of the use cases, familiarise yourself with [the Click to Pay scenarios](https://developer.mastercard.com/mastercard-checkout-solutions/documentation/testing/test_cases/click_to_pay_case/) on the Mastercard website.

The Click to Pay transforms the inputted details into a One-time-token (OTT). You can then use this Token with other Paydock API calls, including Capture Payment, Save Card to Vault, and so on.

## iOS

### How to use the Mastercard SRC Click To Pay Widget

This section describes how to initialise and use the `ClickToPayWidget` view in your application for iOS. The widget integrates with the Paydock `client-sdk` and performs tokenisation to retrieve a OTT (One-Time-Token).

The definition of the `ClickToPayWidget` is as follows:

```Swift
ClickToPayWidget(
    serviceId: String,
    accessToken: String,
    meta: ClickToPayMeta?
    completion: (Result<ClickToPayResult, ClickToPayError> -> Void)
```

### Parameter definitions

The following table defines the parameters required by the `ClickToPayWidget` view. The table describes the purpose of each parameter and its functionality when configuring the `ClickToPayWidget`.

#### ClickToPayWidget
| Name                | Definition                                                                                                | Type                                                   | Mandatory/Optional |
| :------------------ | :-------------------------------------------------------------------------------------------------------- | :----------------------------------------------------- | :----------------  |
| serviceId           |  This is the `id` of the SRC Service created on Paydock                                                   | String                                                 | Mandatory          |
| accessToken         |  The access token used for authentication with the backend service.                                       | String                                                 | Mandatory          |
| meta                |  Object that contains additional data used for the SRC Checkout.                                          | `ClickToPayMeta`                                       | Optional           |
| completion          |  Result callback with the `ClickToPayResult`. Contains token if successful or error in case of failure    | `(Result<ClickToPayResult, ClickToPayError> -> Void)`  | Mandatory          |

The following tables describe the properties of the `ClickToPayDPAData` object used by the `client-sdk`. They provide information on the data related to Mastercard's Digital Payment Application (DPA).

#### ClickToPayResult
| Name            | Definition                                          | Type           | Mandatory/Optional   |
| :-------------- | :-------------------------------------------------- | :------------- | :------------------  |
| event           |  Enum indicating outcome of the widget flow.        | `Event`        | Mandatory            |
| mastercardToken |  OTT received from the ClickToPay widget            | String         | Optional             |

#### ClickToPayMeta
| Name          | Definition                                                                                            | Type                    | Mandatory/Optional   |
| :------------ | :---------------------------------------------------------------------------------------------------- | :---------------------- | :------------------  |
| dpaData               |  Object where the DPA creation data is stored.                                                | `ClickToPayDPAData`     | Optional             |
| disableSummaryScreen  |  flag that controls if a final summary screen is presented in the checkout flow.              | Boolean                 | Optional             |
| cardBrands            |  List of allowed card brands - options: 'mastercard', 'maestro', 'visa', 'amex', 'discover'   | `Array<Enum>`           | Optional             |
| coBrandNames          |  List of co-brand names associated with the SRC experience.                                   | `Array<String>`         | Optional             |
| checkoutExperience    |  Checkout experience type, either 'WITHIN_CHECKOUT' or 'PAYMENT_SETTINGS'.                    | Enum                    | Optional             |
| services              |  Services offered, such as 'INLINE_CHECKOUT' or 'INLINE_INSTALLMENTS'.                        | Enum                    | Optional             |
| dpaTransactionOptions |  Object that stores options for creating a transaction with DPA.                              | `ClickToPayDPAOptions`  | Optional             |

#### ClickToPayDPAData
| Name                      | Definition                                                    | Type                  | Mandatory/Optional   |
| :------------------------ | :------------------------------------------------------------ | :-------------------- | :------------------  |
| dpaAddress                |  Address associated with the DPA.                             | String                | Optional             |
| dpaEmailAddress           |  Email address for DPA communication.                         | String                | Optional             |
| dpaPhoneNumber            |  Phone number structure for DPA communication.                | `PhoneNumber`         | Optional             |
| dpaLogoUri                |  URI for the DPA logo.                                        | String                | Optional             |
| dpaSupportedEmailAddress  |  Supported email address for DPA support.                     | String                | Optional             |
| dpaSupportedPhoneNumber   |  Supported phone number for DPA support.                      | `PhoneNumber`         | Optional             |
| dpaUri                    |  URI for DPA.                                                 | String                | Optional             |
| dpaSupportUri             |  URI for DPA support.                                         | String                | Optional             |
| applicationType           |  Application type, either 'WEB_BROWSER' or 'MOBILE_APP'.      | Enum                  | Optional             |

#### ClickToPayDPAOptions
| Name                 | Definition                                                                    | Type                   | Mandatory/Optional   |
| :------------------- | :---------------------------------------------------------------------------- | :--------------------- | :------------------  |
| dpaBillingPreference |  Billing preferences for DPA, options are 'FULL', 'POSTAL_COUNTRY', 'NONE'.   | Enum                   | Optional             |
| paymentOptions       |  Payment options included in the transaction.                                 | `Array<PaymentOption>` | Optional             |
| orderType            |  Type of the order, options are 'SPLIT_SHIPMENT', 'PREFERRED_CARD'.           | Enum                   | Optional             |
| threeDSPreference    |  Preference for 3DS usage in the transaction.                                 | String                 | Optional             |
| confirmPayment       |  Indicates if payment confirmation is required.                               | Bool                   | Optional             |

#### PhoneNumber
| Name          | Definition                                       | Type         | Mandatory/Optional   |
| :------------ | :----------------------------------------------- | :----------- | :------------------  |
| countryCode   |  The country code of the phone number.           | String       | Mandatory            |
| phoneNumber   |  The phone number part of the phone number.      | String       | Mandatory            |

#### PaymentOption
| Name              | Definition                      | Type         | Mandatory/Optional   |
| :---------------- | :------------------------------ | :----------- | :------------------  |
| dynamicDataType   |  Dynamic data types.            | String       | Mandatory            |

#### MobileSDK.ClickToPayError

| Name                       | Description                                                                     | Error Result            |
| :------------------------ | :------------------------------------------------------------------------------- | :---------------------- |
| webViewFailed             |  Error thrown when there is an error while communicating with a WebView.         |  NSError                |
| UnknownException          |  Error thrown when there is an unknown error related to ClickToPay.              |  nil                    |

### Callback Explanation

The `completion` callback is invoked after the SRC tokenisation flow is completed. The SRC flow receives a `ClickToPayResult` and, after the successful tokenisation of the SRC details, generates an OTT.

## Android

### How to use the Mastercard SRC Click To Pay Widget

This section demonstrates how to initialise and use the `ClickToPayWidget` composable in your application. The widget integrates with the Paydock `client-sdk` and performs tokenisation to retrieve an OTT.

The definition of the `ClickToPayWidget` is as follows:

```Kotlin
@Composable
fun ClickToPayWidget(
    modifier: Modifier,
    accessToken: String,
    serviceId: String,
    meta: ClickToPayMeta?,
    completion: (Result<String>) -> Unit
) {...}
```

Populate the required fields to use the widget in your application. Some fields are optional depending on your use case:

```Kotlin
// Initialize the ClickToPay
ClickToPayWidget(
    modifier = Modifier.fillMaxWidth(), // optional
    accessToken = ACCESS_TOKEN, // required
    serviceId = GATEWAY_ID_CLICK_TO_PAY, // required
    meta = ClickToPayMeta(
        disableSummaryScreen = true
    ) // optional
) { result ->
    result.onSuccess {
        // Handle success - Update UI or perform actions
        Log.d("ClickToPayWidget", "SRC Tokenisation successful. OTT: $token")
    }.onFailure {
        // Handle failure - Show error message or take appropriate action
        Log.e("ClickToPayWidget", "SRC Tokenisation failed. Error: ${exception.message}")
    }
}
```

### Parameter definitions

The following table describes the parameters for the `ClickToPayWidget` composable. The table outlines the purpose and function of each parameter, the type, and whether it is optional or mandatory.

#### ClickToPayWidget
| Name                | Definition                                                                                                | Type                        | Mandatory/Optional |
| :------------------ | :-------------------------------------------------------------------------------------------------------- | :-------------------------- | :----------------  |
| modifier            |  Compose modifier for container modifications                                                             | `Modifier`                  | Optional           |
| accessToken         |  The access token used for authentication with the backend service.                                       | String                      | Mandatory          |
| serviceId           |  This is the `id` of the SRC Service created on Paydock                                                   | String                      | Mandatory          |
| meta                |  Object that contains additional data used for the SRC Checkout.                                          | `ClickToPayMeta`            | Optional           |
| completion          |  Result callback with the SRC OTT if successful, or error if not.                                         | `(Result<String>) -> Unit`  | Mandatory          |

The following tables describe the properties of the `ClickToPayMeta` object that are used by the `client-sdk`. The tables describe the data related to Mastercard's Digital Payment Application (DPA).

#### ClickToPayMeta
| Name          | Definition                                                                                            | Type                    | Mandatory/Optional   |
| :------------ | :---------------------------------------------------------------------------------------------------- | :---------------------- | :------------------  |
| dpaData               |  Object where the DPA creation data is stored.                                                | `ClickToPayDPAData`     | Optional             |
| disableSummaryScreen  |  flag that controls if a final summary screen is presented in the checkout flow.              | Boolean                 | Optional             |
| cardBrands            |  List of allowed card brands - options: 'mastercard', 'maestro', 'visa', 'amex', 'discover'   | `List<Enum>`            | Optional             |
| coBrandNames          |  List of co-brand names associated with the SRC experience.                                   | `List<String>`          | Optional             |
| checkoutExperience    |  Checkout experience type, either 'WITHIN_CHECKOUT' or 'PAYMENT_SETTINGS'.                    | Enum                    | Optional             |
| services              |  Services offered, such as 'INLINE_CHECKOUT' or 'INLINE_INSTALLMENTS'.                        | Enum                    | Optional             |
| dpaTransactionOptions |  Object that stores options for creating a transaction with DPA.                              | `ClickToPayDPAOptions`  | Optional             |

#### ClickToPayDPAData
| Name                      | Definition                                                    | Type                  | Mandatory/Optional   |
| :------------------------ | :------------------------------------------------------------ | :-------------------- | :------------------  |
| dpaAddress                |  Address associated with the DPA.                             | String                | Optional             |
| dpaEmailAddress           |  Email address for DPA communication.                         | String                | Optional             |
| dpaPhoneNumber            |  Phone number structure for DPA communication.                | `PhoneNumber`         | Optional             |
| dpaLogoUri                |  URI for the DPA logo.                                        | String                | Optional             |
| dpaSupportedEmailAddress  |  Supported email address for DPA support.                     | String                | Optional             |
| dpaSupportedPhoneNumber   |  Supported phone number for DPA support.                      | `PhoneNumber`         | Optional             |
| dpaUri                    |  URI for DPA.                                                 | String                | Optional             |
| dpaSupportUri             |  URI for DPA support.                                         | String                | Optional             |
| applicationType           |  Application type, either 'WEB_BROWSER' or 'MOBILE_APP'.      | Enum                  | Optional             |

#### ClickToPayDPAOptions
| Name                 | Definition                                                                    | Type                   | Mandatory/Optional   |
| :------------------- | :---------------------------------------------------------------------------- | :--------------------- | :------------------  |
| dpaBillingPreference |  Billing preferences for DPA, options are 'FULL', 'POSTAL_COUNTRY', 'NONE'.   | Enum                   | Optional             |
| paymentOptions       |  Payment options included in the transaction.                                 | `List<PaymentOption>`  | Optional             |
| orderType            |  Type of the order, options are 'SPLIT_SHIPMENT', 'PREFERRED_CARD'.           | Enum                   | Optional             |
| threeDSPreference    |  Preference for 3DS usage in the transaction.                                 | String                 | Optional             |
| confirmPayment       |  Indicates if payment confirmation is required.                               | Boolean                | Optional             |

#### PhoneNumber
| Name          | Definition                                       | Type         | Mandatory/Optional   |
| :------------ | :----------------------------------------------- | :----------- | :------------------  |
| countryCode   |  The country code of the phone number.           | String       | Mandatory            |
| phoneNumber   |  The phone number part of the phone number.      | String       | Mandatory            |

#### PaymentOption
| Name              | Definition                      | Type         | Mandatory/Optional   |
| :---------------- | :------------------------------ | :----------- | :------------------  |
| dynamicDataType   |  Dynamic data types.            | String       | Mandatory            |

### Callback Explanation

After the merchant completes the SRC tokenisation flow, they invoke the `completion` callback. Once the SRC details are tokenised successfully, the merchant receives a `Result<String>` containing the OTT.

### Error/Exceptions Mapping

The following describes ClickToPay exceptions that can be thrown. 

```Kotlin
CheckoutErrorException(displayableMessage: String) : ClickToPayException(error.displayableMessage)
WebViewException(code: Int?, displayableMessage: String) : ClickToPayException(displayableMessage)
CancellationException(displayableMessage: String) : ClickToPayException(displayableMessage)
```

| Exception                 | Description                                                                                   | Error Model        |
| :------------------------ | :-------------------------------------------------------------------------------------------- | :----------------- |
| CheckoutErrorException    |  Exception thrown when there is an error during the checkout process for Click to Pay.        |  ClickToPayError   |
| WebViewException          |  Exception thrown when there is an error while communicating with a WebView.                  |  ClickToPayError   |
| CancellationException     |  Exception thrown when there is a cancellation error related to Click to Pay.                 |  ClickToPayError   |
