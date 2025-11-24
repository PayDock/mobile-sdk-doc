# ToggleAppearance

## iOS

The `ToggleAppearance` struct defines the visual styling for toggle UI components in the SDK, controlling the active (selected) color of the toggle.

### ToggleAppearance Definition

The `ToggleAppearance` struct includes the following property:

```swift
public struct ToggleAppearance {
    public var activeColor: Color
    public var inactiveColor: Color?
    public var toggleColor: Color?
}
```

### Properties

| Property        | Type    | Description                                                 |
| --------------- | ------- | ----------------------------------------------------------- |
| `activeColor`   | `Color` | The color displayed when the toggle is active (selected).   |
| `inactiveColor` | `Color` | The color displayed when the toggle is inactive (selected). |
| `toggleColor`   | `Color` | The color of the actual toggle switch                       |

### Default ToggleAppearance

A default `ToggleAppearance` instance uses the SDK default primary color for the active state.

```swift
public init(
    activeColor: Color = .defaultPrimary
    inactiveColor: Color? = nil,
    toggleColor: Color? = nil) {
)
```

### Usage & Customization

Customize the active color of a toggle by creating a `ToggleAppearance` instance with a specific color.

**1. Using Default Appearance:**

```swift
let toggleAppearance = ToggleAppearance()
```

**2. Creating a Custom ToggleAppearance:**

```swift
let toggleAppearance = ToggleAppearance(activeColor: .red)
```

**3. Modifying After Initialization:**

```swift
var toggleAppearance = ToggleAppearance()
toggleAppearance.activeColor = .blue
```

**Note:**
*   Changing `inactiveColor`, and `toggleColor` from their default values will initialize the toggle with a custom `ToggleStyle`. On iOS 26.0 and later, this disables the liquid-glass appearance of the toggle.
