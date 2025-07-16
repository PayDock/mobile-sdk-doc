# ButtonAppearance

## iOS

The `ButtonAppearance` struct defines the complete visual and layout configuration for button components in the SDK. It includes customizable properties for color, font, padding, and loading behavior, grouped into four substructures: `ButtonColors`, `ButtonDimensions`, `ButtonFonts`, and `ButtonLoader`.

---

### ButtonAppearance Definition

```swift
public struct ButtonAppearance {
    public var colors: ButtonColors
    public var dimensions: ButtonDimensions
    public var fonts: ButtonFonts
    public var loader: ButtonLoader

    public init(colors: ButtonColors = ButtonColors(),
                dimensions: ButtonDimensions = ButtonDimensions(),
                fonts: ButtonFonts = ButtonFonts(),
                loader: ButtonLoader = ButtonLoader()) {
        self.colors = colors
        self.dimensions = dimensions
        self.fonts = fonts
        self.loader = loader
    }
}
````

### Properties

| Property     | Type               | Description                                                  |
| ------------ | ------------------ | ------------------------------------------------------------ |
| `colors`     | `ButtonColors`     | Defines background, text, image, border colors, and opacity. |
| `dimensions` | `ButtonDimensions` | Defines corner radius, border width, and padding.            |
| `fonts`      | `ButtonFonts`      | Defines text attributes for the button title.                |
| `loader`     | `ButtonLoader`     | Defines appearance of the loading spinner in the button.     |

---

## Substructures

### ButtonColors

```swift
public struct ButtonColors {
    public var background: Color
    public var text: Color
    public var image: Color
    public var border: Color
    public var opacity: Double

    public init(background: Color = .defaultPrimary,
                text: Color = .defaultOnPrimary,
                image: Color = .defaultOnPrimary,
                border: Color = .defaultPrimary,
                opacity: Double = 1.0) {
        self.background = background
        self.text = text
        self.image = image
        self.border = border
        self.opacity = opacity
    }
}
```

| Property     | Type     | Description                             |
| ------------ | -------- | --------------------------------------- |
| `background` | `Color`  | Background color of the button.         |
| `text`       | `Color`  | Color of the button title text.         |
| `image`      | `Color`  | Tint color for any image inside button. |
| `border`     | `Color`  | Color of the button border.             |
| `opacity`    | `Double` | Opacity value for the button.           |

---

### ButtonDimensions

```swift
public struct ButtonDimensions {
    public var cornerRadius: CGFloat
    public var borderWidth: CGFloat
    public var padding: Padding

    public init(cornerRadius: CGFloat = .defaultButtonCornerRadius,
                borderWidth: CGFloat = .defaultBorderWidth,
                padding: Padding = Padding()) {
        self.cornerRadius = cornerRadius
        self.borderWidth = borderWidth
        self.padding = padding
    }
}
```

| Property       | Type      | Description                        |
| -------------- | --------- | ---------------------------------- |
| `cornerRadius` | `CGFloat` | The corner radius of the button.   |
| `borderWidth`  | `CGFloat` | The border width of the button.    |
| `padding`      | `Padding` | Padding around the button content. |

---

### ButtonFonts

```swift
public struct ButtonFonts {
    public var title: TextAttributes

    public init(title: TextAttributes = TextAttributes(font: CustomFont(size: 16.0))) {
        self.title = title
    }
}
```

| Property | Type             | Description                        |
| -------- | ---------------- | ---------------------------------- |
| `title`  | `TextAttributes` | Text styling for the button title. |

---

### ButtonLoader

```swift
public struct ButtonLoader {
    public var spinnerColor: Color

    public init(spinnerColor: Color = .defaultOnPrimary) {
        self.spinnerColor = spinnerColor
    }
}
```

| Property       | Type    | Description                       |
| -------------- | ------- | --------------------------------- |
| `spinnerColor` | `Color` | The color of the loading spinner. |

---

## Usage & Customization

**1. Using Default Appearance:**

```swift
let appearance = ButtonAppearance()
```

**2. Creating a Custom ButtonAppearance:**

```swift
let colors = ButtonColors(background: .black, text: .white, image: .white, border: .gray, opacity: 0.9)
let dimensions = ButtonDimensions(cornerRadius: 10, borderWidth: 2, padding: Padding(top: 8, leading: 16, bottom: 8, trailing: 16))
let fonts = ButtonFonts(title: TextAttributes(font: CustomFont(name: "HelveticaNeue-Bold", size: 18)))
let loader = ButtonLoader(spinnerColor: .gray)

let appearance = ButtonAppearance(
    colors: colors,
    dimensions: dimensions,
    fonts: fonts,
    loader: loader
)
```

**3. Modifying After Initialization:**

```swift
var appearance = ButtonAppearance()
appearance.colors.opacity = 0.5
appearance.dimensions.padding.top = 12
```
