# TextFieldAppearance

## iOS

The `TextFieldAppearance` struct defines the visual styling and layout configuration of text fields within the SDK. It supports customization for color states, font attributes, and layout dimensions, all organized into logical substructures.

---

### TextFieldAppearance Definition

```swift
public struct TextFieldAppearance {
    public var colors: TextFieldColors
    public var dimensions: TextFieldDimensions
    public var fonts: TextFieldFonts

    public init(colors: TextFieldColors = TextFieldColors(),
                dimensions: TextFieldDimensions = TextFieldDimensions(),
                fonts: TextFieldFonts = TextFieldFonts()) {
        self.colors = colors
        self.dimensions = dimensions
        self.fonts = fonts
    }
}
````

### Properties

| Property     | Type                  | Description                                                                                 |
| ------------ | --------------------- | ------------------------------------------------------------------------------------------- |
| `colors`     | `TextFieldColors`     | Defines active, inactive, error, and success colors, as well as text and background colors. |
| `dimensions` | `TextFieldDimensions` | Defines corner radius, border widths, and padding.                                          |
| `fonts`      | `TextFieldFonts`      | Defines typography for text, title, placeholder, and error messages.                        |

---

## Substructures

### TextFieldColors

```swift
public struct TextFieldColors {
    public var active: Color
    public var inactive: Color
    public var error: Color
    public var success: Color
    public var text: Color
    public var placeholder: Color
    public var background: Color

    public init(active: Color = .defaultPrimary,
                inactive: Color = .defaultBorder,
                error: Color = .defaultError,
                success: Color = .defaultSuccess,
                text: Color = .defaultText,
                placeholder: Color = .defaultPlaceholder,
                background: Color = .defaultBackground) {
        self.active = active
        self.inactive = inactive
        self.error = error
        self.success = success
        self.text = text
        self.placeholder = placeholder
        self.background = background
    }
}
```

| Property      | Type    | Description                            |
| ------------- | ------- | -------------------------------------- |
| `active`      | `Color` | Border color when active or focused.   |
| `inactive`    | `Color` | Border color when not focused.         |
| `error`       | `Color` | Border and icon color in error state.  |
| `success`     | `Color` | Icon color in success state.           |
| `text`        | `Color` | Color of the inputted text.            |
| `placeholder` | `Color` | Placeholder text color.                |
| `background`  | `Color` | Background color of the text field.    |

---

### TextFieldDimensions

```swift
public struct TextFieldDimensions {
    public var cornerRadius: CGFloat
    public var borderWidth: CGFloat
    public var activeBorderWidth: CGFloat
    public var padding: Padding

    public init(cornerRadius: CGFloat = .defaultTextFieldCornerRadius,
                borderWidth: CGFloat = .defaultBorderWidth,
                activeBorderWidth: CGFloat = .defaultBorderWidth * 2,
                padding: Padding = Padding()) {
        self.cornerRadius = cornerRadius
        self.borderWidth = borderWidth
        self.activeBorderWidth = activeBorderWidth
        self.padding = padding
    }
}
```

| Property            | Type      | Description                               |
| ------------------- | --------- | ----------------------------------------- |
| `cornerRadius`      | `CGFloat` | Corner radius of the text field.          |
| `borderWidth`       | `CGFloat` | Border width when inactive.               |
| `activeBorderWidth` | `CGFloat` | Border width when field is focused.       |
| `padding`           | `Padding` | Spacing around the text field             |

---

### TextFieldFonts

```swift
public struct TextFieldFonts {
    public var text: TextAttributes
    public var title: TextAttributes
    public var placeholder: TextAttributes
    public var error: TextAttributes

    public init(text: TextAttributes = TextAttributes(font: CustomFont(size: 16)),
                title: TextAttributes = TextAttributes(font: CustomFont(size: 16)),
                placeholder: TextAttributes = TextAttributes(font: CustomFont(size: 16)),
                error: TextAttributes = TextAttributes(font: CustomFont(size: 12))) {
        self.text = text
        self.title = title
        self.placeholder = placeholder
        self.error = error
    }
}
```

| Property      | Type             | Description                          |
| ------------- | ---------------- | ------------------------------------ |
| `text`        | `TextAttributes` | Font and style for user input text.  |
| `title`       | `TextAttributes` | Font and style for the field label.  |
| `placeholder` | `TextAttributes` | Font and style for placeholder text. |
| `error`       | `TextAttributes` | Font and style for error messages.   |

---

## Usage & Customization

**1. Using Default Appearance:**

```swift
let appearance = TextFieldAppearance()
```

**2. Customizing Colors and Fonts:**

```swift
let colors = TextFieldColors(
    active: .blue,
    inactive: .gray,
    error: .red,
    success: .green,
    text: .black,
    placeholder: .lightGray,
    background: .white
)

let dimensions = TextFieldDimensions(cornerRadius: 8, borderWidth: 1, activeBorderWidth: 2, padding: Padding(top: 4, leading: 8, bottom: 4, trailing: 8))

let fonts = TextFieldFonts(
    text: TextAttributes(font: CustomFont(name: "Helvetica", size: 14)),
    title: TextAttributes(font: CustomFont(name: "Helvetica-Bold", size: 14)),
    placeholder: TextAttributes(font: CustomFont(name: "Helvetica-Light", size: 14)),
    error: TextAttributes(font: CustomFont(name: "Helvetica", size: 12), textColor: .red)
)

let appearance = TextFieldAppearance(colors: colors, dimensions: dimensions, fonts: fonts)
```

**3. Modifying Specific Fields:**

```swift
var appearance = TextFieldAppearance()
appearance.colors.error = .orange
appearance.fonts.placeholder.isItalic = true
```
