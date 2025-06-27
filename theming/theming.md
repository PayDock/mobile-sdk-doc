# Theming the SDK

Customise the appearance of your UI elements to match the look and feel of your app. This step is optional, and if you choose not to customise the theming parameters, the SDK's default values will be used.

Although the layout of these elements is fixed to remain consistent, you can modify colors, fonts, and other parameters such as corner radius and spacing.

You can specify both the Light and Dark mode themes upfront, and the SDK seamlessly handles switching between the two based on your customer's device settings.

The following image showcases the full capabilities of the MobileSDK's theming parameters. It is meant purely for demonstrative purposes, and is not an offering at this time.

![What you Can Theme](/img/Theming.png)

| Colours | Notes |
| ----- | ----- |
| primary | The primary brand color used throughout UI Elements (e.g. CTA Button Colour) |
| onPrimary | A color with a high contrast to the `primary` colour, used for elements that are displayed over a `primary` colour (e.g. CTA Button text) |
| background | The color used as the background for the sheet that UI elements are presented on |
| text | The default text color used throughout UI elements, appearing over the `background` color. |
| success | The color used to indicate success (e.g. Valid input) |
| error | The color used to indicate errors or destructive actions (e.g. Invalid input) |
| border | The color used for borders of input fields and tabs |
| placeholder | The color used for input placeholder text |

![Dimensions](/img/Dimensions.png)

| Dimensions | Notes |
| ----- | ----- |
| textFieldCornerRadius | The rounding applied to TextField Elements |
| buttonCornerRadius | The rounding applied to Button Elements |
| borderWidth | The thickness of borders of input fields and tabs |
| spacing | The spacing between individual views on UI Elements |

| Other | Notes |
| ----- | ----- |
| font | The font family used throughout UI Elements |

## iOS

### How to Customise the SDK visuals for iOS

Match the look and feel of the SDK to your app using the iOS SDK's visual customization capabilities. 

Easily tweak the SDK by passing the optional **Theme()** object when initializing the SDK. This enables customization of the color scheme, customization of the font and the dimensions of the elements.

When the **Theme()** object is not provided, the SDK uses it's default values.

The **Theme()** component is as follows:

```Swift
public struct Theme {
    var colors: Colors
    var dimensions: Dimensions
    var fontName: String
}
```

### Examples of SDK customization for iOS

The following examples showcase how the MobileSDK can be customised:

1. The following shows the default for the MobileSDK, and so there is no customization: 

```Swift
let config = MobileSDKConfig(environment: .staging)
MobileSDK.shared.configureMobileSDK(config: config)
```

2. The following example demonstrates how to customise all parameters::

```Swift
let colors = Colors(
    primary: Color.yourPrimaryColor,
    onPrimary: Color.yourOnPrimaryColor,
    text: Color.yourTextColor,
    success: Color.yourSuccessColor,
    error: Color.yourErrorColor,
    background: Color.yourBackgroundColor,
    border: Color.yourBorderColor,
    placeholder: Color.yourPlaceholderColor)

let dimensions = Dimensions(
    cornerRadius:  8,
    borderWidth: 1,
    spacing: 20)

let fontName = "Avenir-LightOblique"

let theme = Theme(
    colors: colors, 
    dimensions: dimensions, 
    fontName: fontName)

let config = MobileSDKConfig(environment: .staging, theme: theme)
MobileSDK.shared.configureMobileSDK(config: config)
```

3. The following example demonstrates how to customise colors only:

```Swift
let colors = Colors(
    primary: Color.yourPrimaryColor,
    onPrimary: Color.yourOnPrimaryColor,
    text: Color.yourTextColor,
    success: Color.yourSuccessColor,
    error: Color.yourErrorColor,
    background: Color.yourBackgroundColor,
    border: Color.yourBorderColor,
    placeholder: Color.yourPlaceholderColor)

let theme = Theme(colors: colors)

let config = MobileSDKConfig(environment: .staging, theme: theme)
MobileSDK.shared.configureMobileSDK(config: config)
```

4. The following demonstrates how to customise the dimensions only:

```Swift
let dimensions = Dimensions(
    cornerRadius:  8,
    borderWidth: 1,
    spacing: 20)

let theme = Theme(dimensions: dimensions)

let config = MobileSDKConfig(environment: .staging, theme: theme)
MobileSDK.shared.configureMobileSDK(config: config)

```
5. The following demonstrates customizing fonts only:

