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

### How to initialise the Address widget sheet in iOS

### 1. Overview

The Address Capture Widget collects your customer's billing location. As a standalone widget it manages all required inputs and validations, and provides an address autocomplete function using Apple MapKit SDK.

The widget displays as a SwiftUI sliding bottom sheet. Once your customer has confirmed their address, the widget returns the Address object back to the application.

The Address Widget view is as follows:

```Swift
AddressSheetView(
    config: AddressWidgetConfig,
    appearance: AddressWidgetAppearance = AddressWidgetAppearance(),
    completion: @escaping (Address) -> Void)
    { ... }
``` 

An example of the widget before any customer details have been populated is as follows. The widget is in a SwiftUI View:

```Swift
import SwiftUI
import MobileSDK

struct AddressExampleView: View {
    var body: some View {
        AddressWidget(
            config: AddressWidgetConfig(),
            appearance: AddressWidgetAppearance()) { address in
                // Handle received address object
        }
    }
}
```
### 2. Parameter definitions

The following definitions provide a more detailed overview of the parameters included in the Address widget:

#### MobileSDK.AddressWidget
| Name          | Definition                                                                                            | Type                             | Mandatory/Optional |
| :------------ | :---------------------------------------------------------------------------------------------------- | :------------------------------- | :----------------  |
| config        |  Used for configuration of the widget behaviour                                                       | `MobileSDK.AddressWidgetConfig`  | Mandatory          |
| appearance    |  Customization options for the visual appearance of the widget                                        | `AddressWidgetAppearance`        | Optional           |

#### MobileSDK.AddressWidgetConfig
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

### 3. Callback Explanation

#### Completion Callback

The `completion` callback is invoked after the address details is captured. It receives an `Address` once the address details have been saved.

### 4. Widget Styling

Defines the visual appearance and layout attributes for the `AddressWidget`. This allows for extensive customisation of how the address input fields and action button are presented to the user.

#### Appearance Contract

The `AddressWidgetAppearance` class encapsulates all configurable style properties for the widget.

```Swift
public struct AddressWidgetAppearance {
    public var horizontalSpacing: CGFloat
    public var verticalSpacing: CGFloat
    public var title: Theme.TextAppearance
    public var textField: Theme.TextFieldAppearance
    public var actionButton: Theme.ButtonAppearance
    public var expandSectionButton: Theme.ButtonAppearance
    public var searchDropdown: Theme.SearchDropdownAppearance
}
```

#### Default Appearance & Customisation

A default appearance is provided by `GlobalTheme` values for each of the subcomponents. You can use this as a starting point and customise specific attributes as needed.

##### Using Default Appearance

```Swift
    AddressWidget( 
        ...
        appearance: AddressWidgetAppearance = AddressWidgetAppearance() // Uses the default appearance
    )
```

##### Customising Appearance

You can create a custom `AddressWidgetAppearance` or modify the default one.

```Swift
struct MyCustomAddressScreen: View { 
    private func myCustomAppearance() -> AddressWidgetAppearance {
        let title = Theme.TextAppearance.init(text: .init(font: .init(name: "CustomFont", size: 20), isUnderlined: true, underlineColor: .black))
        let textField = Theme.TextFieldAppearance.init(colors: .init(active: .blue), dimensions: .init(cornerRadius: 20.0))
        let actionButton = Theme.ButtonAppearance.init(colors: .init(background: .blue))
        let appearance = AddressWidgetAppearance(
            verticalSpacing: 10,
            horizontalSpacing: 16,
            title: title,
            textField: textField,
            actionButton: actionButton)
        return appearance
    }
    
    var body: some View {
            AddressWidget( 
            ...
            appearance: AddressWidgetAppearance = myCustomAppearance()
            ...
        )
    }
}
```

#### Style Attributes

The following attributes can be configured within `AddressWidgetAppearance`:

| Name                   | Description                                                                                                | Type                           | Default Value                     |
|------------------------|------------------------------------------------------------------------------------------------------------|--------------------------------|-----------------------------------|
| `verticalSpacing`      | The vertical space between the groups of different elements on the screen.                                 | `CGFloat`                      | `16.0`                            |
| `horizontalSpacing`    | The horizontal space between the card number input field and the PIN input field within their shared row.  | `CGFloat`                      | `8.0`                             |
| `title`                | Defines the appearance of the section titles on the screen.                                                | `TextAppearance`               | `GlobalTheme.title`               |
| `textField`            | Defines the appearance of the card number and PIN input text fields.                                       | `TextFieldAppearance`          | `GlobalTheme.textField`           |
| `actionButton`         | Defines the appearance of the primary submit button.                                                       | `ButtonAppearance`             | `GlobalTheme.actionButton`        |
| `expandSectionButton`  | Defines the appearance of manual entry button.                                                             | `ButtonAppearance`             | `GlobalTheme.expandSectionButton` |
| `searchDropdown`       | Defines the appearance of the address search dropdown texfield and picker.                                 | `SearchDropdownAppearance`     | `GlobalTheme.searchDropdown`      |


---

