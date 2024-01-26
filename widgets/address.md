# Standalone Address Widget

The Address capture Widget uses a prebuilt form to capture an address. Your customer can prefill the form with either an address or a partial address if available. They can then use the autocomplete field to look up their address by searching for key parts such as their postcode, zip code and so on.

## Overview

The Address capture Widget captures the following fields. These fields are mandatory for your customer to add, and optional for the merchant to provide.

| Name         | Definition                           | Mandatory/Optional                               |
| ------------ | ------------------------------------ | ------------------------------------------------ |
| firstName    | First name of the customer           | Optional to provide / Mandatory for the customer |
| lastName     | Last name of the customer            | Optional to provide / Mandatory for the customer |
| addressLine1 | First line of the customer's address | Optional to provide / Mandatory for the customer |
| addressLine2 | Secondary optional address line      | Optional to provide and for the customer         |
| city         | City of residence of the customer    | Optional to provide / Mandatory for the customer |
| state        | State of residence of the customer   | Optional to provide / Mandatory for the customer |
| postcode     | Address postcode of the customer     | Optional to provide / Mandatory for the customer |
| country      | Country of residence of the customer | Optional to provide / Mandatory for the customer |

## iOS

### Initializing the Address widget sheet in iOS

The Address Capture Widget collects your customer's billing location. This standalone widget manages all required inputs and validations, and provides address autocomplete using Apple MapKit SDK.

The Address Widget displays as a SwiftUI sliding bottom sheet. Once your customer has confirmed their address, the widget returns the Address object back to the application.

The Address Widget view is as follows:

```Swift
AddressSheetView(address: Address, isPresented: Binding<Bool>, onCompletion: Binding<MobileSDK.Address>)
```

The following example demonstrates the Widget before any customer details have been populated. It is in a SwiftUI View:

```Swift
import SwiftUI
import MobileSDK

struct AddressExampleView: View {
    var body: some View {
        AddressWidget { result in
            switch result {
            case .success(let address):
                // Handle the address result
            case .failure(let error):
                // Handle the error
            }
        }
    }
}
```

### Definitions

#### MobileSDK.AddressWidget

| Name       | Definition                                                                                           | Type                            | Mandatory/Optional |
| :--------- | :--------------------------------------------------------------------------------------------------- | :------------------------------ | :----------------- |
| address    | Used for passing in the information to the address sheet to pre-populate the address fields          | MobileSDK.Address               | Optional           |
| completion | Completion block that returns result. Either Address in case of success, or Error in case of failure | Result<Address, Error>) -> Void | Mandatory          |

#### MobileSDK.Address

| Name         | Definition                           | Type         | Mandatory/Optional                           |
| :----------- | :----------------------------------- | :----------- | :------------------------------------------- |
| firstName    | First name of the customer           | Swift.String | Optional to provide / Mandatory for the user |
| lastName     | Last name of the customer            | Swift.String | Optional to provide / Mandatory for the user |
| addressLine1 | Full address line with               | Swift.String | Optional to provide / Mandatory for the user |
| addressLine2 | Secondary optional address line      | Swift.String | Optional to provide and for the user         |
| city         | City of residence of the customer    | Swift.String | Optional to provide / Mandatory for the user |
| state        | State of residence of the customer   | Swift.String | Optional to provide / Mandatory for the user |
| postcode     | Address postcode of the customer     | Swift.String | Optional to provide / Mandatory for the user |
| country      | Country of residence of the customer | Swift.String | Optional to provide / Mandatory for the user |

## Android

### How to use the AddressDetailsWidget

This section provides a step-by-step guide on how to initialize and use the `AddressDetailsWidget` composable in your application. The widget allows for capturing billing address details.

The following sample code demonstrates the definition of the `AddressDetailsWidget`:

```Kotlin
@Composable
fun AddressDetailsWidget(
    modifier: Modifier,
    address: BillingAddress?,
    completion: (BillingAddress) -> Unit
) {...}
```

The following sample code example demonstrates how to use the widget in your application:

```Kotlin
// Initialize the AddressDetailsWidget
AddressDetailsWidget(
   modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp), // optional
    address = null, // optional (pre-populate fields)
    completion = { billingAddress ->
        // Handle captured address - Update UI or perform actions
       Log.d("CardDetailsWidget", "Billing address returned. $billingAddress")
    }
)
```

### Definitions

This subsection describes the various parameters required by the `AddressDetailsWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `AddressDetailsWidget`.

#### AddressDetailsWidget

| Name       | Definition                                              | Type                                                      | Mandatory/Optional |
| :--------- | :------------------------------------------------------ | :-------------------------------------------------------- | :----------------- |
| modifier   | Compose modifier for container modifications            | `Modifier`                                                | Optional           |
| address    | The preset address to pre-fill the input fields.        | `com.paydock.feature.address.domain.model.BillingAddress` | Optional           |
| completion | Callback function to execute when the address is saved. | `(BillingAddress) -> Unit`                                | Mandatory          |

#### BillingAddress

| Name         | Definition                                                                         | Type   | Mandatory/Optional                           |
| :----------- | :--------------------------------------------------------------------------------- | :----- | :------------------------------------------- |
| addressLine1 | The first line of the billing address, typically containing street information     | String | Optional to provide / Mandatory for the user |
| addressLine2 | The optional second line of the billing address, often used for additional details | String | Optional to provide and for the user         |
| city         | The city or locality of the billing address                                        | String | Optional to provide / Mandatory for the user |
| state        | The state or region of the billing address                                         | String | Optional to provide / Mandatory for the user |
| postalCode   | The Postal or ZIP code of the billing address                                      | String | Optional to provide / Mandatory for the user |
| country      | Country of residence of the user                                                   | String | Optional to provide / Mandatory for the user |

### Callback Explanation

#### Completion Callback

The `completion` callback is invoked after the address details is captured. It receives a `BillingAddress` once the address details have been saved.
