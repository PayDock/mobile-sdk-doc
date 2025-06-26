# IconAppearance

## Android

The `IconAppearance` class defines the visual style for icons, specifically their size and tint color.

This appearance is typically used by components that display icons (e.g., `SdkIcon`, or icons within buttons, list items, etc.) to control their dimensions and color.

### Appearance Definition

The `IconAppearance` class holds the following properties:

```Kotlin
@Immutable
class IconAppearance(
    val size: Dp,
    val tint: Color
)
```

### Properties

 Property | Type                                 | Description                                                                                                                                                                                            |
----------|--------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
 `size`   | `androidx.compose.ui.unit.Dp`        | The desired size for the icon. If `Dp.Unspecified` is provided, the icon will typically be rendered at its intrinsic size, or the size might be determined by the component containing the icon.       |
 `tint`   | `androidx.compose.ui.graphics.Color` | The color to apply as a tint to the icon. If `Color.Unspecified` is provided, the icon will usually be drawn with its original colors. For vector drawables, this often means it will inherit the `LocalContentColor`. |

### Default Appearance

A default `IconAppearance` can be obtained using the `appearance()` function, typically found within an `IconAppearanceDefaults` object. This default usually means the icon will use its intrinsic size and the local content color for tinting.

```Kotlin
object IconAppearanceDefaults {
    @Composable
    fun appearance(): IconAppearance = IconAppearance(
        size = Dp.Unspecified,
        tint = Color.Unspecified
    )
}
```

### Usage & Customization

`IconAppearance` is applied to components that render icons.

**1. Using the Default Appearance:**

If a component accepts an `IconAppearance` and you want the default styling (e.g., inherit tint, use intrinsic size):

```Kotlin
// Assuming Icon is a component that uses IconAppearance 
Icon(imageVector = Icons.Filled.Add, appearance = IconAppearanceDefaults.appearance())
```

**2. Creating a Custom IconAppearance:**

You can create a custom appearance by specifying the size and/or tint.

```Kotlin
val customIconAppearance = IconAppearance( 
    size = 24.dp, // Specify a fixed size 
    tint = Color.Blue   // Apply a blue tint 
)
// Assuming Icon is a component that uses IconAppearance 
Icon(imageVector = Icons.Filled.Add, appearance = customIconAppearance)

val untintedIconAppearance = IconAppearance( 
    size = 32.dp, 
    tint = Color.Unspecified // Or LocalContentColor.current if explicit inheritance is desired 
)
Icon(imageVector = Icons.Filled.Add, appearance = untintedIconAppearance)
```

**3. Modifying a Default Appearance using `copy()`:**

The `copy()` method on `IconAppearance` is useful for making slight adjustments to an existing appearance.

```Kotlin
val modifiedAppearance = IconAppearanceDefaults.appearance().copy( 
    size = 16.dp // Change only the size, tint remains default (e.g., LocalContentColor) 
)
// Assuming Icon is a component that uses IconAppearance 
Icon(imageVector = Icons.Filled.Add, appearance = modifiedAppearance)
```