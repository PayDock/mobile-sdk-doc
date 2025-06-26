# ButtonAppearance

## Android

The `ButtonAppearance` class is a `sealed class` that serves as the base for defining the comprehensive visual style of various types of buttons within the SDK (e.g. `SdkButton`, `SdkTextButton`, `SdkOutlineButton`, `SdkIconButton`). It encapsulates a wide range of properties, from the button's overall container styling (height, shape, colors, elevation, border, padding) to the appearance of its internal content (icon, text, and loading indicator).

### Base Properties

All subclasses of `ButtonAppearance` share the following core properties:

```Kotlin
sealed class ButtonAppearance(
    // Button Appearance
    open val height: Dp,
    open val contentSpacing: Dp,
    open val shape: Shape,
    open val colors: ButtonColors,
    open val elevation: ButtonElevation?,
    open val border: BorderStroke?,
    open val contentPadding: PaddingValues,
    // Content Appearance
    open val iconAppearance: IconAppearance?,
    open val textAppearance: TextAppearance?,
    open val loaderAppearance: LoaderAppearance,
)
```

 Property         | Type                                                       | Description                                                                                                                                                                                                                            |
------------------|------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
 `height`         | `androidx.compose.ui.unit.Dp`                              | The height of the button.                                                                                                                                                                                                              |
 `contentSpacing` | `androidx.compose.ui.unit.Dp`                              | The horizontal spacing between elements within the button's content (e.g., between an icon and text).                                                                                                                                  |
 `shape`          | `androidx.compose.ui.graphics.Shape`                       | The shape of the button's container (e.g., `RoundedCornerShape`, `CircleShape`).                                                                                                                                                       |
 `colors`         | `androidx.compose.material3.ButtonColors`                  | Defines the colors for the button's container and content in various states (enabled, disabled, focused, pressed). Typically obtained from `ButtonDefaults.buttonColors()`, `ButtonDefaults.outlinedButtonColors()`, etc.              |
 `elevation`      | `androidx.compose.material3.ButtonElevation`?              | Defines the elevation (shadow) for the button in different states. Nullable, as not all button types (e.g., text, outlined) have elevation. Typically obtained from `ButtonDefaults.buttonElevation()`.                               |
 `border`         | `androidx.compose.foundation.BorderStroke`?                | Defines the border stroke for the button. Nullable, as not all button types (e.g., filled, text) have a visible border by default.                                                                                                   |
 `contentPadding` | `androidx.compose.foundation.layout.PaddingValues`         | The padding around the button's content (text, icon, loader).                                                                                                                                                                          |
 `iconAppearance` | `IconAppearance`?                                          | The appearance of the icon within the button (size, tint). Nullable for button types that might not have an icon (e.g., `IconButtonAppearance` might consider the main image content, not a separate "icon"). See `IconAppearance`. |
 `textAppearance` | `TextAppearance`?                                          | The appearance of the text within the button (style, color, etc.). Nullable for button types that might not display text (e.g., `IconButtonAppearance`). See `TextAppearance`.                                                           |
 `loaderAppearance`| `LoaderAppearance`                                         | The appearance of the loading indicator (e.g., a circular progress bar) displayed when the button is in a loading state. See `LoaderAppearance`.                                                                                        |

### Concrete Subclasses

`ButtonAppearance` has the following concrete data class implementations, each tailored for a specific button type:

1.  **`FilledButtonAppearance`**: For standard filled buttons with a background color and often elevation.
    *   `iconAppearance` and `textAppearance` are non-nullable as filled buttons usually have text and can have an icon.
2.  **`OutlineButtonAppearance`**: For buttons with a visible border and typically no filled background.
    *   `iconAppearance` and `textAppearance` are non-nullable.
    *   `border` is typically non-nullable.
3.  **`TextButtonAppearance`**: For buttons that primarily consist of text, with minimal container styling.
    *   `iconAppearance` and `textAppearance` are non-nullable.
    *   `elevation` and `border` are often null.
4.  **`IconButtonAppearance`**: For buttons that are primarily represented by an image/icon, without accompanying text.
    *   `iconAppearance` and `textAppearance` are **nullable** in this specific subclass, as the main content is the button's image itself, not necessarily a separate icon or text within other content.
    *   `elevation` and `border` are often null.

Each of these data classes includes a `copy()` method, allowing for easy modification of their specific properties.

