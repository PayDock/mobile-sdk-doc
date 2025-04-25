# Theming the SDK

Customize the appearance of your UI elements to match the look and feel of your app. This step is optional, and if you choose not to customize the theming parameters, the SDK's default values will be used.

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

### How to Customize the SDK visuals for iOS

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

The following examples showcase how the MobileSDK can be customized:

1. The following shows the default for the MobileSDK, and so there is no customization: 

```Swift
let config = MobileSDKConfig(environment: .staging)
MobileSDK.shared.configureMobileSDK(config: config)
```

2. The following example demonstrates how to customize all parameters::

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

3. The following example demonstrates how to customize colors only:

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

4. The following demonstrates how to customize the dimensions only:

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

## Android SDK Theming

### Automatic Integration with Your App's Theme

Our Android SDK components are built using Jetpack Compose and are designed to seamlessly integrate with your application's existing visual style defined using Compose's `MaterialTheme`.

By default, UI elements provided by our SDK, such as `CardDetailsWidget`, will automatically adopt the colors, typography, and shapes defined in the `MaterialTheme` that wraps them within your application's UI hierarchy.

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

## Summary

Our SDK leverages standard Jetpack Compose theming principles. By simply ensuring our components are placed within your app's `MaterialTheme`, they will automatically adopt your defined visual styles for colors, typography, and shapes, providing a consistent user experience with minimal effort. If specific overrides are needed beyond the default theme adoption, standard Compose techniques (like passing specific parameters if available on SDK components, or using modifiers) can be applied.