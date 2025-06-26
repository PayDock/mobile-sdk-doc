# TextFieldAppearance

## Android

The `TextFieldAppearance` class provides a comprehensive way to define the visual style and behavior of text input fields. It encapsulates various aspects, including the style of the input text itself, placeholder text, labels (including error labels), validation icons, and overall container properties like shape and colors in different states.

### Appearance Definition

```Kotlin
@Immutable
class TextFieldAppearance(
    // Properties primarily related to the text input area (e.g., for OutlineTextField)
    val style: TextStyle,  // Style of the actual input text
    val placeholder: TextAppearance, // Appearance of the placeholder text
    val label: TextAppearance, // Appearance of the floating label
    val errorLabel: TextAppearance, // Appearance of the error message text
    val validIcon: IconAppearance, // Appearance of the icon shown for valid input
    // Properties primarily related to the decoration box (e.g., for OutlinedTextField)
    val singleLine: Boolean, // Whether the text field should be single-line
    val colors: TextFieldColors, // Colors for different states (focused, unfocused, error, etc.)
    val shape: Shape  // The shape of the text field's container/outline
) 
```

### Properties

| Property      | Type                                     | Description                                                                                                                               |
|---------------|------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| `style`       | `TextStyle`                              | The `TextStyle` for the text that the user types into the field.                                                                          |
| `placeholder` | `TextAppearance`                         | The appearance of the placeholder text shown when the input field is empty and unfocused.                                                 |
| `label`       | `TextAppearance`                         | The appearance of the label associated with the text field. This label often floats above the input area when the field is focused or filled. |
| `errorLabel`  | `TextAppearance`                         | The appearance of the error message displayed below the text field when input validation fails.                                           |
| `validIcon`   | `IconAppearance`                         | The appearance of an icon (e.g., a checkmark) that can be displayed, typically to indicate that the current input is valid.                   |
| `singleLine`  | `Boolean`                                | If `true`, the text field will be constrained to a single line of input. If `false`, it can expand to multiple lines.                     |
| `colors`      | `androidx.compose.material3.TextFieldColors` | Defines the colors for various parts of the text field (text, cursor, indicators, container, label, placeholder) across different states (enabled, disabled, focused, unfocused, error). Typically obtained from `OutlinedTextFieldDefaults.colors()` or `TextFieldDefaults.colors()`. |
| `shape`       | `androidx.compose.ui.graphics.Shape`     | The shape of the text field's outline or background container. Typically obtained from `OutlinedTextFieldDefaults.shape`.                      |


### Default Appearance

The `TextFieldAppearanceDefaults` object provides a standard, Material Design-aligned appearance for text fields.

```Kotlin
object TextFieldAppearanceDefaults {
    @Composable
    fun appearance(): TextFieldAppearance = TextFieldAppearance(
        style = MaterialTheme.typography.bodyMedium,
        singleLine = false,
        placeholder = TextAppearanceDefaults.appearance().copy(
            style = MaterialTheme.typography.bodyMedium,
            maxLines = 1
        ),
        label = TextAppearanceDefaults.appearance().copy(
            maxLines = 1,
            style = MaterialTheme.typography.labelMedium
        ),
        errorLabel = TextAppearanceDefaults.appearance().copy(
            style = MaterialTheme.typography.labelMedium.copy(color = MaterialTheme.colorScheme.error)
        ),
        validIcon = IconAppearanceDefaults.appearance().copy(tint = Success),
        colors = OutlinedTextFieldDefaults.colors(),
        shape = OutlinedTextFieldDefaults.shape,
    )
}
```

The default appearance generally features:
*   Input text styled with `MaterialTheme.typography.bodyMedium`.
*   Placeholder and label styled appropriately, often constrained to a single line.
*   Error label styled with the theme's error color.
*   A success-tinted validation icon.
*   Default colors and shape from `OutlinedTextFieldDefaults`.

### Usage & Customization

`TextFieldAppearance` is used to style custom text field components.

**1. Using the Default Appearance:**

If a component accepts a `TextAppearance` and you want the default styling:

```Kotlin
// Assuming OutlinedTextField is a component that uses TextFieldAppearance 
SdkTextField(..., appearance = TextFieldAppearanceDefaults.appearance())
```

**2. Creating a Custom TextFieldAppearance:**

You can customise by creating a new `TextFieldAppearance` instance or by modifying a default one using its `copy()` method.

```Kotlin
// At the call site or within a theme definition
// Customizing specific parts 
val customLabelAppearance = TextAppearanceDefaults.appearance().copy( 
    style = MaterialTheme.typography.titleSmall.copy(color = Color.DarkGray) 
) 
val customErrorLabelAppearance = TextAppearanceDefaults.appearance().copy( 
    style = MaterialTheme.typography.bodySmall.copy(color = Color.Red, 
    fontWeight = FontWeight.Bold) 
) 
val customValidIconAppearance = IconAppearanceDefaults.appearance().copy(tint = Color.Blue)
val customTextFieldColors = OutlinedTextFieldDefaults.colors( 
    focusedBorderColor = Color.Magenta, 
    unfocusedBorderColor = Color.LightGray, 
    focusedLabelColor = Color.Magenta 
)
val customAppearance = TextFieldAppearance( 
    style = MaterialTheme.typography.bodyLarge.copy(color = Color.Black), 
    placeholder = TextAppearanceDefaults.appearance().copy(
        style = MaterialTheme.typography.bodyLarge.copy(
            color = Color.Gray
        )
    ), 
    label = customLabelAppearance, 
    errorLabel = customErrorLabelAppearance, 
    validIcon = customValidIconAppearance, 
    singleLine = true, 
    colors = customTextFieldColors, 
    shape = CutCornerShape(topStart = 8.dp, bottomEnd = 8.dp)
)
// Assuming OutlinedTextField is a component that uses TextFieldAppearance 
SdkTextField(..., appearance = customAppearance)
```

**3. Modifying a Default Appearance using `copy()`:**

The `copy()` method on `TextFieldAppearance` is very useful for this.

```Kotlin
val modifiedAppearance = TextFieldAppearanceDefaults.appearance().copy(
     style = MaterialTheme.typography.bodyMedium.copy(
        fontWeight = FontWeight.SemiBold),
        singleLine = true,
        colors = OutlinedTextFieldDefaults.colors( 
            focusedBorderColor = MaterialTheme.colorScheme.secondary, 
            focusedLabelColor = MaterialTheme.colorScheme.secondary 
        ), 
        label = TextFieldAppearanceDefaults.appearance().label.copy( 
            style = MaterialTheme.typography.labelMedium.copy(
                color = MaterialTheme.colorScheme.secondary
            ) 
        ) 
)
// Assuming OutlinedTextField is a component that uses TextFieldAppearance 
SdkTextField(..., appearance = modifiedAppearance)
```