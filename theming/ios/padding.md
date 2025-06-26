# Padding

## iOS

The `Padding` struct is a building block for defining the padding arround various UI components in the SDK. It defines separate padding on all sides of the component.

### Padding Definition

The `Padding` class struct the following properties.

```Swift
public struct Padding {
    public var top: CGFloat
    public var leading: CGFloat
    public var bottom: CGFloat
    public var trailing: CGFloat
}
```

### Properties

| Property      | Type      | Description                                                                 |
|---------------|-----------|-----------------------------------------------------------------------------|
| `top`         | `CGFloat` | Defines the top padding of the component that is being customized.          |
| `leading`     | `CGFloat` | Defines the leading padding of the component that is being customized.      |
| `bottom`      | `CGFloat` | Defines the bottom padding of the component that is being customized.       |
| `trailing`    | `CGFloat` | Defines the trailing padding of the component that is being customized.     |

### Default Padding

A default `Padding` can be obtained using the `Padding()` function, typically found within an associated defaults object (e.g., `TextAppearance`). This provides a sensible baseline configuration.

```Swift
    public init(top: CGFloat = 0,
                leading: CGFloat = 0,
                bottom: CGFloat = 0,
                trailing: CGFloat = 0) {
        self.top = top
        self.leading = leading
        self.bottom = bottom
        self.trailing = trailing
    }
```

### Usage & Customization

Padding is used as a customization option in some of the UI components. It can be cutomized by creating an instance of `Padding` or changing only a specific variable.

**1. Using the Default Appearance:**

If a component accepts a `Padding` and you want the default styling:

```Swift
// Assuming TextAppearance is a component that uses Padding
let textAppearance = TextAppearance(padding: Padding())
```

**2. Creating a Custom TextAppearance from Scratch:**

```Swift
// Assuming TextAppearance is a component that uses Padding
let padding = Padding(top: 2, leading: 3, bottom: 5, trailing: 0)
let textAppearance = TextAppearance(padding: padding)
```

**3. Modifying a Default Appearance using:**

```Swift
// Assuming TextAppearance is a component that uses Padding
let textAppearance = TextAppearance(padding: padding)
textAppearance.padding.top = 4
```