```Swift
let fontName = "Avenir-LightOblique"
let theme = Theme(fontName: fontName)
let config = MobileSDKConfig(environment: .staging, theme: theme)
MobileSDK.shared.configureMobileSDK(config: config)
```

### Recommended way of providing colors to the SDK

The Mobile SDK supports light mode, dark mode, and high-contrast accessibility colors.
To take full advantage of this support, it is recommended to create Color Assets in your app's Asset Catalog and provide them in the **Theme()** object. While you can also provide colors directly to the **Theme()** object without using the Asset Catalog, this approach will not support dark mode or high-contrast accessibility.

Each color you define should include appropriate color variants for different traits.

Below is an example of a default color used by the Mobile SDK for error messages across screens:

![Color Assets](/img/ColorAssets.png)

### Definitions

#### MobileSDK.Theme

| Name              | Definition                                                                 | Type                 |
| :---------------- | :------------------------------------------------------------------------- | :------------------- |
| colors            |  Color theme containing ligh mode, dark mode and high contrast traits.     | MobileSDK.Colors     |
| dimensions        |  The dimension values for various UI elements                              | MobileSDK.Dimensions |
| fontName          |  Name of the font to be used within the theme.                             | Swift.String         |

#### MobileSDK. Colors
| Name              | Definition                                                                                                           | Type          |
| :---------------- | :--------------------------------------------------------------------------------------------------------------------| :-------------|
| primary           |  The primary color that is displayed over active elements such as buttons, active text fields, dropdown menus etc.   | SwiftUI.Color |
| onPrimary         |  Color used for text and icons displayed on top of primary color.                                                    | SwiftUI.Color |
| text              |  The color that is used for the text that is input into the field by the user.                                       | SwiftUI.Color |
| success           |  The success color is used to indicate success in components such as text fields, icons, validations etc.            | SwiftUI.Color |
| error             |  The error color is used to indicate error state in components such as textfield for cases when validations fail.    | SwiftUI.Color |
| background        |  This color is used for the background of the widget bottom sheet on which all other components are placed.          | SwiftUI.Color |
| border            |  Used as a color for textfield outline border and separators.                                                        | SwiftUI.Color |
| placeholder       |  Color used for text and icons displayed on the background as well as textfield labels and placeholders.             | SwiftUI.Color |

#### MobileSDK.Dimensions
| Name                  | Definition                                                                                                                                                          | Type                   |
| :-------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :----------------------|
| buttonCornerRadius    |  The corner radius that is applied to buttons.                                                                                                                      | Swift.Double           |
| textFieldCornerRadius |  The corner radius that is applied to text fields.                                                                                                                  | Swift.Double           |
| borderWidth           |  Width of the borders in the SDK such as text fields in points. This value is for inactive text fields. Active text field double this amount.                       | Swift.Double           |
| spacing               |  The spacing that is applied to the layout of the SDK. It impacts horizontal and vertical distance from the bottom sheet borders and spacing between the elements.  | CoreFoundation.CGFloat |

#### MobileSDK.Theme.fontName
| Name      | Definition                                                                                                                    | Type         |
| :---------| :-----------------------------------------------------------------------------------------------------------------------------| :------------|
| fontName  |  Name of the font that will be used across the entire SDK UI. The font needs to be imported as a resource into the project.   | Swift.String |

## Android

Our Android SDK is designed to visually integrate with your application seamlessly. This guide explains how our components adopt your app's theme and how you can further customise their appearance.

### Automatic Integration with Your App's `MaterialTheme`

The SDK's UI components, built with Jetpack Compose, are engineered to automatically inherit styling from your application's `MaterialTheme`. This means colors, typography, and shapes defined in your app's theme will naturally apply to SDK components, ensuring a consistent look and feel with minimal effort.

**How it Works:**

This automatic styling works because:

1.  **Standard Compose Theming:** Your application typically defines its look and feel using `MaterialTheme`, providing a `colorScheme`, `typography`, and `shapes`. Jetpack Compose makes these theme attributes available to all composables nested within that `MaterialTheme` via Composition Locals.
2.  **SDK Components Respect the Theme:** Our SDK's composable functions (like `CardDetailsWidget` and its internal elements) are built to respect this standard Compose mechanism. They read the provided `colorScheme`, `typography`, `shapes`, and contextual values like `LocalContentColor` directly from the theme environment established by your app's `MaterialTheme`.

