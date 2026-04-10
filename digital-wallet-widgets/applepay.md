# Apple Pay Widget

> 
>
>Use the iOS SDK to make payments using ApplePay. 

The logic of this widget exists as a single view that you can initialise and embed in your payment flow.

## How to use the ApplePay widget

1. Use the following to initialise the **ApplePayWidget**:

```Swift    
ApplePayWidget(
    config: ApplePayWidgetConfig,
    appearance: ApplePayWidgetAppearance = ApplePayWidgetAppearance(),
    eventDelegate: WidgetEventDelegate? = nil,
    onShippingContactSelected: ((PKContact) -> PKPaymentRequestShippingContactUpdate)? = nil,
    onShippingMethodSelected: ((PKShippingMethod) -> PKPaymentRequestShippingMethodUpdate)? = nil,
    completion: @escaping (Result<ApplePayResult, ApplePayError>) -> Void
    ) { ... }
)
```

If the tokenisation is successful, the **ApplePayWidget** returns an `ApplePayResult` that contains all the relevant information. In case of an error, the `ApplePayError` object is returned with information regarding the failure.

2. The **ApplePayWidgetConfig** contains all of the required data to enable ApplePay.

```Swift
/// Configuration for Apple Pay widget
public struct ApplePayWidgetConfig {

    /// The Paydock service ID for Apple Pay
    public let serviceId: String

    /// The widget access token for API authentication
    public let accessToken: String

    /// The PKPaymentRequest configured with merchant details
    public let pkPaymentRequest: PKPaymentRequest

    /// Whether to show setup button if no cards configured in wallet for default networks
    public let showSetUpButtonWhenNoCardsEnrolled: Bool

    public init(serviceId: String,
                accessToken: String,
                pkPaymentRequest: PKPaymentRequest,
                showSetUpButtonWhenNoCardsEnrolled: Bool = false) {
        self.serviceId = serviceId
        self.accessToken = accessToken
        self.pkPaymentRequest = pkPaymentRequest
        self.showSetUpButtonWhenNoCardsEnrolled = showSetUpButtonWhenNoCardsEnrolled
    }
}
```

3. The Mobile SDK provides convenient helper methods to initialise the `PKPaymentRequest` object. You can then configure this in various ways, depending on your use case:

```Swift
    public static func createApplePayRequest(
        amount: Decimal,
        amountLabel: String,
        countryCode: String,
        currencyCode: String,
        merchantIdentifier: String,
        merchantCapabilities: PKMerchantCapability = [.capabilityCredit, .capabilityDebit, .capability3DS],
        supportedNetworks: [PKPaymentNetwork] = [.visa, .masterCard, .amex, .discover],
        requireBillingAddress: Bool = false,
        requireShippingAddress: Bool = false,
        shippingOptions: [PKShippingMethod]? = nil) -> PKPaymentRequest {
            let item = PKPaymentSummaryItem(label: amountLabel, amount: amount as NSDecimalNumber, type: .final)
            let paymentRequest = PKPaymentRequest()
            paymentRequest.paymentSummaryItems = [item]
            paymentRequest.countryCode = countryCode
            paymentRequest.currencyCode = currencyCode
            paymentRequest.merchantIdentifier = merchantIdentifier
            paymentRequest.merchantCapabilities = merchantCapabilities
            paymentRequest.supportedNetworks = supportedNetworks
            paymentRequest.requiredBillingContactFields = requireBillingAddress ? [.name, .postalAddress] : []
            paymentRequest.requiredShippingContactFields = requireShippingAddress
                ? [.phoneNumber, .emailAddress, .postalAddress, .name]
                : []
            paymentRequest.shippingMethods = shippingOptions
            return paymentRequest
    }
```

### Parameter Definitions

| Name                        | Definition                                                                                           | Type                                                                                       | Mandatory/Optional |
| :-------------------------- | :--------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------- | :----------------- |
| config                      |  Configuration options for the Apple Pay widget                                                      | `ApplePayWidgetConfig`                                                                     | Mandatory          |
| appearance                  |  Object for visual customization of the widget.                                                      | `MobileSDK.ApplePayWidgetAppearance`                                                       | Optional           |
| eventDelegate               |  Delegate for handling widget events such as button clicks.                                          | `MobileSDK.WidgetEventDelegate`                                                            | Optional           |
| onShippingContactSelected   |  Callback to allow merchant to react to shipping contact updates within Apple Pay.                   | `(PKContact) -> PKPaymentRequestShippingContactUpdate)`                                    | Optional           |
| onShippingMethodSelected    |  Callback to allow merchant to react to shipping method selection update within Apple Pay.           | `(PKShippingMethod) -> PKPaymentRequestShippingMethodUpdate`                               | Optional           |
| completion                  |  Result callback with the toenisation result if successful, or an error if unsuccessful.             | `(Result<ApplePayResult, ApplePayError>) -> Void)`                                         | Mandatory          |

