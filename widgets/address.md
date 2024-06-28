# Standalone Address Widget

> 
>
> Use a prebuilt form to capture an address. 

## Overview

Paydock's Address Widget enables your customer to prefill a form with either an address or a partial address if available. They can then use the autocomplete field to look up their address by searching for key parts such as their postcode, zip code and so on.

![Address-search](/img/Address_search.png) 

![Address-manual](/img/Address_manual.png) 

The Address capture Widget captures the following fields. These fields are mandatory for your customer to add, and optional for the merchant to provide.   

| Name          | Definition                         | Mandatory/Optional                           |
| ------------- | ---------------------------------- | -------------------------------------------  |
| firstName     |  First name of the customer            | Optional to provide / Mandatory for the customer |
| lastName      |  Last name of the customer             | Optional to provide / Mandatory for the customer |
| addressLine1  |  First line of the customer's address  | Optional to provide / Mandatory for the customer |
| addressLine2  |  Secondary optional address line   | Optional to provide and for the customer         |
| city          |  City of residence of the customer     | Optional to provide / Mandatory for the customer |
| state         |  State of residence of the customer    | Optional to provide / Mandatory for the customer |
| postcode      |  Address postcode of the customer      | Optional to provide / Mandatory for the customer |
| country       |  Country of residence of the customer  | Optional to provide / Mandatory for the customer |

## iOS

### How to initialize the Address widget sheet in iOS

### 1. Overview

The Address Capture Widget collects your customer's billing location. As a standalone widget it manages all required inputs and validations, and provides an address autocomplete function using Apple MapKit SDK.

The widget displays as a SwiftUI sliding bottom sheet. Once your customer has confirmed their address, the widget returns the Address object back to the application.

The Address Widget view is as follows:

```Swift
AddressSheetView(address: Address, isPresented: Binding<Bool>, onCompletion: Binding<MobileSDK.Address>)
``` 

An example of the widget before any customer details have been populated is as follows. The widget is in a SwiftUI View:

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
### 2. Parameter definitions

The following definitions provide a more detailed overview of the parameters included in the Address widget:

#### MobileSDK.AddressWidget
| Name          | Definition                                                                                            | Type                             | Mandatory/Optional |
| :------------ | :---------------------------------------------------------------------------------------------------- | :------------------------------- | :----------------  |
| address       |  Used for passing in the information to the address sheet to pre-populate the address fields          | MobileSDK.Address                | Optional           |

#### MobileSDK.Address
| Name          | Definition                         | Type          | Mandatory/Optional                           |
| :------------ | :--------------------------------- | :------------ | :------------------------------------------  |
| firstName     |  First name of the customer            | Swift.String  | Optional to provide / Mandatory for the user |
| lastName      |  Last name of the customer             | Swift.String  | Optional to provide / Mandatory for the user |
| addressLine1  |  Full address line with            | Swift.String  | Optional to provide / Mandatory for the user |
| addressLine2  |  Secondary optional address line   | Swift.String  | Optional to provide and for the user         |
| city          |  City of residence of the customer     | Swift.String  | Optional to provide / Mandatory for the user |
| state         |  State of residence of the customer    | Swift.String  | Optional to provide / Mandatory for the user |
| postcode      |  Address postcode of the customer      | Swift.String  | Optional to provide / Mandatory for the user |
| country       |  Country of residence of the customer  | Swift.String  | Optional to provide / Mandatory for the user |


## Android

## How to use the AddressDetailsWidget

### 1. Overview

The Address widget captures billing address details. This section details how to initialize and use the `AddressDetailsWidget` composable in your application. 

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

### 2. Parameter definitions

This subsection describes the various parameters required by the `AddressDetailsWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `AddressDetailsWidget`.

#### AddressDetailsWidget

| Name             | Definition                                               | Type                                                      | Mandatory/Optional |
| :--------------- | :------------------------------------------------------- | :-------------------------------------------------------- | :----------------  |
| modifier         |  Compose modifier for container modifications            | `Modifier`                                                | Optional           |
| address          |  The preset address to pre-fill the input fields.        | `com.paydock.feature.address.domain.model.BillingAddress` | Optional           |
| completion       |  Callback function to execute when the address is saved. | `(BillingAddress) -> Unit`                                | Mandatory          |

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

### 2. Parameter definitions

This subsection describes the various parameters required by the `AddressDetailsWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `AddressDetailsWidget`.

#### AddressDetailsWidget

| Name             | Definition                                               | Type                                                      | Mandatory/Optional |
| :--------------- | :------------------------------------------------------- | :-------------------------------------------------------- | :----------------  |
| modifier         |  Compose modifier for container modifications            | `Modifier`                                                | Optional           |
| address          |  The preset address to pre-fill the input fields.        | `com.paydock.feature.address.domain.model.BillingAddress` | Optional           |
| completion       |  Callback function to execute when the address is saved. | `(BillingAddress) -> Unit`                                | Mandatory          |

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

### 3. Callback Explanation

#### Completion Callback

The `completion` callback is invoked after the address details is captured. It receives a `BillingAddress` once the address details have been saved.

