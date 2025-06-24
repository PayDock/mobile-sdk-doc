# TextAppearance

## Android

The `TextAppearance` class is a fundamental building block for defining the visual style and layout behavior of text within various UI components in the SDK. It encapsulates essential text properties, allowing for consistent and reusable text styling across the SDK.

### Appearance Definition

The `TextAppearance` class holds the following properties to define the appearance and behavior of text:

```Kotlin
@Immutable
class TextAppearance(
    val style: TextStyle,
    val overflow: TextOverflow,
    val softWrap: Boolean,
    val maxLines: Int,
    val minLines: Int
)
```

### Properties

| Property     | Type                                   | Description                                                                                                                                                              |
|--------------|----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `style`      | `androidx.compose.ui.text.TextStyle`     | Defines the core visual styling of the text, including font family, weight, size, color, letter spacing, baseline shift, text decoration, etc.                            |
| `overflow`   | `androidx.compose.ui.text.style.TextOverflow` | Specifies how visual overflow should be handled when the text exceeds the available space and `softWrap` is `false`, or `maxLines` is reached. Common values include `Clip`, `Ellipsis`, and `Visible`. |
| `softWrap`   | `Boolean`                              | If `true`, text will wrap at soft line breaks (e.g., spaces) to fit within the available width. If `false`, text will continue on a single line unless a hard line break (`\n`) is encountered or `maxLines` is 1. |
| `maxLines`   | `Int`                                  | The maximum number of lines the text should span. If the text exceeds this limit, it will be truncated according to the `overflow` property. Use `Int.MAX_VALUE` for no limit. |
| `minLines`   | `Int`                                  | The minimum number of lines the text should occupy. If the text content is shorter, the component may reserve space for this many lines.                                 |

### Default Appearance

A default `TextAppearance` can be obtained using the `appearance()` function, typically found within an associated defaults object (e.g., `TextAppearanceDefaults.appearance()`). This provides a sensible baseline configuration.

```Kotlin
object TextAppearanceDefaults {
    @Composable
    fun appearance(): TextAppearance = TextAppearance(
        style = MaterialTheme.typography.bodyMedium,
        overflow = TextOverflow.Clip,
        softWrap = true,
        maxLines = Int.MAX_VALUE,
        minLines = 1,
    )
}
```

### Usage & Customization

You typically apply a `TextAppearance` to a component that renders text. Customization can be achieved by creating an instance of `TextAppearance` with specific values or by using a `copy()` method if provided by the `TextAppearance` class.

**1. Using the Default Appearance:**

If a component accepts a `TextAppearance` and you want the default styling:

```Kotlin
// Assuming Text is a component that uses TextAppearance 
Text(text = "Hello, World!", appearance = TextAppearanceDefaults.appearance())
```

**2. Creating a Custom TextAppearance from Scratch:**

```Kotlin
val customText = TextAppearance( 
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
// Assuming Text is a component that uses TextAppearance 
Text(text = "Hello, World!", appearance = customText)
```

**3. Modifying a Default Appearance using `copy()`:**

If your `TextAppearance` class (or a defaults object) provides a `copy()` method, it's a convenient way to make slight modifications to an existing appearance.

```Kotlin
val modifiedAppearance = TextAppearanceDefaults.appearance().copy( 
    style = MaterialTheme.typography.bodyMedium.copy(color = Color.Red), // Change only the color
    maxLines = 1, 
    overflow = TextOverflow.Ellipsis 
)
// Assuming Text is a component that uses TextAppearance 
Text(text = "Hello, World!", appearance = modifiedAppearance)
```