## Definitions

More in depth definitions of the parameters, as well as potential errors and responses, are as follows:

### MobileSDK.ApplePayWidgetConfig

| Name                                 | Definition                                                                                       | Type                                               | Mandatory/Optional |
| :----------------------------------- | :----------------------------------------------------------------------------------------------- | :------------------------------------------------- |:------------------ |
| serviceId                            |  Service ID for the Apple Pay service registered in Paydock dashboard.                           | String                                             | Mandatory          |
| accessToken                          |  The access token used for authentication with the backend service.                              | String                                             | Mandatory          |
| pkPaymentRequest                     |  The Apple Pay configuration to be used for processing payment.                                  | `PKPaymentRequest`                                 | Mandatory          |
| showSetUpButtonWhenNoCardsEnrolled   |  Whether to show setup if user has no cards setup in wallet from default support list.           | Bool                                               | Optional           |

For more info on `PKPaymentRequest`. See https://developer.apple.com/documentation/passkit/pkpaymentrequest.                                                      | Swift.String             | Mandatory          |

### MobileSDK.ApplePayResult

| Name             | Definition                                       | Type                   | Mandatory/Optional |
| :--------------- | :----------------------------------------------- | :------------          |:-----------------  |
| ottToken         |  The OTT token created using Apple Pay wallet.   | String                 | Mandatory          |
| cardInfo         |  Info on the selected user's card.               | ApplePayOTTCardInfo    | Optional           |
| shippingAddress  |  Info on the user's shipping address.            | ApplePayOTTShipping    | Optional           |
| billingAddress   |  Info on the user's billing address.             | ApplePayOTTBilling     | Optional           |

### MobileSDK.ApplePayOTTCardInfo

| Name             | Definition                                       | Type                   | Mandatory/Optional |
| :--------------- | :----------------------------------------------- | :------------          |:-----------------  |
| cardScheme       | Card scheme selected for payment.                | String                 | Mandatory          |

### MobileSDK.ApplePayOTTShipping

| Name             | Definition                                       | Type                   | Mandatory/Optional |
| :--------------- | :----------------------------------------------- | :------------          |:-----------------  |
| addressLine1     |  User's shipping address line 1                  | String                 | Optional           |
| addressLine2     |  User's shipping address line 2                  | String                 | Optional           |
| addressCountry   |  User's shipping address country                 | String                 | Optional           |
| addressCity      |  User's shipping address city                    | String                 | Optional           |
| addressPostcode  |  User's shipping address postcode                | String                 | Optional           |
| addressState     |  User's shipping address state                   | String                 | Optional           |
| contact          |  User's shipping address contact details         | ApplePayOTTContact     | Optional           |

### MobileSDK.ApplePayOTTContact

| Name             | Definition                                       | Type                   | Mandatory/Optional |
| :--------------- | :----------------------------------------------- | :------------          |:-----------------  |
| firstName        |  User's contact first name                       | String                 | Optional           |
| lastName         |  User's contact last name                        | String                 | Optional           |
| email            |  User's contact email                            | String                 | Optional           |
| phone            |  User's contact phone number                     | String                 | Optional           |

### MobileSDK.ApplePayOTTBilling

| Name             | Definition                                       | Type                   | Mandatory/Optional |
| :--------------- | :----------------------------------------------- | :------------          |:-----------------  |
| addressLine1     |  User's billing address line 1                  | String                 | Optional           |
| addressLine2     |  User's billing address line 2                  | String                 | Optional           |
| addressCountry   |  User's billing address country                 | String                 | Optional           |
| addressCity      |  User's billing address city                    | String                 | Optional           |
| addressPostcode  |  User's billing address postcode                | String                 | Optional           |
| addressState     |  User's billing address state                   | String                 | Optional           |

