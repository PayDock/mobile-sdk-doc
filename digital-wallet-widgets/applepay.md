# ApplePay Widget

Use the iOS SDK to make payments using ApplePay. The logic of this widget exists as a single view that you can initialise and embed in your payment flow.

![ApplePay View](/img/ApplePay.png)

## How to use the ApplePay widget

> **Note**:
>
> When the customer taps the Payment button for your Digital Wallet, the SDK triggers a callback to the Merchant app requesting a `wallet_token`. You must perform a wallet initialization request to receive your `wallet_token`. To do this, follow the instructions in the [generate a wallet_token](/digital-wallet-widgets/wallettoken.md) section.  

1. Use the following to initialise the **ApplePayView**:

```Swift
ApplePayWidget(
    applePayRequestHandler: @escaping (_ applePayRequest: @escaping (ApplePayRequest) -> Void) -> Void,
    completion: @escaping (Result<ChargeResponse, ApplePayError>) -> Void)
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
                case .success(let chargeResponse): // Handle successfull result
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
<<<<<<< HEAD:markdoc/pages/digital-wallet-widgets/applepay.mdoc
        currencyCode: "AUD",
        merchantIdentifier: <merchant_identifier>)

    let applePayRequest = ApplePayRequest(
        token: <wallet_token>,
        merchantIdentifier: <merchant_identifier>),
=======
        currencyCode: "AUD")

    let applePayRequest = ApplePayRequest(
        token: <Merchant wallet token>,
>>>>>>> main:markdoc/pages/widgets/applepay.mdoc
        request: paymentRequest)

    return applePayRequest
}
```

## Definitions

More in depth definitions of the parameters, as well as potential errors and responses, are as follows:

### MobileSDK.ApplePayRequest
| Name               | Definition                                                                                                               | Type                     | Mandatory/Optional |
| :----------------- | :----------------------------------------------------------------------------------------------------------------------- | :----------------------- | :----------------  |
| token              |  Wallet charge token that is generated by merchant backend                                                               | Swift.String             | Mandatory          |
| request            |  Data for configuration of Apple Pay. Can be built manually or using the helper method MobileSDK.createApplePayRequest() | PassKit.PKPaymentRequest | Mandatory          |

### MobileSDK.ChargeResponse
| Name     | Definition                                       | Type          | Mandatory/Optional |
| :------- | :----------------------------------------------- | :------------ | :----------------  |
| status   |  Status of an ApplePay payment after completion. | Swift.String  | Mandatory          |
| amount   |  The amount of money that was charged            | Swift.Decimal | Mandatory          |
| currency |  Currency of the charge.                         | Swift.String  | Mandatory          |

### MobileSDK.ApplePayError

| Name          | Error message                                    | Type  |
| :------------ | :----------------------------------------------- | :---- |
| initFailed    |  Initialisation of Apple Pay has failed          | Error | 
| paymentFailed |  Processing the payment has failed.              | Error |