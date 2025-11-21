# Apple Pay Widget

> 
>
>Use the iOS SDK to make payments using ApplePay. 

The logic of this widget exists as a single view that you can initialise and embed in your payment flow.

## How to use the ApplePay widget

> **Note**:
>
> When the customer taps the Payment button for your Digital Wallet, the SDK triggers a callback to the Merchant app requesting a `wallet_token`. You must perform a wallet initialization request to receive your `wallet_token`. To do this, follow the instructions in the [generate a wallet_token](/digital-wallet-widgets/wallettoken.md) section.  

1. Use the following to initialise the **ApplePayView**:

```Swift
ApplePayWidget(
    appearance: ApplePayWidgetAppearance = ApplePayWidgetAppearance(),
    eventDelegate: WidgetEventDelegate? = nil,
    createPaymentRequest: @escaping (_ createPaymentRequestResult: @escaping (Result<ApplePayRequestResult, ApplePayRequestError>) -> Void) -> Void,
    completion: @escaping (Result<ChargeResponse, ApplePayError>) -> Void
    ) { ... }
```

If the charge is successful, the **ApplePayView** returns a `ChargeResponse` that contains all the relevant information. In case of an error, the `ApplePayError` object is returned with information regarding the failure.

2. The **ApplePayRequest** contains all of the required data to enable ApplePay.

```Swift
public struct ApplePayRequest {
    public let token: String
    public let request: PKPaymentRequest

    public init(token: String, request: PKPaymentRequest) {
        self.token = token
        public let request: PKPaymentRequest
}
```

3. The Mobile SDK provides convenient helper methods to initialise the `PKPaymentRequest` object. You can configure this in various ways, depending on your use case:

```Swift
    public static func createApplePayRequest(
        amount: Decimal,
        amountLabel: String,
        countryCode: String,
        currencyCode: String,
        merchantIdentifier: String,
        merchantCapabilities: PKMerchantCapability = [.credit, .debit, .threeDSecure],
        supportedNetworks: [PKPaymentNetwork] = [.visa, .masterCard, .amex, .discover],
        requireBillingAddress: Bool = true,
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
            paymentRequest.requiredShippingContactFields = requireShippingAddress ? [.phoneNumber, .emailAddress, .postalAddress, .name] : []
            paymentRequest.shippingMethods = shippingOptions
            return paymentRequest
    }
```

4. The following is an example of the full **ApplePayView** initialisation. Ensure that you have your `wallet_token`, which you can generate by following the instructions [here](/digital-wallet-widgets/wallettoken):

```Swift
struct ApplePayExampleView: View {
    var body: some View {
        ApplePayWidget { onApplePayButtonTap in
            onApplePayButtonTap(<Your ApplePayRequest>)
        } completion: { result in 
            switch result {
                case .success(let chargeResponse): // Handle successful result
                case .failure(let error): // Handle error
            }
        }
    }
}

func getApplePayRequest() -> ApplePayRequest {
    let paymentRequest = MobileSDK.createApplePayRequest(
        amount: 0.01,
        amountLabel: "Amount",
        countryCode: "AU",
        currencyCode: "AUD")

    let applePayRequest = ApplePayRequest(
        token: <Merchant wallet token>,
        request: paymentRequest)

    return applePayRequest
}
```

### Parameter Definitions

| Name                  | Definition                                                                                           | Type                                                                                                                  | Mandatory/Optional |
| :-------------------- | :--------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------- | :----------------- |
| appearance            |  Object for visual customization of the widget.                                                      | `MobileSDK.ApplePayWidgetAppearance`                                                                                  | Optional           |
| eventDelegate         |  Delegate for handling widget events such as button clicks.                                          | `MobileSDK.WidgetEventDelegate`                                                                                       | Optional           |
| completion            |  Result callback with the Charge creation API response if successful, or an error if unsuccessful.   | `(Result<ChargeResponse, ApplePayError>) -> Void)`                                                                    | Mandatory          |

#### Token Callback

> **Note**:
>
> The `createPaymentRequest` callback obtains the wallet token asynchronously. It receives a callback function `(_ createPaymentRequestResult: @escaping (Result<ApplePayRequestResult, ApplePayRequestError>) -> Void) -> Void` as a parameter, which you must invoke with the success or failure once it is obtained. To obtain the `wallet_token`, follow the instructions in the [generate a wallet_token](/digital-wallet-widgets/wallettoken.md) section of this guide. 

## Definitions

More in depth definitions of the parameters, as well as potential errors and responses, are as follows:

### MobileSDK.ApplePayRequestResult
| Name               | Definition                                                                                                               | Type                     | Mandatory/Optional |
| :----------------- | :----------------------------------------------------------------------------------------------------------------------- | :----------------------- | :----------------  |
| request            |  Data for configuration of Apple Pay. Can be built manually or using the helper method MobileSDK.createApplePayRequest() | PassKit.PKPaymentRequest | Mandatory          |
| token              |  Wallet charge token that is generated by merchant backend                                                               | Swift.String             | Mandatory          |

### MobileSDK.ChargeResponse
| Name     | Definition                                       | Type          | Mandatory/Optional |
| :------- | :----------------------------------------------- | :------------ | :----------------  |
| status   |  Status of an ApplePay payment after completion. | Swift.String  | Mandatory          |
| amount   |  The amount of money that was charged            | Swift.Decimal | Mandatory          |
| currency |  Currency of the charge.                         | Swift.String  | Mandatory          |

### MobileSDK.ApplePayError

| Name                         | Description                                                                                     | Error Result            |
| :-------------------------- | :----------------------------------------------------------------------------------------------- | :---------------------- |
| invalidApplePayRequest      |  Error thrown when provided ApplePayRequest object is not valid.                                 |  nil                    |
| errorInitializingPayment    |  Error thrown when there is an error initializing payment request.                               |  nil                    |
| errorCompletingPayment      |  Error thrown when there is an error while capturing the charge.                                 |  ErrorRes               |
| userCanceledPayment         |  Error thrown when user has canceled the payment flow.                                           |  nil                    |
| unableToPresentPaymentSheet |  Error thrown when there is an issue presenting the payment usually due to invalid merchant ID.  |  ErrorRes               |
| creatingPaymentRequest      |  Error thrown when merchant app has failed to create a payment request.                          |  String                 |
| unknownError                |  Error thrown when there is an unknown error related to ApplePay.                                |  nil                    |


## Widget Styling

Defines the visual appearance for the `ApplePayWidget`. It handles customizing the button type and style.

### Appearance Contract

The `ApplePayWidgetAppearance` class encapsulates the configurable style properties for the widget.

```Swift
public struct ApplePayWidgetAppearance {
    public var type: PKPaymentButtonType
    public var style: PKPaymentButtonStyle
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

You can create a custom `ApplePayWidgetAppearance` by providing a specific `PKPaymentButtonType` and `PKPaymentButtonStyle` configuration.

```Swift
struct MyCustomApplePayScreen: View { 
    private func myCustomAppearance() -> ApplePayWidgetAppearance {
        let buttonType = PKPaymentButtonType.checkout
        let buttonStyle = PKPaymentButtonStyle.white
        
        let appearance = ApplePayWidgetAppearance(type: buttonType, style: buttonStyle)
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
 `type`              | Defines the text within the button                             | `PassKit.PKPaymentButtonType`      | `.plain`                            |
 `style`             | Defines the color scheme of the button.                        | `PassKit.PKPaymentButtonStyle`     | `.automatic`                        |
 
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