**What This Means for You:**

* **Consistency:** SDK components will naturally match the look and feel of your application without extra configuration. Buttons, text, backgrounds, and other elements within our SDK's UI will use your defined brand colors and fonts.
* **Simplicity:** You generally don't need to write custom styling code specifically for our SDK components just to match your app's theme.
* **Dynamic Theming Support:** If your app supports light/dark themes by switching `MaterialTheme` configurations, our SDK components will adapt accordingly along with the rest of your app's UI.

**Key Themed Aspects:**

* **Colors:** Backgrounds, text colors, button colors, input field styles, and icon tints within the SDK will derive from your `MaterialTheme.colorScheme` (e.g., using `primary`, `surface`, `onSurface`, etc.) and `LocalContentColor`.
* **Typography:** Text elements will use appropriate styles (like `bodyLarge`, `labelMedium`, etc.) from your `MaterialTheme.typography`.
* **Shapes:** Where applicable (e.g., buttons or card-like elements within the SDK), shapes will be based on your `MaterialTheme.shapes`.

**Prerequisites for Automatic Theming:**

For this seamless integration to work, ensure that:

* Your application uses Jetpack Compose's `MaterialTheme` to define its theme.
* The point in your UI hierarchy where you integrate our SDK's components (e.g., placing `CardDetailsWidget`) is *inside* the scope of your `MaterialTheme` composable.

```kotlin
// Example: Your App's Screen Structure

@Composable
fun YourAppScreenUsingOurSDK() {
    // Your app's MaterialTheme definition
    YourAppTheme {
        Surface(
            modifier = Modifier.fillMaxSize(),
            color = MaterialTheme.colorScheme.background
        ) {
            Column(modifier = Modifier.padding(16.dp)) {
                // Your existing app content - uses YourAppTheme
                Text(
                    "Welcome to Your App",
                    style = MaterialTheme.typography.headlineMedium
                )
                Spacer(modifier = Modifier.height(16.dp))

                // Integrating our SDK component:
                // It automatically inherits styling from YourAppTheme
                CardDetailsWidget(
                    // ... required SDK parameters
                )

                Spacer(modifier = Modifier.height(16.dp))
                // More of your app content
                Button(onClick = { /* ... */ }) {
                    Text("Submit") // Also uses YourAppTheme
                }
            }
        }
    }
}

// Note: YourAppTheme wraps MaterialTheme internally
@Composable
fun YourAppTheme(content: @Composable () -> Unit) {
    MaterialTheme(
        colorScheme = yourAppColorScheme,
        typography = yourAppTypography,
        shapes = yourAppShapes,
        content = content
    )
}
```

### Advanced Customization: Styling SDK Widgets

While automatic theme integration provides a strong foundation, our SDK offers granular control over the appearance of the **SDK Widgets** you integrate (e.g., `CardDetailsWidget`, `PayPalWidget` etc). This is achieved through dedicated `[WidgetName]Appearance` classes.

These `Appearance` classes allow you to customise not only the overall look of the widget but also the appearance of the various internal UI elements it's composed of (like input fields, buttons, labels, etc.).

**How Widget Styling Works:**

1.  **Widget-Specific `Appearance` Classes:** Each SDK Widget that supports detailed customization (e.g., `CardDetailsWidget`) has an associated `[WidgetName]Appearance` data class (e.g., `CardDetailsAppearance`).
2.  **Access to Internal Component Styles:** The properties within a `[WidgetName]Appearance` class often include instances of more fundamental `Appearance` classes for the internal components it uses. For example, `CardDetailsAppearance` might expose:
    *   `textFieldAppearance: TextFieldAppearance` to style its input fields.
    *   `buttonAppearance: ButtonAppearance` to style its action button.
    *   `labelAppearance: TextAppearance` for its labels.
    *   ... etc.
3.  **Applying Customizations:** You create an instance of the widget's `Appearance` class (or modify its default), configure its properties (including the appearances of its internal parts), and pass it to the `appearance` parameter of the SDK Widget.
4.  **Defaults Available:** Each `[WidgetName]Appearance` typically has a `[WidgetName]Defaults.appearance()` composable function providing sensible defaults. You can use the `copy()` method on these defaults to change only what you need.