### Default Appearances (`ButtonAppearanceDefaults`)

The `ButtonAppearanceDefaults` object provides factory functions to create default instances for each type of button appearance. These defaults are configured to align with Material Design guidelines and use common values for sizing, spacing, and styling.

```Kotlin
object ButtonAppearanceDefaults {

    internal val ButtonCornerRadius = 4.dp
    internal val ButtonSpacing = 8.dp
    internal val ButtonIconSize = 20.dp
    internal val ButtonLoaderSize = 22.dp
    internal val ButtonLoaderWidth = 2.dp
    internal val ButtonHeight = 48.dp

    @Composable
    fun filledButtonAppearance(): ButtonAppearance.FilledButtonAppearance =
        ButtonAppearance.FilledButtonAppearance(
            height = ButtonHeight,
            contentSpacing = ButtonSpacing,
            shape = MaterialTheme.shapes.small,
            colors = ButtonDefaults.buttonColors(),
            elevation = ButtonDefaults.buttonElevation(),
            border = null,
            contentPadding = ButtonDefaults.ContentPadding,
            iconAppearance = IconAppearanceDefaults.appearance()
                .copy(
                    size = ButtonIconSize,
                    tint = ButtonDefaults.buttonColors().contentColor
                ),
            textAppearance = TextAppearanceDefaults.appearance().copy(
                style = LocalTextStyle.current.copy(
                    platformStyle = PlatformTextStyle(includeFontPadding = false)
                )
            ),
            loaderAppearance = LoaderAppearanceDefaults.appearance().copy(
                color = ButtonDefaults.buttonColors().containerColor,
                strokeWidth = ButtonLoaderWidth
            )
        )

    @Composable
    fun outlineButtonAppearance(): ButtonAppearance.OutlineButtonAppearance =
        ButtonAppearance.OutlineButtonAppearance(
            height = ButtonHeight,
            contentSpacing = ButtonSpacing,
            shape = MaterialTheme.shapes.small,
            colors = ButtonDefaults.outlinedButtonColors(),
            elevation = null,
            border = BorderStroke(
                width = ButtonDefaults.outlinedButtonBorder().width,
                color = ButtonDefaults.buttonColors().containerColor
            ),
            contentPadding = ButtonDefaults.ContentPadding,
            iconAppearance = IconAppearanceDefaults.appearance()
                .copy(
                    size = ButtonIconSize,
                    tint = ButtonDefaults.buttonColors().containerColor
                ),
            textAppearance = TextAppearanceDefaults.appearance().copy(
                style = LocalTextStyle.current.copy(
                    platformStyle = PlatformTextStyle(includeFontPadding = false)
                )
            ),
            loaderAppearance = LoaderAppearanceDefaults.appearance().copy(
                color = ButtonDefaults.buttonColors().containerColor,
                strokeWidth = ButtonLoaderWidth
            )
        )

    @Composable
    fun textButtonAppearance(): ButtonAppearance.TextButtonAppearance =
        ButtonAppearance.TextButtonAppearance(
            height = ButtonHeight,
            contentSpacing = ButtonSpacing,
            shape = ButtonDefaults.textShape,
            colors = ButtonDefaults.textButtonColors(),
            elevation = null,
            border = null,
            contentPadding = ButtonDefaults.TextButtonContentPadding,
            iconAppearance = IconAppearanceDefaults.appearance()
                .copy(
                    size = ButtonIconSize,
                    tint = ButtonDefaults.buttonColors().containerColor
                ),
            textAppearance = TextAppearanceDefaults.appearance().copy(
                style = LocalTextStyle.current.copy(
                    platformStyle = PlatformTextStyle(includeFontPadding = false)
                )
            ),
            loaderAppearance = LoaderAppearanceDefaults.appearance().copy(
                color = ButtonDefaults.buttonColors().containerColor,
                strokeWidth = ButtonLoaderWidth
            )
        )

    @Composable
    fun imageButtonAppearance(): ButtonAppearance.IconButtonAppearance =
        ButtonAppearance.IconButtonAppearance(
            height = ButtonHeight,
            contentSpacing = ButtonSpacing,
            shape = ButtonDefaults.textShape,
            colors = ButtonDefaults.textButtonColors(),
            elevation = null,
            border = null,
            contentPadding = ButtonDefaults.ContentPadding,
            iconAppearance = null,
            textAppearance = null,
            loaderAppearance = LoaderAppearanceDefaults.appearance().copy(
                color = ButtonDefaults.buttonColors().containerColor,
                strokeWidth = ButtonLoaderWidth
            )
        )
}
```

