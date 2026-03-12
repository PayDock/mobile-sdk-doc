# TextAttributes

## iOS

The `TextAttributes` struct defines the visual styling of textual content within SDK components. It encapsulates properties related to font, color, underline, strikethrough, and italic formatting.

### TextAttributes Definition

The `TextAttributes` struct defines the following properties:

```swift
public struct TextAttributes {
    public var customFont: CustomFont
    public var textColor: Color
    public var isUnderlined: Bool
    public var underlineColor: Color
    public var isStrikethrough: Bool
    public var strikethroughColor: Color
    public var isItalic: Bool
}
```

### Properties

#### TextAttributes

| Property             | Type         | Description                                                         |
| -------------------- | ------------ | ------------------------------------------------------------------- |
| `customFont`         | `CustomFont` | The font used for rendering the text, including font name and size. |
| `textColor`          | `Color`      | The primary color used to render the text.                          |
| `isUnderlined`       | `Bool`       | Determines if the text should be underlined.                        |
| `underlineColor`     | `Color`      | The color of the underline (if `isUnderlined` is `true`).           |
| `isStrikethrough`    | `Bool`       | Determines if the text should have a strikethrough line.            |
| `strikethroughColor` | `Color`      | The color of the strikethrough (if `isStrikethrough` is `true`).    |
| `isItalic`           | `Bool`       | Determines if the text should be italicized.                        |

#### CustomFont

| Property             | Type         | Description                                                         |
| -------------------- | ------------ | ------------------------------------------------------------------- |
| `type`               | `FontType`   | The font type - either `.system` or `.custom(name: String)`.        |
| `size`               | `CGFloat`    | The font size used for rendering the text.                          |

#### FontType (Enum)

| Case                 | Description                                                         |
| -------------------- | ------------------------------------------------------------------- |
| `.system`            | Uses the system font.                                               |
| `.custom(name:)`     | Uses a custom font with the specified font name.                    |

### Default TextAttributes

A default `TextAttributes` instance can be created using the default initializer, which provides a baseline style.

```swift
public init(font: CustomFont = CustomFont(type: .system, size: 14.0),
            textColor: Color = .defaultText,
            isUnderlined: Bool = true,
            underlineColor: Color = .clear,
            isStrikethrough: Bool = true,
            strikethroughColor: Color = .clear,
            isItalic: Bool = false)
```

### Usage & Customization

`TextAttributes` can be customized for individual UI components by modifying properties such as font, color, and decorations.

**1. Using Default Appearance:**

If a component accepts `TextAttributes` and you want to apply the default styling:

```swift
// Assuming TextAppearance is a component that uses Padding
let textAppearance = TextAppearance(text: TextAttributes())
```

**2. Creating a Custom TextAttributes from Scratch:**

```swift
// Using a custom font
let customFont = CustomFont(type: .custom(name: "HelveticaNeue-Bold"), size: 16.0)
let textAttributes = TextAttributes(
    font: customFont,
    textColor: .black,
    isUnderlined: false,
    underlineColor: .clear,
    isStrikethrough: true,
    strikethroughColor: .red,
    isItalic: true
)
let textAppearance = TextAppearance(text: textAttributes)

// Using the system font
let systemFont = CustomFont(type: .system, size: 16.0)
let systemTextAttributes = TextAttributes(font: systemFont, textColor: .black)
```

**3. Modifying a Specific Property:**

```swift
let textAppearance = TextAppearance(text: TextAttributes())
textAppearance.text.isUnderlined = true
```
