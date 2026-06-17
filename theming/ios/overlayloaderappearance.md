# OverlayLoaderAppearance

## iOS

The `OverlayLoaderAppearance` struct defines the visual styling and layout of the full-screen overlay loader shown while a widget is processing (for example, during 3DS or tokenisation). It controls the dimmed background, an optional card container, the spinner, and the loading text, along with accessibility overrides.

### OverlayLoaderAppearance Definition

```swift
public struct OverlayLoaderAppearance {
    public var backgroundColor: Color
    public var showCard: Bool
    public var cardAppearance: CardAppearance
    public var loaderType: OverlayLoaderType
    public var loaderAppearance: LoaderAppearance
    public var loaderSize: CGSize
    public var loaderSpacing: CGFloat
    public var loaderText: String
    public var loaderTextAppearance: TextAppearance
    public var accessibilityLabel: String?
    public var accessibilityHint: String?
    public var accessibilityIdentifier: String?

    public init(backgroundColor: Color = .defaultLoaderOverlay,
                showCard: Bool = true,
                cardAppearance: CardAppearance = CardAppearance(
                    cornerRadius: 16,
                    color: .defaultBackground,
                    padding: UIEdgeInsets(top: 24, left: 32, bottom: 24, right: 32)),
                loaderType: OverlayLoaderType = .swiftUIStyle,
                loaderAppearance: LoaderAppearance = LoaderAppearance(),
                loaderSize: CGSize = .init(width: 48, height: 48),
                loaderSpacing: CGFloat = 16,
                loaderText: String = "Loading...",
                loaderTextAppearance: TextAppearance = TextAppearance(
                    text: TextAttributes(
                        font: CustomFont(type: .system, size: 16),
                        textColor: .defaultText,
                        isUnderlined: false,
                        isStrikethrough: false),
                    padding: Padding(top: 4)),
                accessibilityLabel: String? = nil,
                accessibilityHint: String? = nil,
                accessibilityIdentifier: String? = nil)
}
```

### Properties

| Property                  | Type                | Description                                                                                       |
| ------------------------- | ------------------- | ------------------------------------------------------------------------------------------------ |
| `backgroundColor`         | `Color`             | The dimmed full-screen background behind the loader. Default: `.defaultLoaderOverlay`.            |
| `showCard`                | `Bool`              | Whether the spinner and text sit inside a card container. Default: `true`.                        |
| `cardAppearance`          | `CardAppearance`    | Styling of the card container (corner radius, color, padding) when `showCard` is `true`.          |
| `loaderType`              | `OverlayLoaderType` | The loader style. Default: `.swiftUIStyle`.                                                       |
| `loaderAppearance`        | `LoaderAppearance`  | Styling of the spinner itself.                                                                    |
| `loaderSize`              | `CGSize`            | The size of the spinner. Default: `48 × 48`.                                                      |
| `loaderSpacing`           | `CGFloat`           | Spacing between the spinner and the loading text. Default: `16`.                                  |
| `loaderText`              | `String`            | The text shown beneath the spinner. Default: `"Loading..."`.                                      |
| `loaderTextAppearance`    | `TextAppearance`    | Typography and padding for the loading text.                                                      |
| `accessibilityLabel`      | `String?`           | Optional VoiceOver label override for the loader. When `nil`, a label derived from `loaderText` is used. |
| `accessibilityHint`       | `String?`           | Optional VoiceOver hint override for the loader. Default: `nil`.                                  |
| `accessibilityIdentifier` | `String?`           | Optional accessibility identifier for UI testing. Default: `nil`.                                 |

### Usage & Customization

**1. Using Default Appearance:**

```swift
let loaderAppearance = Theme.OverlayLoaderAppearance()
```

**2. Creating a Custom OverlayLoaderAppearance:**

```swift
let loaderAppearance = Theme.OverlayLoaderAppearance(
    backgroundColor: .black.opacity(0.6),
    showCard: true,
    loaderSize: .init(width: 32, height: 32),
    loaderText: "Processing payment..."
)
```

**3. Modifying After Initialization:**

```swift
var loaderAppearance = Theme.OverlayLoaderAppearance()
loaderAppearance.loaderText = "Processing payment..."
loaderAppearance.showCard = false
```