**Key Benefits of this Approach:**

*   **Targeted Control:** Style specific parts of an SDK Widget without affecting other areas or your app's global theme.
*   **Structured Customization:** Provides a clear and organised way to manage the various visual aspects of complex SDK UI elements.
*   **Consistency within the Widget:** Ensures that if you customise, for instance, how input fields look, all input fields *within that specific SDK Widget instance* will adopt that style.

**Stylable Aspects and Their `Appearance` Classes (Internal Components):**

While you interact directly with `[WidgetName]Appearance` classes for the SDK Widgets, these often expose and allow you to configure appearances for common underlying UI elements used *within* those Widgets. Understanding these underlying `Appearance` types can be helpful when configuring a Widget's appearance:

*   **`ButtonAppearance`**: Defines styles for various button types (filled, outlined, text-based) that might be used as action buttons or other interactive elements within an SDK Widget.
    *   *See: [Button Styling Reference](android/button.md)*
*   **`ImageButtonAppearance`**: Specifically for buttons whose primary content is an image, controlling aspects like image scaling, shape, and ripple if used within a Widget.
    *   *See: [ImageButton Styling Reference](android/imagebutton.md)*
*   **`LinkButtonAppearance`**: For buttons styled to look like hyperlinks/text buttons, often wrapping a `ButtonAppearance.TextButtonAppearance`, used for navigation or secondary actions within a Widget.
    *   *See: [LinkButton Styling Reference](android/linkbutton.md)*
*   **`LinkTextAppearance`**: For text styled to look like hyperlinks, often wrapping a `TextAppearance`, used for inline text within a Widget.
    *   *See: [LinkButton Styling Reference](android/linkbutton.md)*
*   **`TextFieldAppearance`**: Manages the visual style (borders, colors, label styles, placeholder text) of text input fields embedded within an SDK Widget.
    *   *See: [TextField Styling Reference](android/textfield.md)*
*   **`TextAppearance`**: Controls the style (font, color, size, weight) of various text elements such as labels, descriptions, helper text, or non-interactive link-styled text within an SDK Widget.
    *   *See: [Text Styling Reference](android/text.md)*
*   **`IconAppearance`**: Defines the size and tint of decorative or interactive icons used within an SDK Widget.
    *   *See: [Icon Styling Reference](android/icon.md)*
*   **`LoaderAppearance`**: Styles loading indicators (e.g., circular progress spinners) that may appear within an SDK Widget during asynchronous operations.
    *   *See: [Loader Styling Reference](android/loader.md)*
*   **`DropdownAppearance`**: Styles the dropdown menu portion (background, item styles, elevation) of selection components or search fields within an SDK Widget.
    *   *See: [Dropdown Styling Reference](android/dropdown.md)*
*   **`SearchDropdownAppearance`**: Specifically for composite search components, this bundles appearances for both its text input part and the results dropdown list, if such a component is part of a larger SDK Widget.
    *   *See: [SearchDropdown Styling Reference](android/searchdropdown.md)*
*   **`ToggleAppearance`**: Defines the look of on/off toggles, switches, if these are exposed for styling within an SDK Widget.*
    *   *See: [Toggle Styling Reference](android/toggle.md)*

**Examples of Styling SDK Widgets (Linking to Specific Widget Guides):**

Each of the following SDK Widgets provides an `appearance` parameter that accepts a widget-specific `Appearance` class (e.g., `CardDetailsAppearance`). This allows you to fine-tune their look and feel. Refer to the dedicated styling guide for each widget to see the available customization properties and examples:

