# ToggleAppearance

## Android

The `ToggleAppearance` class defines the visual styling for toggleable components, specifically focusing on the colors used by a `Switch` or similar toggle controls. It allows customization of the switch's appearance in different states (checked, unchecked, disabled, etc.).

### Appearance Definition

The `ToggleAppearance` class encapsulates the color configuration for a toggle:

```Kotlin
@Immutable
class ToggleAppearance(
    val colors: SwitchColors
)
```

### Properties

 Property | Type                                     | Description                                                                                                                                                                                             |
----------|------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
 `colors` | `androidx.compose.material3.SwitchColors` | Defines the colors used for the switch thumb and track in various states such as checked, unchecked, disabled, and their combinations with focused or hovered states. See `SwitchDefaults.colors()` for details on what can be customized. |

### Default Appearance

A default `ToggleAppearance` can be obtained using the `appearance()` function, typically found within an associated defaults object (e.g., `ToggleAppearanceDefaults.appearance()`). This provides a standard Material Design color scheme for the switch.

```Kotlin
object ToggleAppearanceDefaults {
    @Composable
    fun appearance(): ToggleAppearance = ToggleAppearance(
        colors = SwitchDefaults.colors()
    )
}
```

### Usage & Customization

You apply a `ToggleAppearance` to a component that renders a switch. Customization is achieved by creating an instance of `ToggleAppearance` and providing a custom `SwitchColors` object.

**1. Using the Default Appearance:**

If a component accepts a `ToggleAppearance` and you want the default styling:

```Kotlin
// Assuming Switch is a component that uses ToggleAppearance 
SdkToggle(..., appearance = ToggleAppearanceDefaults.appearance())
```

**2. Creating a Custom ToggleAppearance:**

To customize the colors, you'll modify the `SwitchColors` provided by `SwitchDefaults.colors()`.

```Kotlin
// At the call site or within a theme definition 
val customSwitchColors = SwitchDefaults.colors( 
    checkedThumbColor = Color.Green,
    checkedTrackColor = Color.Green.copy(alpha = 0.5f),
    uncheckedThumbColor = Color.Gray,
    uncheckedTrackColor = Color.LightGray,
    disabledCheckedThumbColor = Color.DarkGray,
    disabledCheckedTrackColor = Color.DarkGray.copy(alpha = 0.3f) // ... and other color states as needed 
)
val customToggleAppearance = ToggleAppearance( colors = customSwitchColors )
// Assuming Switch is a component that uses ToggleAppearance 
SdkToggle(..., appearance = customToggleAppearance)
```

**3. Modifying a Default Appearance using `copy()` (if available on `ToggleAppearance`):**

If your `ToggleAppearance` class has a `copy()` method, you can modify specific aspects. The `copy()` on `ToggleAppearance` would typically allow replacing the `colors`. The `SwitchColors` itself also has its own `copy()` methods for more granular changes.

```Kotlin
val modifiedAppearance = ToggleAppearanceDefaults.appearance().copy( 
    colors = SwitchDefaults.colors().copy( // Copy the default SwitchColors 
        checkedThumbColor = Color.Magenta // And change only the checked thumb color // Potentially other specific color changes 
    ) 
)
// Assuming Switch is a component that uses ToggleAppearance 
SdkToggle(..., appearance = modifiedAppearance)
```