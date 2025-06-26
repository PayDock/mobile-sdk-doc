# LinkButtonAppearance

## Android

The `LinkButtonAppearance` class defines the visual style for an `SdkLinkButton`, which is a text-based button styled to resemble a hyperlink/inline text button. It leverages a more specific `ButtonAppearance.TextButtonAppearance` to control the underlying button's characteristics.

### Appearance Definition

`LinkButtonAppearance` wraps a `ButtonAppearance.TextButtonAppearance` instance, thereby inheriting and specializing the styling for text buttons specifically for link purposes.

```Kotlin
@Immutable
class LinkButtonAppearance(
    val actionButton: ButtonAppearance.TextButtonAppearance
)
```

### Properties

 Property       | Type                                   | Description                                                                                                                                                                                                                                                           |
----------------|----------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
 `actionButton` | `ButtonAppearance.TextButtonAppearance` | An instance of `ButtonAppearance.TextButtonAppearance` that defines all visual aspects of the link button, such as its text appearance (via `textAppearance`), height, content padding, colors, etc. See `ButtonAppearance.TextButtonAppearance` for more details. |

### Default Appearance

The `LinkButtonAppearanceDefaults` object provides a default styling for link buttons, making them visually distinct as interactive links.

### Default Appearance

A default `TextAppearance` can be obtained using the `appearance()` function, typically found within an associated defaults object (e.g., `TextAppearanceDefaults.appearance()`). This provides a baseline configuration.

```Kotlin
object LinkButtonAppearanceDefaults {

    private val LinkButtonHeight = 24.dp // Specific height for link buttons    

    /**
    * Provides a default appearance for an [SdkLinkButton].
    * This appearance configures a text button with specific styling for links:
    * - Height: 24.dp
    * - Content Padding: 0.dp (typically links don't have extensive padding)
    * - Text Appearance: Primary color, slightly larger font size than bodyMedium.
    */
    @Composable
    fun appearance() = LinkButtonAppearance(
        actionButton = ButtonAppearanceDefaults.textButtonAppearance().copy(
            height = LinkButtonHeight,
            contentPadding = PaddingValues(0.dp),
            textAppearance = TextAppearanceDefaults.appearance().copy(
                style = MaterialTheme.typography.bodyMedium.copy(
                    color = MaterialTheme.colorScheme.primary,
                    fontSize = (MaterialTheme.typography.bodyMedium.fontSize.value + 1).sp

                )
            )
        )
    )
}
```

The default appearance typically features:
*   A fixed height (`24.dp`).
*   No explicit content padding (`0.dp`), relying on text metrics.
*   Text styled with the primary color from the Material Theme and a slightly increased font size.

### Usage & Customization

**1. Using the Default Appearance:**

```Kotlin
// Assuming LinkButton is a component that uses LinkButtonAppearance 
LinkButton(..., appearance = LinkButtonAppearanceDefaults.appearance() // Apply default styling )
```

**2. Creating a Custom LinkButtonAppearance:**

You customize by creating a new `ButtonAppearance.TextButtonAppearance` with your desired settings and wrapping it in `LinkButtonAppearance`, or by modifying the default.

```Kotlin
// Define a custom TextButtonAppearance for the link 
val customTextButtonForLink = ButtonAppearanceDefaults.textButtonAppearance().copy( 
    height = 30.dp, 
    contentPadding = PaddingValues(horizontal = 4.dp), // Slight horizontal padding 
    textAppearance = TextAppearanceDefaults.appearance().copy( 
        style = MaterialTheme.typography.labelLarge.copy( 
            color = Color.Blue, 
            fontWeight = FontWeight.Bold, 
            textDecoration = TextDecoration.Underline // Add underline 
        ) 
    ), // Potentially customize colors if your TextButtonAppearance supports it //
    colors = ButtonDefaults.textButtonColors(contentColor = Color.Blue) 
)
    
val customLinkButtonAppearance = LinkButtonAppearance( actionButton = customTextButtonForLink )

// Assuming LinkButton is a component that uses LinkButtonAppearance 
LinkButton(..., appearance = customLinkButtonAppearance )
```

**3. Modifying a Default Appearance using `copy()`:**

The `copy()` method on `LinkButtonAppearance` allows you to provide a new `actionButton` appearance. You would typically `copy()` the nested `actionButton` appearance.

```Kotlin
val modifiedAppearance = LinkButtonAppearanceDefaults.appearance().copy( 
    actionButton = LinkButtonAppearanceDefaults.appearance().actionButton.copy( // Copy the inner actionButton 
        textAppearance = LinkButtonAppearanceDefaults.appearance().actionButton.textAppearance.copy( // Copy its textAppearance 
            style = MaterialTheme.typography.bodySmall.copy( // Change the style 
                color = MaterialTheme.colorScheme.secondary, 
                textDecoration = TextDecoration.None // Remove default link-like emphasis if not needed 
            ) 
        ), 
    height = Dp.Unspecified // Let height be determined by content 
    ) 
)
// Assuming LinkButton is a component that uses LinkButtonAppearance 
LinkButton(..., appearance = modifiedAppearance )
```