*   **`CardDetailsWidget`**: For collecting credit/debit card information. Allows customization of its input fields, labels, action buttons, and overall container.
    *   *See its specific styling guide: [Styling the Card Details Widget](../widgets/card.md#5-widget-styling)*
*   **`GiftCardWidget`**: For entering gift card details. Customise its input fields for card number and PIN, balance check elements, and action buttons.
    *   *See its specific styling guide: [Styling the Gift Card Widget](../widgets/giftcard.md#5-widget-styling)*
*   **`AddressDetailsWidget`**: For collecting billing or shipping address information. Style its various address input fields, labels, and any associated action buttons.
    *   *See its specific styling guide: [Styling the Address Details Widget](../widgets/address.md#4-widget-styling)*
*   **`ClickToPayWidget`**: For integrating with Click to Pay services. Customise the appearance of the Click to Pay button/prompt and any related UI elements presented by this widget.
    *   *See its specific styling guide: [Styling the Click To Pay Widget](../widgets/clicktopay.md#4-widget-styling)*
*   **`AfterpayWidget`**: For displaying Afterpay payment options and messaging. Style the Afterpay branding elements, informational text, and any interactive components according to Afterpay's guidelines and your app's theme.
    *   *See its specific styling guide: [Styling the Afterpay Widget](../digital-wallet-widgets/afterpay.md#5-widget-styling)*
*   **`GooglePayWidget`**: For integrating Google Pay. While Google Pay has its own button branding guidelines, this widget might allow customization of surrounding elements or presentational aspects if any are controlled by the SDK.
    *   *See its specific styling guide: [Styling the Google Pay Widget](../digital-wallet-widgets/googlepay.md#9-widget-styling)*
*   **`ColesPayWidget`**: For integrating with Coles Pay. Customise the appearance of Coles Pay specific UI elements, buttons, and branding.
    *   *See its specific styling guide: [Styling the Coles Pay Widget](../digital-wallet-widgets/colespay.md#5-widget-styling)*
*   **`PayPalSavePaymentSourceWidget`**: For saving PayPal as a payment source. Style the PayPal button, informational text, and any UI elements related to the vaulting process.
    *   *See its specific styling guide: [Styling the PayPal Save Payment Source Widget](../digital-wallet-widgets/paypalvault.md#4-widget-styling)*
*   **`Integrated3DS`**: For handling 3D Secure challenges within an integrated flow. While the core challenge UI is often presented in a web view controlled by the issuer, this widget might allow styling of any surrounding SDK-provided chrome or loading indicators.
    *   *See its specific styling guide: [Styling the Integrated 3DS Experience](../widgets/integrated3ds.md#5-widget-styling)*
*   **`Standalone3DS`**: For initiating and displaying standalone 3D Secure challenges. Similar to Integrated 3DS, customization might apply to SDK-provided UI elements around the core challenge window.
    *   *See its specific styling guide: [Styling the Standalone 3DS Experience](../widgets/standalone3ds.md#5-widget-styling)*

### Styling Hierarchy & Precedence

When you style an SDK Widget:

1.  **Base `MaterialTheme` (Foundation):** SDK Widgets and their internal elements will always first try to resolve styling from the ambient `MaterialTheme`. This provides the default look and feel.
2.  **Widget-Level `[WidgetName]Appearance` (Your Primary Control):**
    *   Properties set directly on the `[WidgetName]Appearance` object (e.g., `containerPadding` in the example above) will override the base theme for those specific overall aspects of the widget.
    *   If the `[WidgetName]Appearance` exposes appearances for its internal components (e.g., `inputFieldAppearance: TextFieldAppearance`), the settings you provide for these nested appearances will override the base theme *for those specific internal components within that widget instance*.

**In essence, the explicit settings in the `[WidgetName]Appearance` object you provide are your direct way to customise our SDK Widgets and will take precedence over the general theme adoption for the aspects they control.**

### Summary of Styling SDK Widgets

1.  **Default (Recommended for Basic Integration):**
    *   Rely on automatic `MaterialTheme` integration. SDK Widgets will generally match your app's look and feel.
    *   **When to use:** For most cases where general theme consistency is desired.

2.  **Widget-Specific `Appearance` Classes (For Targeted Customization):**
    *   Use the `appearance` parameter on the SDK Widgets you integrate, providing a configured instance of its `[WidgetName]Appearance` class.
    *   **When to use:**
        *   To deviate from the main theme for specific SDK Widgets or parts within them.
        *   To fine-tune aspects like internal padding, specific component colors, text styles, or shapes within an SDK Widget.

By understanding how to use the `[WidgetName]Appearance` classes, you can tailor our SDK Widgets to perfectly align with your application's unique design requirements. For detailed properties and capabilities of each SDK Widget's `Appearance`, please refer to their individual styling guides linked above. The "Styling Reference" links provide more context on the types of appearance objects you might configure *within* a widget's main appearance.