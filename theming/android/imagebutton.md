# ImageButtonAppearance

## Android

The `ImageButtonAppearance` class defines the visual styling for an `SdkImageButton` or similar image-based button components. It allows customization of the button's shape, the ripple effect on interaction, how the image is scaled within the button's bounds, and the image's opacity when the button is disabled.

### Appearance Definition

The `ImageButtonAppearance` class encapsulates the following properties:

```Kotlin
@Immutable
class ImageButtonAppearance(
    val shape: Shape,
    val rippleColor: Color,
    val disabledImageAlpha: Float
)
```

### Properties

 Property             | Type                                     | Description                                                                                                                                                             |
----------------------|------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
 `shape`              | `androidx.compose.ui.graphics.Shape`     | The shape of the button's container, which also defines its clipping boundary.                                                                                            |
 `rippleColor`        | `androidx.compose.ui.graphics.Color`     | The color of the ripple effect displayed when the button is pressed. `Color.Transparent` can be used for no visible ripple.                                              |
 `disabledImageAlpha` | `Float`                                  | The alpha (opacity) value applied to the image when the button is disabled. This value should be between 0.0f (fully transparent) and 1.0f (fully opaque).                 |

### Default Appearance

The `ImageButtonDefaults` object provides a standard appearance configuration for image buttons.

```Kotlin
object ImageButtonDefaults {
    @Composable
    fun appearance(): ImageButtonAppearance =
        ImageButtonAppearance(
            shape = ButtonDefaults.shape,
            rippleColor = Color.Transparent,
            disabledImageAlpha = 0.5f
        )
}
```

The default appearance features:
*   Standard Material Design button shape.
*   No visible ripple effect by default.
*   A 50% alpha transparency for the image when the button is disabled.

### Usage & Customization

`ImageButtonAppearance` is used to style `SdkImageButton` components.

**1. Using the Default Appearance:**

```Kotlin
// Assuming ImageButton is a component that uses ImageButtonAppearance 
ImageButton( 
    ..., 
    appearance = ImageButtonDefaults.appearance() // Apply default styling 
)
```

**2. Creating a Custom ImageButtonAppearance:**

You can customize the appearance by creating a new `ImageButtonAppearance` instance or by modifying a default one using its `copy()` method.

```Kotlin
val customImageButtonAppearance = ImageButtonAppearance( 
    shape = CircleShape, // Make the button circular 
    rippleColor = MaterialTheme.colorScheme.primary.copy(alpha = 0.3f), // Use primary color for ripple 
    disabledImageAlpha = 0.3f // More transparent when disabled 
)
// Assuming ImageButton is a component that uses ImageButtonAppearance 
ImageButton( 
    ..., 
    appearance = customImageButtonAppearance // Apply default styling 
)
```

**3. Modifying a Default Appearance using `copy()`:**

The `copy()` method on `ImageButtonAppearance` allows for easy modification of specific properties.

```Kotlin
val modifiedAppearance = ImageButtonDefaults.appearance().copy( 
    rippleColor = Color.Gray.copy(alpha = 0.2f) // Add a subtle gray ripple 
)
// Assuming ImageButton is a component that uses ImageButtonAppearance 
ImageButton( 
    ..., 
    appearance = ImageButtonDefaults.appearance() // Apply default styling 
)
```