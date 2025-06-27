# LinkTextAppearance

## Android

The `LinkTextAppearance` class is designed to style text that functions as a hyperlink. It builds upon the general `TextAppearance` by providing a specific context for links, often involving distinct visual cues like a primary color etc.

### Appearance Definition

`LinkTextAppearance` wraps a `TextAppearance` instance, allowing it to leverage all the standard text styling capabilities while being specifically designated for links.

```Kotlin
@Immutable
class LinkTextAppearance(
    val textAppearance: TextAppearance
)
```

### Properties

| Property         | Type           | Description                                                                                                |
|------------------|----------------|------------------------------------------------------------------------------------------------------------|
| `textAppearance` | `TextAppearance` | An instance of the base `TextAppearance` class that defines the font, color, size, and other text attributes for the link. |

### Default Appearance

The `LinkTextAppearanceDefaults` object provides a default styling for links, typically making them stand out from regular text.

```Kotlin
object LinkTextAppearanceDefaults {
    @Composable
    fun appearance() = LinkTextAppearance(
        textAppearance = TextAppearanceDefaults.appearance().copy(
            style = MaterialTheme.typography.bodyMedium.copy(
                color = MaterialTheme.colorScheme.primary,
                fontSize = (MaterialTheme.typography.bodyMedium.fontSize.value + 1).sp
            )
        )
    )
}
```

The default appearance typically uses:
*   The primary color from the current Material Theme (`MaterialTheme.colorScheme.primary`).
*   A slightly increased font size compared to the standard body text to give it more prominence.

### Usage & Customization

`LinkTextAppearance` is intended for use with components that render clickable text links.

**1. Using the Default Appearance:**

If a component accepts a `TextAppearance` and you want the default styling:

```Kotlin
// Assuming Text is a component that uses TextAppearance 
Text(text = "Hello, World!", appearance = LinkTextAppearanceDefaults.appearance().textAppearance)
```

**2. Creating a Custom LinkTextAppearance:**

You can customize the link's appearance by providing a custom `TextAppearance` object.

```Kotlin
val customBaseAppearanceForLink = TextAppearance( 
    style = TextStyle( 
        fontFamily = FontFamily.Serif, 
        fontSize = 18.sp, 
        fontWeight = FontWeight.Bold, 
        color = Color.Blue 
    ), 
    overflow = TextOverflow.Ellipsis, 
    softWrap = true, 
    maxLines = 2, 
    minLines = 1
)
val customLinkAppearance = LinkTextAppearance( textAppearance = customBaseAppearanceForLink )
// Assuming Text is a component that uses TextAppearance 
Text(text = "Hello, World!", appearance = customLinkAppearance.textAppearance)
```

**3. Modifying a Default Appearance using `copy()`:**

If your `LinkTextAppearance` class has a `copy()` method, you can easily modify the wrapped `textAppearance`.

```Kotlin
val modifiedAppearance = TextAppearanceDefaults.appearance().copy( 
    style = MaterialTheme.typography.bodyMedium.copy(color = Color.Red), // Change only the color
    maxLines = 1, 
    overflow = TextOverflow.Ellipsis 
)
val modifiedLinkAppearance = LinkTextAppearance( textAppearance = modifiedAppearance )
// Assuming Text is a component that uses TextAppearance 
Text(text = "Hello, World!", appearance = modifiedLinkAppearance.textAppearance)
```