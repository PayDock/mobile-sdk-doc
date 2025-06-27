# SearchDropdownAppearance

## Android

The `SearchDropdownAppearance` class defines the complete visual style for a `SearchDropdown` component. It acts as a container for two more specific appearance classes: `TextFieldAppearance` for the input field part and `DropdownAppearance` for the dropdown list part.

### Appearance Definition

`SearchDropdownAppearance` combines the appearances for its constituent parts:

```Kotlin
@Immutable
class SearchDropdownAppearance(
    val textField: TextFieldAppearance,
    val dropdown: DropdownAppearance
)
```

### Properties

 Property    | Type                  | Description                                                                                                                                                              |
-------------|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
 `textField` | `TextFieldAppearance` | Defines the appearance of the text input field where the user types their search query. This includes styles for the text, label, placeholder, colors, shape, etc. See `TextFieldAppearance` for more details. |
 `dropdown`  | `DropdownAppearance`  | Defines the appearance of the dropdown list that displays search results. This includes the list's shape, item styling (padding, height, text appearance), etc. See `DropdownAppearance` for more details. |

### Default Appearance

The `SearchDropdownAppearanceDefaults` object provides a default styling configuration for the `SearchDropdown` component.

```Kotlin
object SearchDropdownAppearanceDefaults {
    @Composable
    fun appearance(): SearchDropdownAppearance = SearchDropdownAppearance(
        textField = TextFieldAppearanceDefaults.appearance().copy(singleLine = true),
        dropdown = DropdownAppearanceDefaults.appearance(),
    )
}
```

The default appearance ensures:
*   The text field part uses the default `TextFieldAppearance`, specifically configured to be `singleLine = true` which is typical for search inputs.
*   The dropdown list part uses the default `DropdownAppearance`.

### Usage & Customization

**1. Using the Default Appearance:**

```Kotlin
// Assuming SearchDropdown is the component exists
SearchDropdown( 
    ...
    appearance = SearchDropdownAppearanceDefaults.appearance() // Apply default styling 
)
```

**2. Creating a Custom SearchDropdownAppearance:**

To customize, you can create new instances of `TextFieldAppearance` and `DropdownAppearance` and combine them, or modify the defaults using the `copy()` method.

```Kotlin
/ Define a custom TextFieldAppearance for the search input 
val customSearchTextFieldAppearance = TextFieldAppearanceDefaults.appearance().copy( 
    singleLine = true, 
    colors = OutlinedTextFieldDefaults.colors( 
        focusedBorderColor = Color.Magenta, 
        focusedLabelColor = Color.Magenta 
    ), 
    shape = RoundedCornerShape(8.dp) 
)
// Define a custom DropdownAppearance for the results list
val customSearchDropdownListAppearance = DropdownAppearanceDefaults.appearance().copy( 
    shape = RoundedCornerShape(bottomStart = 8.dp, bottomEnd = 8.dp), // Match text field corners 
    item = TextAppearanceDefaults.appearance().copy( 
        style = MaterialTheme.typography.bodyMedium.copy(color = Color.DarkBlue) 
    ) 
)
// Combine them into a custom SearchDropdownAppearance 
val customSearchAppearance = SearchDropdownAppearance( 
    textField = customSearchTextFieldAppearance, 
    dropdown = customSearchDropdownListAppearance 
)
// Assuming SearchDropdown is the component exists
SearchDropdown( 
    ...
    appearance = SearchDropdownAppearanceDefaults.appearance() // Apply default styling 
)
```

**3. Modifying a Default Appearance using `copy()`:**

The `copy()` method on `SearchDropdownAppearance` allows you to replace either the `textField` or `dropdown` appearance selectively.

```Kotlin
// Example: Only change the dropdown's item text color and keep the default text field
val modifiedSearchAppearance = SearchDropdownAppearanceDefaults.appearance().copy( 
    dropdown = DropdownAppearanceDefaults.appearance().copy( 
        item = TextAppearanceDefaults.appearance().copy( 
            style = MaterialTheme.typography.bodyMedium.copy(color = MaterialTheme.colorScheme.secondary) 
        ) 
    ) 
)
// Assuming SearchDropdown is the component exists
SearchDropdown( // ... appearance = modifiedSearchAppearance )

// Example: Only change the text field's focused border and keep default dropdown 
val modifiedTextFieldSearchAppearance = SearchDropdownAppearanceDefaults.appearance().copy( 
    textField = TextFieldAppearanceDefaults.appearance().copy( 
        singleLine = true, // Important to restate if copying from a base that might not have it 
        colors = OutlinedTextFieldDefaults.colors( 
            focusedBorderColor = Color.Cyan
        ) 
    ) 
)
// Assuming SearchDropdown is the component exists
SearchDropdown( // ... appearance = modifiedTextFieldSearchAppearance )
```