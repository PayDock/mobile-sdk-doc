# SearchDropdownAppearance

## iOS

The `SearchDropdownAppearance` struct defines the visual styling for a search dropdown component, composed of a search text field and a dropdown list. It enables fine-tuned styling through sub-appearance types like `TextFieldAppearance` and `DropdownAppearance`.

---

### SearchDropdownAppearance Definition

```swift
public struct SearchDropdownAppearance {
    public var textField: TextFieldAppearance
    public var dropdown: DropdownAppearance

    public init(textField: TextFieldAppearance = TextFieldAppearance(),
                dropdown: DropdownAppearance = DropdownAppearance()) {
        self.textField = textField
        self.dropdown = dropdown
    }
}
```

### Properties

| Property     | Type                  | Description                                   |
|--------------|-----------------------|-----------------------------------------------|
| `textField`  | `TextFieldAppearance` | Appearance of the search input field.         |
| `dropdown`   | `DropdownAppearance`  | Appearance of the dropdown suggestion list.   |

---

## Substructures

### DropdownAppearance

```swift
public struct DropdownAppearance {
    public var colors: DropdownColors
    public var dimensions: DropdownDimensions
    public var text: DropdownText

    public init(colors: DropdownColors = DropdownColors(),
                dimensions: DropdownDimensions = DropdownDimensions(),
                text: DropdownText = DropdownText()) {
        self.colors = colors
        self.dimensions = dimensions
        self.text = text
    }
}
```

| Property     | Type                  | Description                                      |
|--------------|-----------------------|--------------------------------------------------|
| `colors`     | `DropdownColors`      | Background color of the dropdown list.           |
| `dimensions` | `DropdownDimensions`  | Padding and spacing between list items.          |
| `text`       | `DropdownText`        | Text appearance for items in the dropdown list.  |

---

### DropdownColors

```swift
public struct DropdownColors {
    public var backgroundColor: Color

    public init(backgroundColor: Color = Color(red: 0.91, green: 0.80, blue: 0.96)) {
        self.backgroundColor = backgroundColor
    }
}
```

| Property           | Type    | Description                   |
|--------------------|---------|-------------------------------|
| `backgroundColor`  | `Color` | Background color of dropdown. |

---

### DropdownDimensions

```swift
public struct DropdownDimensions {
    public var padding: Padding
    public var listSpacing: Double

    public init(padding: Padding = Padding(),
                listSpacing: Double = 8) {
        self.padding = padding
        self.listSpacing = listSpacing
    }
}
```

| Property       | Type     | Description                                 |
|----------------|----------|---------------------------------------------|
| `padding`      | `Padding`| Padding around the dropdown content.        |
| `listSpacing`  | `Double` | Vertical spacing between list items.        |

---

### DropdownText

```swift
public struct DropdownText {
    public var listText: TextAppearance

    public init(listText: TextAppearance = TextAppearance()) {
        self.listText = listText
    }
}
```

| Property     | Type             | Description                                   |
|--------------|------------------|-----------------------------------------------|
| `listText`   | `TextAppearance` | Appearance of each dropdown item’s text.      |

---

## Usage & Customization

**1. Using Default Appearance:**

```swift
let appearance = SearchDropdownAppearance()
```

**2. Customizing Appearance:**

```swift
let customDropdownText = DropdownText(
    listText: TextAppearance(
        text: TextAttributes(font: CustomFont(name: "Helvetica", size: 14))
    )
)

let dropdownAppearance = DropdownAppearance(
    colors: DropdownColors(backgroundColor: .white),
    dimensions: DropdownDimensions(listSpacing: 12),
    text: customDropdownText
)

let appearance = SearchDropdownAppearance(
    textField: TextFieldAppearance(), // can be customized too
    dropdown: dropdownAppearance
)
```

**3. Modifying Specific Properties:**

```swift
var appearance = SearchDropdownAppearance()
appearance.dropdown.dimensions.listSpacing = 16
appearance.dropdown.text.listText.text.isItalic = true
```
