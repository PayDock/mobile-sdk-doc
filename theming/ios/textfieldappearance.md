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
    public var placeholderText: String?
    public var hintText: String?
    public var accessibilityHintText: String?

    public init(colors: TextFieldColors = TextFieldColors(),
                dimensions: TextFieldDimensions = TextFieldDimensions(),
                fonts: TextFieldFonts = TextFieldFonts(),
                placeholderText: String? = nil,
                hintText: String? = nil,
                accessibilityHintText: String? = nil) {
        self.colors = colors
        self.dimensions = dimensions
        self.fonts = fonts
        self.placeholderText = placeholderText
        self.hintText = hintText
        self.accessibilityHintText = accessibilityHintText
    }
}
````

### Properties

| Property     | Type                  | Description                                                                                 |
| ------------ | --------------------- | ------------------------------------------------------------------------------------------- |
| `colors`     | `TextFieldColors`     | Defines active, inactive, error, and success colors, as well as text and background colors. |
| `dimensions` | `TextFieldDimensions` | Defines corner radius, border widths, content padding, and the error/hint message padding.  |
| `fonts`      | `TextFieldFonts`      | Defines typography for text, title, placeholder, and error messages.                        |
| `placeholderText` | `String?`        | Optional override for the field's placeholder text. Defaults to `nil` (the field's built-in placeholder is used). |
| `hintText`   | `String?`             | Optional override for the hint text shown below the field. Defaults to `nil`.               |
| `accessibilityHintText` | `String?`  | Optional custom VoiceOver hint that replaces only the visual hint in the field's accessibility readout. The placeholder, label, value, and state are still announced. Defaults to `nil`. |

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
    public var hint: Color
    public var icon: Color
    public var background: Color

    public init(active: Color = .defaultPrimary,
                inactive: Color = .defaultBorder,
                error: Color = .defaultError,
                success: Color = .defaultSuccess,
                text: Color = .defaultText,
                placeholder: Color = .defaultPlaceholder,
                hint: Color = .defaultHint,
                icon: Color = .defaultHint,
                background: Color = .defaultBackground) {
        self.active = active
        self.inactive = inactive
        self.error = error
        self.success = success
        self.text = text
        self.placeholder = placeholder
        self.hint = hint
        self.icon = icon
        self.background = background
    }
}
```

| Property      | Type    | Description                                                          |
| ------------- | ------- | ------------------------------------------------------------------- |
| `active`      | `Color` | Border color when active or focused.                                |
| `inactive`    | `Color` | Border color when not focused.                                      |
| `error`       | `Color` | Border and icon color in error state.                               |
| `success`     | `Color` | Icon color in success state.                                        |
| `text`        | `Color` | Color of the inputted text.                                         |
| `placeholder` | `Color` | Placeholder text color.                                             |
| `hint`        | `Color` | Color of the hint message shown below the field. Default `.defaultHint`. |
| `icon`        | `Color` | Color of the field's leading icon. Default `.defaultHint`.          |
| `background`  | `Color` | Background color of the text field.                                 |

---

### TextFieldDimensions

```swift
public struct TextFieldDimensions {
    public var cornerRadius: CGFloat
    public var borderWidth: CGFloat
    public var activeBorderWidth: CGFloat
    public var padding: EdgeInsets
    public var messagePadding: EdgeInsets

    public init(cornerRadius: CGFloat = .defaultTextFieldCornerRadius,
                borderWidth: CGFloat = .defaultBorderWidth,
                activeBorderWidth: CGFloat = .defaultBorderWidth * 2,
                padding: EdgeInsets = EdgeInsets(),
                messagePadding: EdgeInsets = EdgeInsets()) {
        self.cornerRadius = cornerRadius
        self.borderWidth = borderWidth
        self.activeBorderWidth = activeBorderWidth
        self.padding = padding
        self.messagePadding = messagePadding
    }
}
```

| Property            | Type         | Description                                                                 |
| ------------------- | ------------ | --------------------------------------------------------------------------- |
| `cornerRadius`      | `CGFloat`    | Corner radius of the text field.                                            |
| `borderWidth`       | `CGFloat`    | Border width when inactive.                                                 |
| `activeBorderWidth` | `CGFloat`    | Border width when field is focused.                                         |
| `padding`           | `EdgeInsets` | Spacing around the text field's content.                                    |
| `messagePadding`    | `EdgeInsets` | Spacing around the error/hint message displayed below the text field.       |

---

### TextFieldFonts

```swift
public struct TextFieldFonts {
    public var text: TextAttributes
    public var title: TextAttributes
    public var placeholder: TextAttributes
    public var error: TextAttributes
    public var hint: TextAttributes

    public init(text: TextAttributes = TextAttributes(font: CustomFont(size: 16)),
                title: TextAttributes = TextAttributes(font: CustomFont(size: 16)),
                placeholder: TextAttributes = TextAttributes(font: CustomFont(size: 16)),
                error: TextAttributes = TextAttributes(font: CustomFont(size: 12)),
                hint: TextAttributes = TextAttributes(font: CustomFont(size: 12))) {
        self.text = text
        self.title = title
        self.placeholder = placeholder
        self.error = error
        self.hint = hint
    }
}
```

| Property      | Type             | Description                          |
| ------------- | ---------------- | ------------------------------------ |
| `text`        | `TextAttributes` | Font and style for user input text.  |
| `title`       | `TextAttributes` | Font and style for the field label.  |
| `placeholder` | `TextAttributes` | Font and style for placeholder text. |
| `error`       | `TextAttributes` | Font and style for error messages.   |
| `hint`        | `TextAttributes` | Font and style for the hint message shown below the field. |

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

let dimensions = TextFieldDimensions(
    cornerRadius: 8,
    borderWidth: 1,
    activeBorderWidth: 2,
    padding: EdgeInsets(top: 4, leading: 8, bottom: 4, trailing: 8),
    messagePadding: EdgeInsets(top: 4, leading: 0, bottom: 0, trailing: 0))

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
