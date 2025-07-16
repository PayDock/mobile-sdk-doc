# OverlayLoaderAppearance

## iOS

The `OverlayLoaderAppearance` struct defines the visual styling of a loading indicator that appears over an overlay background. It controls both the spinner color and the color of the overlay itself.

### OverlayLoaderAppearance Definition

The `OverlayLoaderAppearance` struct includes the following properties:

```swift
public struct OverlayLoaderAppearance {
    public var color: Color
    public var overlayColor: Color
}
```

### Properties

| Property       | Type    | Description                                            |
| -------------- | ------- | ------------------------------------------------------ |
| `color`        | `Color` | The color of the loading spinner.                      |
| `overlayColor` | `Color` | The color of the overlay background behind the loader. |

### Default OverlayLoaderAppearance

A default `OverlayLoaderAppearance` instance uses SDK default colors for the spinner and for the overlay background.

```swift
public init(color: Color = .defaultPrimary,
            overlayColor: Color = .defaultLoaderOverlay)
```

### Usage & Customization

Customize the appearance of the loader by adjusting the spinner and overlay colors.

**1. Using Default Appearance:**

```swift
let loaderAppearance = OverlayLoaderAppearance()
```

**2. Creating a Custom OverlayLoaderAppearance:**

```swift
let loaderAppearance = OverlayLoaderAppearance(
    color: .white,
    overlayColor: .black.opacity(0.6)
)
```

**3. Modifying After Initialization:**

```swift
var loaderAppearance = OverlayLoaderAppearance()
loaderAppearance.color = .gray
loaderAppearance.overlayColor = .clear
```