### MobileSDK.ApplePayResult

| Name             | Definition                                       | Type                   | Mandatory/Optional |
| :--------------- | :----------------------------------------------- | :------------          |:-----------------  |
| ottToken         |  The OTT token created using Apple Pay wallet.   | String                 | Mandatory          |
| cardInfo         |  Info on the selected user's card.               | ApplePayOTTCardInfo    | Optional           |
| shippingAddress  |  Info on the user's shipping address.            | ApplePayOTTShipping    | Optional           |
| billingAddress   |  Info on the user's billing address.             | ApplePayOTTBilling     | Optional           |

### MobileSDK.ApplePayError

| Name                        | Description                                                                                      | Error Result            |
| :-------------------------- | :----------------------------------------------------------------------------------------------- | :---------------------- |
| notSupported                |  Error thrown when Apple Pay is not supported.                                                   |  nil                    |
| noSupportedCardsInWallet    |  Error thrown when there are no supported cards in Apple Wallet for passed config.               |  nil                    |
| errorCreatingToken          |  Error thrown when there is an error while creating the token.                                   |  ErrorRes               |
| userCanceledPayment         |  Error thrown when user has canceled the payment flow.                                           |  nil                    |
| unableToPresentPaymentSheet |  Error thrown when there is an issue presenting the payment usually due to invalid merchant ID.  |  nil                    |
| payloadEncodingFailed       |  Error thrown when failing to encode Apple Pay payment data                                      |  nil                    |
| unknownError                |  Error thrown when there is an unknown error related to ApplePay.                                |  RequestError?          |


## Widget Styling

Defines the visual appearance for the `ApplePayWidget`. It handles customizing the button type and style.

### Appearance Contract

The `ApplePayWidgetAppearance` class encapsulates the configurable style properties for the widget.

```Swift
public struct ApplePayWidgetAppearance {
    public var type: PKPaymentButtonType
    public var style: PKPaymentButtonStyle
    public var cornerRadius: CGFloat?
}
```

### Default Appearance & Customisation

A default appearance is provided by `GlobalTheme` default values. This configures the button appearance.

#### Using Default Appearance


```Swift
    ApplePayWidget( 
        ...
        appearance: ApplePayWidgetAppearance = ApplePayWidgetAppearance() // Uses the default appearance
    )
```

#### Customising Appearance

You can create a custom `ApplePayWidgetAppearance` by providing a specific `PKPaymentButtonType`,`PKPaymentButtonStyle` and `cornerRadius` configuration.

```Swift
struct MyCustomApplePayScreen: View { 
    private func myCustomAppearance() -> ApplePayWidgetAppearance {
        let buttonType = PKPaymentButtonType.checkout
        let buttonStyle = PKPaymentButtonStyle.white
        
        let appearance = ApplePayWidgetAppearance(type: buttonType, style: buttonStyle, cornerRadius: 8.0)
        return appearance
    }
    
    var body: some View {
        ApplePayWidget( 
            ...
            appearance: ApplePayWidgetAppearance = myCustomAppearance()
            ...
        )
    }
}
```

### Style Attributes

The following attributes can be configured within `ApplePayWidgetAppearance`:

 Name                | Description                                                    | Type                               | Default Value (from `GlobalTheme`)  |
-------------------- | -------------------------------------------------------------- | -----------------------------------|-------------------------------------|
 `type`              | Defines the text within the button                             | PKPaymentButtonType                | `.plain`                            |
 `style`             | Defines the color scheme of the button.                        | PKPaymentButtonStyle               | `.automatic`                        |
 `cornerRadius`      | Defines the corner radius of the button.                       | CGFloat                            | 4.0                                 |
 
### WidgetEventDelegate

This `eventDelegate` allows the calling app to receive notifications of user interactions within the widget, such as button clicks. This is useful for analytics and tracking purposes.

```Swift
protocol WidgetEventDelegate {
    /**
     * Called when a widget event occurs.
     *
     * @param event The event that occurred, containing the event type and properties.
     */
    fun widgetEvent(event: Event)
}
```

##### ApplePay Events

The ApplePay Widget triggers the following events:

**ApplePay Start Checkout Event** - Triggered when the ApplePay checkout button is clicked:

```json
{
  "type": "Button",
  "properties": {
    "name": "ApplePayCheckoutButton",
    "action": "click"
  }
}
```
