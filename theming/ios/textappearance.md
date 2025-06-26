# TextAppearance

## iOS

The `TextAppearance` struct combines text styling and layout padding into a single configuration object used for customizing text-based UI components in the SDK.

### TextAppearance Definition

The `TextAppearance` struct defines the following properties:

```swift
public struct TextAppearance {
    public var text: TextAttributes
    public var padding: Padding
}
```

### Properties

| Property  | Type             | Description                                                                         |
| --------- | ---------------- | ----------------------------------------------------------------------------------- |
| `text`    | `TextAttributes` | Defines the font, color, underline, strikethrough, and italic styling for the text. |
| `padding` | `Padding`        | Defines the space around the text content inside the component.                     |

### Default TextAppearance

A default `TextAppearance` instance can be created using the initializer without arguments, which applies default text styling and zero padding.

```swift
public init(text: TextAttributes = TextAttributes(),
            padding: Padding = Padding())
```

### Usage & Customization

`TextAppearance` allows you to configure both the textual style and its padding in one object.

**1. Using Default Appearance:**

```swift
let appearance = TextAppearance()
```

**2. Creating a Custom TextAppearance:**

```swift
let customText = TextAttributes(
    font: CustomFont(name: "HelveticaNeue-Bold", size: 16.0),
    textColor: .black,
    isUnderlined: false,
    underlineColor: .clear,
    isStrikethrough: false,
    strikethroughColor: .clear,
    isItalic: true
)

let customPadding = Padding(top: 4, leading: 8, bottom: 4, trailing: 8)

let appearance = TextAppearance(text: customText, padding: customPadding)
```

**3. Modifying a Property After Initialization:**

```swift
var appearance = TextAppearance()
appearance.padding.top = 10
appearance.text.isItalic = true
```