**Note:**
*   The `TextAppearance`, `TextFieldAppearance`, `ButtonAppearance` and `SearchDropdownAppearance` themselves would have their own detailed documentation explaining their configurable attributes (like colors, typography, borders, etc.). This documentation focuses on how they are composed within the `AddressWidgetAppearance`.

## Android

### How to use the AddressDetailsWidget

### 1. Overview

The Address widget captures billing address details. This section details how to initialise and use the `AddressDetailsWidget` composable in your application. 

The following sample code demonstrates the definition of the `AddressDetailsWidget`:

```Kotlin
@Composable
fun AddressDetailsWidget(
    modifier: Modifier = Modifier,
    config: AddressDetailsWidgetConfig,
    appearance: AddressDetailsWidgetAppearance = AddressDetailsAppearanceDefaults.appearance(),
    address: BillingAddress? = null,
    eventDelegate: WidgetEventDelegate? = null,
    completion: (BillingAddress) -> Unit,
) {...}
```

The following sample code example demonstrates how to use the widget in your application:

```Kotlin
// Initialise the AddressDetailsWidget
AddressDetailsWidget(
   modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp), // optional
    config = AddressDetailsWidgetConfig(
        address = null // optional - pre-populate address fields
    ),
    appearance = currentOrDefaultAppearance, // optional
    eventDelegate = EVENT_DELEGATE_INSTANCE, // optional - Delegate class to handle events
    completion = { billingAddress ->
        // Handle captured address - Update UI or perform actions
       Log.d("AddressDetailsWidget", "Billing address returned. $billingAddress")
    }
)
```

### 2. Parameter definitions

This subsection describes the various parameters required by the `AddressDetailsWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `AddressDetailsWidget`.

#### AddressDetailsWidget

| Name             | Definition                                                         | Type                                                      | Mandatory/Optional |
| :--------------- | :----------------------------------------------------------------- | :-------------------------------------------------------- | :----------------  |
| modifier         |  Compose modifier for container modifications                      | `Modifier`                                                | Optional           |
| config           |  Configuration options for the address details widget              | `AddressDetailsWidgetConfig`                              | Mandatory          |
| appearance       |  Customization options for the visual appearance of the widget     | `AddressDetailsWidgetAppearance`                          | Optional           |
| address          |  The preset address to pre-fill the input fields.                  | `com.paydock.feature.address.domain.model.BillingAddress` | Optional           |
| eventDelegate    |  Delegate for handling widget events such as button clicks.        | `WidgetEventDelegate`                                     | Optional           |
| completion       |  Callback function to execute when the address is saved.           | `(BillingAddress) -> Unit`                                | Mandatory          |

#### AddressDetailsWidgetConfig

Configuration data class for the Address Details Widget. This class holds the necessary parameters to initialize and display the address details form within the Paydock SDK.

| Name             | Definition                                                                                            | Type                                                      | Mandatory/Optional |
| :--------------- | :---------------------------------------------------------------------------------------------------- | :-------------------------------------------------------- | :----------------  |
| address          |  An optional [BillingAddress] object to pre-populate the address form fields. If null, the form will be displayed with empty fields. | `BillingAddress?`                                         | Optional           |

#### BillingAddress
| Name          | Definition                                                                           | Type       | Mandatory/Optional                           |
| :------------ | :----------------------------------------------------------------------------------- | :--------- | :------------------------------------------  |
| firstName     |  The first name of the user associated with the billing address                      | String     | Optional to provide and for the user         |
| lastName      |  The last name of the user associated with the billing address                       | String     | Optional to provide and for the user         |
| name          |  The name associated with the billing address                                        | String     | Optional to provide and for the user         |
| addressLine1  |  The first line of the billing address, typically containing street information      | String     | Optional to provide / Mandatory for the user |
| addressLine2  |  The optional second line of the billing address, often used for additional details  | String     | Optional to provide and for the user         |
| city          |  The city or locality of the billing address                                         | String     | Optional to provide / Mandatory for the user |
| state         |  The state or region of the billing address                                          | String     | Optional to provide / Mandatory for the user |
| postalCode    |  The Postal or ZIP code of the billing address                                       | String     | Optional to provide / Mandatory for the user |
| country       |  The country of the billing address                                                  | String     | Optional to provide / Mandatory for the user |
| phoneNumber   |  The phone number associated with the billing address                                | String     | Optional to provide and for the user         |

### 3. Callback Explanation

#### WidgetEventDelegate

This `eventDelegate` allows the calling app to receive notifications of user interactions within the widget, such as button clicks. This is useful for analytics and tracking purposes.

```Kotlin
interface WidgetEventDelegate {
    /**
     * Called when a widget event occurs.
     *
     * @param event The event that occurred, containing the event type and properties.
     */
    fun widgetEvent(event: Event)
}
```

##### Address Details Events

The Address Details Widget triggers the following events:

**Save Event** - Triggered when the save button is clicked:

```json
{
  "type": "Button",
  "properties": {
    "name": "SaveButton",
    "action": "click",
    "text": "(Whatever is set by merchant)"
  }
}
```

**Manual Entry Event** - Triggered when the manual entry button is clicked:

```json
{
  "type": "Button",
  "properties": {
    "name": "ManualEntryButton",
    "action": "click"
  }
}
```

| Property | Description | Type | Optional/Required |
|----------|-------------|------|-------------------|
| `type` | The type of UI element that triggered the event | String | Required |
| `properties.name` | The name identifier of the specific element | String | Required |
| `properties.action` | The action performed on the element | String (Enum) | Required |
| `properties.text` | The text content of the element (Save Event only) | String | Optional |

#### Completion Callback

The `completion` callback is invoked after the address details is captured. It receives a `BillingAddress` once the address details have been saved.

### 4. Widget Styling

Defines the visual appearance and layout attributes for the `AddressDetailsWidget`. This class allows for extensive customisation of the address input form, including the address search functionality, manual input fields, title, and action buttons.

#### Appearance Contract

The `AddressDetailsWidgetAppearance` class encapsulates all configurable style properties for the widget.

```Kotlin
@Immutable
class AddressDetailsWidgetAppearance(
    val verticalSpacing: Dp,
    val title: TextAppearance,
    val textField: TextFieldAppearance,
    val actionButton: ButtonAppearance,
    val linkButton: LinkButtonAppearance,
    val searchDropdown: SearchDropdownAppearance
)
```

#### Default Appearance & Customisation

A default appearance is provided by `AddressDetailsAppearanceDefaults`. This uses `MaterialTheme` values and predefined component defaults for a standard look and feel. You can use this as a starting point and customise specific attributes.

##### Using Default Appearance


```Kotlin
    AddressDetailsWidget( 
        ...
        appearance = AddressDetailsAppearanceDefaults.appearance() // Uses the default appearance
    )
