# DropdownAppearance

## Android

The `DropdownAppearance` class defines the visual styling for dropdown menus and their items. It encapsulates properties such as the overall shape of the dropdown container, padding and height of individual items, and the text appearance of the items.

This class is used to ensure dropdown menus are styled consistently across the SDK, aligning with the design system.

### Appearance Definition

The `DropdownAppearance` class holds the following properties:

```Kotlin
@Immutable
class DropdownAppearance(
    val shape: Shape,
    val itemPadding: PaddingValues,
    val itemHeight: Dp,
    val item: TextAppearance
)
```

### Properties

| Property      | Type                                     | Description                                                                                                                              |
|---------------|------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| `shape`       | `androidx.compose.ui.graphics.Shape`     | The shape of the dropdown menu container itself (e.g., `RectangleShape`, `RoundedCornerShape`).                                            |
| `itemPadding` | `androidx.compose.foundation.layout.PaddingValues` | The padding applied around the content within each individual dropdown item.                                                     |
| `itemHeight`  | `androidx.compose.ui.unit.Dp`            | The preferred height for each item in the dropdown list.                                                                                   |
| `item`        | `TextAppearance`                         | The appearance (style, color, size, etc.) of the text displayed within each dropdown item. Uses the `TextAppearance` class for definition. |

### Default Appearance

The `DropdownAppearanceDefaults` object provides a standard appearance for dropdown menus.

```Kotlin
object DropdownAppearanceDefaults {

    private val ItemHorizontalPadding = 15.dp
    private val ItemVerticalPadding = 15.dp
    private val ItemHeight = 50.dp

    /**
    * Provides a default appearance for a dropdown menu.
    * - Shape: RectangleShape
    * - Item Padding: 15.dp horizontal, 15.dp vertical
    * - Item Height: 50.dp
    * - Item Text Appearance: Based on TextAppearanceDefaults with MaterialTheme.typography.bodyMedium
    */
    @Composable
    fun appearance(): DropdownAppearance = DropdownAppearance(
        shape = RectangleShape,
        itemPadding = PaddingValues(
            horizontal = ItemHorizontalPadding,
            vertical = ItemVerticalPadding
        ),
        itemHeight = ItemHeight,
        item = TextAppearanceDefaults.appearance().copy(
            style = MaterialTheme.typography.bodyMedium,
        )
    )
}
```

The default appearance typically features:
*   A simple `RectangleShape` for the dropdown container.
*   Specific padding values for items to ensure consistent spacing.
*   A fixed height for each item.
*   Item text styled using `MaterialTheme.typography.bodyMedium`.

### Usage & Customization

`DropdownAppearance` is used to style components that present a list of selectable items in a dropdown format.

**1. Using the Default Appearance:**

If a component accepts a `DropdownAppearance` and you want the default styling:

```Kotlin
// Assuming SearchDropdown uses DropdownAppearance for its dropdown part 
SearchDropdown( 
    ...
    appearance = SearchDropdownAppearanceDefaults.appearance() // This implicitly uses DropdownAppearanceDefaults for the dropdown part 
)

// If a component directly takes DropdownAppearance:
// Assuming DropdownItem is a component that uses DropdownAppearance 
DropdownItem(item = item, appearance = appearance = DropdownAppearanceDefaults.appearance() )
```

**2. Creating a Custom DropdownAppearance:**

You can customize the appearance by creating a new `DropdownAppearance` instance or by modifying a default one using its `copy()` method.

```Kotlin
// Custom item text appearance 
val customItemTextAppearance = TextAppearanceDefaults.appearance().copy( 
    style = MaterialTheme.typography.titleSmall.copy( 
        color = Color.DarkGray, 
        fontWeight = FontWeight.SemiBold 
    ) 
)
val customDropdownAppearance = DropdownAppearance( 
    shape = RoundedCornerShape(8.dp), 
    itemPadding = PaddingValues(horizontal = 20.dp, vertical = 10.dp), 
    itemHeight = 40.dp, 
    item = customItemTextAppearance 
)
// Assuming DropdownItem is a component that uses DropdownAppearance 
DropdownItem(item = item, appearance = appearance = customDropdownAppearance )
```

**3. Modifying a Default Appearance using `copy()`:**

The `copy()` method on `DropdownAppearance` is useful for minor adjustments.

```Kotlin
val modifiedDropdownAppearance = DropdownAppearanceDefaults.appearance().copy( 
    shape = RoundedCornerShape(topStart = 12.dp, topEnd = 12.dp), // Different shape 
    item = DropdownAppearanceDefaults.appearance().item.copy( // Modify the item's TextAppearance 
    style = MaterialTheme.typography.bodyLarge.copy(color = MaterialTheme.colorScheme.secondary) ), 
    itemHeight = 55.dp 
)

// Apply to a component that wraps DropdownAppearance 
val searchAppearanceWithModifiedDropdown = SearchDropdownAppearanceDefaults.appearance().copy( 
    dropdown = modifiedDropdownAppearance 
) 
SearchDropdown( // ... appearance = searchAppearanceWithModifiedDropdown )

// Assuming DropdownItem is a component that uses DropdownAppearance 
DropdownItem(item = item, appearance = appearance = modifiedDropdownAppearance )
```