Key constants defined in `ButtonAppearanceDefaults`:
*   `ButtonCornerRadius = 4.dp`
*   `ButtonSpacing = 8.dp` (content spacing)
*   `ButtonIconSize = 20.dp`
*   `ButtonLoaderSize = 22.dp` (Note: This is not directly used in the provided defaults, `LoaderAppearance` defaults are used)
*   `ButtonLoaderWidth = 2.dp` (stroke width for the loader)
*   `ButtonHeight = 48.dp`

**Default Factory Functions:**

*   **`filledButtonAppearance()`**:
    *   Uses `MaterialTheme.shapes.small`.
    *   `ButtonDefaults.buttonColors()` and `ButtonDefaults.buttonElevation()`.
    *   `border` is `null`.
    *   `IconAppearance` with `ButtonIconSize` and content color tint.
    *   `TextAppearance` with `LocalTextStyle.current` and `includeFontPadding = false`.
    *   `LoaderAppearance` with container color and `ButtonLoaderWidth`.
*   **`outlineButtonAppearance()`**:
    *   Uses `MaterialTheme.shapes.small`.
    *   `ButtonDefaults.outlinedButtonColors()`.
    *   `elevation` is `null`.
    *   `border` is configured using `ButtonDefaults.outlinedButtonBorder()`.
    *   `IconAppearance` with `ButtonIconSize` and container color tint.
    *   `TextAppearance` and `LoaderAppearance` similar to filled buttons.
*   **`textButtonAppearance()`**:
    *   Uses `ButtonDefaults.textShape`.
    *   `ButtonDefaults.textButtonColors()`.
    *   `elevation` and `border` are `null`.
    *   `contentPadding` uses `ButtonDefaults.TextButtonContentPadding`.
    *   `IconAppearance`, `TextAppearance`, and `LoaderAppearance` similar to outlined buttons.
*   **`imageButtonAppearance()`**: (This maps to `ButtonAppearance.IconButtonAppearance`)
    *   Uses `ButtonDefaults.textShape` and `ButtonDefaults.textButtonColors()` (often implying minimal visual container).
    *   `elevation` and `border` are `null`.
    *   `iconAppearance` and `textAppearance` are explicitly `null` as the button's primary content is the image itself.
    *   `LoaderAppearance` similar to text buttons.

### Usage & Customization

`ButtonAppearance` and its subclasses are used to style various button components throughout your SDK.

**1. Using a Default Appearance:**

```Kotlin
// For a standard filled button
SdkButton(..., appearance = ButtonAppearanceDefaults.filledButtonAppearance() )

// For an outlined button (e.g., SdkOutlineButton) 
SdkOutlineButton(..., appearance = ButtonAppearanceDefaults.outlineButtonAppearance() )

// For a text button (e.g., SdkTextButton) 
SdkTextButton(..., appearance = ButtonAppearanceDefaults.textButtonAppearance() )

// For an icon-only button (e.g., SdkIconButton) 
SdkIconButton(..., appearance = ButtonAppearanceDefaults.imageButtonAppearance() )
```

**2. Customizing an Appearance:**

You typically start from a default and use the `copy()` method of the specific subclass.

```Kotlin
// Customizing a FilledButtonAppearance 
val customFilledAppearance = ButtonAppearanceDefaults.filledButtonAppearance().copy( 
    shape = CircleShape, 
    height = 56.dp, 
    colors = ButtonDefaults.buttonColors(containerColor = Color.Magenta), 
    textAppearance = ButtonAppearanceDefaults.filledButtonAppearance().textAppearance.copy( 
        style = MaterialTheme.typography.titleMedium.copy(color = Color.White) 
    ) 
)

SdkButton(..., appearance = customFilledAppearance )

// Customizing an OutlineButtonAppearance 
val customOutlineAppearance = ButtonAppearanceDefaults.outlineButtonAppearance().copy( 
    border = BorderStroke(2.dp, MaterialTheme.colorScheme.secondary), 
    iconAppearance = ButtonAppearanceDefaults.outlineButtonAppearance().iconAppearance.copy( 
        tint = MaterialTheme.colorScheme.secondary 
    ) 
)

SdkOutlineButton(..., appearance = customOutlineAppearance )
```