```

##### Customising Appearance

You can create a custom `AddressDetailsWidgetAppearance` or modify the default one using its `copy` method (if your class has one, as seen in the initially provided context).

```Kotlin
@Composable 
fun MyCustomAddressScreen() { 
    // Create appearance by using provided defaults, with custom changes
    val customAppearance = defaultFromContext.copy( // Assuming .copy() exists as per context
        verticalSpacing = 12.dp,
        title = TextAppearanceDefaults.appearance().copy(
            style = MaterialTheme.typography.headlineSmall.copy(
                // color = MyAppColors.primary
            )
        ),
        textField = TextFieldAppearanceDefaults.outlineAppearance().copy(
            // Example: Customizing text field label color
            // labelColor = MyAppColors.onSurfaceVariant
        ),
        linkButton = LinkButtonAppearanceDefaults.appearance().copy(
            // Example: Customizing link button text style
            // textStyle = MaterialTheme.typography.labelSmall
        )
    )

    // Alternatively, create entirely from scratch:
    val completelyCustomAppearance = AddressDetailsWidgetAppearance(
        verticalSpacing = 10.dp,
        title = TextAppearanceDefaults.appearance(/*...custom title params...*/),
        textField = TextFieldAppearanceDefaults.filledAppearance(/*...custom text field params...*/),
        actionButton = ButtonAppearanceDefaults.textButtonAppearance(/*...custom button params...*/),
        linkButton = LinkButtonAppearanceDefaults.appearance(/*...custom link button params...*/),
        searchDropdown = SearchDropdownAppearanceDefaults.appearance(/*...custom search params...*/)
    )

    AddressDetailsWidget(
        ...
        appearance = customAppearance, // Use your custom appearance
    )
}
```

#### Style Attributes

The following attributes can be configured within `AddressDetailsWidgetAppearance`:

 Name                | Description                                                                                                                                  | Type                                                            | Default Value (from `AddressDetailsAppearanceDefaults`)                                        |
---------------------|----------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|------------------------------------------------------------------------------------------------|
 `verticalSpacing`   | The vertical space between major elements in the widget (e.g., between search section, manual entry link, input fields, and action button).  | `androidx.compose.ui.unit.Dp`                                   | `WidgetDefaults.Spacing`                                                                       |
 `title`             | Defines the text appearance for section titles, such as the title for the address search section.                                            | `TextAppearance`                   | `TextAppearanceDefaults.appearance()` with `titleMedium` style.                                |
 `textField`         | Defines the appearance for all manual address input text fields (e.g., street, city, postal code).                                           | `TextFieldAppearance`               | `TextFieldAppearanceDefaults.appearance().copy(singleLine = true)`                             |
 `actionButton`      | Defines the appearance of the primary "Save Address" button.                                                                                 | `ButtonAppearance`             | `ButtonAppearanceDefaults.filledButtonAppearance()`                                            |
 `linkButton`        | Defines the appearance for interactive link-style buttons, such as "Enter Address Manually".                                                 | `LinkButtonAppearance`             | `LinkButtonAppearanceDefaults.appearance()`                                                    |
 `searchDropdown`    | Defines the appearance of the address search input field and its associated dropdown results list.                                           | `SearchDropdownAppearance'         | `SearchDropdownAppearanceDefaults.appearance()`                                                |

---

**Note:**
*   The appearance types (`TextAppearance`, `TextFieldAppearance`, etc.) would each have their own detailed documentation.
