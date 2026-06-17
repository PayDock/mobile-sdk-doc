# LoaderAppearance

## Android

The `LoaderAppearance` class defines the visual style for circular loading indicators, such as `CircularProgressIndicator`. It allows for customization of the loader's color, thickness, track background, and the shape of its stroke ends.

### Appearance Definition

The `LoaderAppearance` class encapsulates the following properties:

```Kotlin
@Immutable
class LoaderAppearance(
    val color: Color,
    val strokeWidth: Dp,
    val trackColor: Color,
    val strokeCap: StrokeCap
)
```

### Properties

| Property      | Type                                     | Description                                                                                                                               |
|---------------|------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| `color`       | `androidx.compose.ui.graphics.Color`     | The color of the active part of the loading indicator (the moving arc or circle).                                                           |
| `strokeWidth` | `androidx.compose.ui.unit.Dp`            | The thickness of the stroke used to draw the loading indicator.                                                                             |
| `trackColor`  | `androidx.compose.ui.graphics.Color`     | The color of the track or background circle behind the active loading indicator. This provides contrast and context for the loader's movement. |
| `strokeCap`   | `androidx.compose.ui.graphics.StrokeCap` | The style of the cap used at the ends of the loading indicator's stroke (e.g., `StrokeCap.Round`, `StrokeCap.Butt`, `StrokeCap.Square`).         |

### Default Appearances

The `LoaderAppearanceDefaults` object provides pre-configured styles for common loader use cases:

```Kotlin
object LoaderAppearanceDefaults {

    /**
    * Default appearance for an indeterminate circular loader.
    * Often used when the loading progress is unknown or ongoing.
    */
    @Composable
    fun appearance(): LoaderAppearance = LoaderAppearance(
        color = ProgressIndicatorDefaults.circularColor, // Default Material 3 indicator color
        strokeWidth = ProgressIndicatorDefaults.CircularStrokeWidth, // Default Material 3 stroke width
        trackColor = ProgressIndicatorDefaults.circularIndeterminateTrackColor, // Default track for indeterminate
        strokeCap = ProgressIndicatorDefaults.CircularIndeterminateStrokeCap // Default stroke cap for indeterminate
    )

    /**
    * Default appearance for a determinate circular progress indicator.
    * Used when the loading progress (e.g., 0-100%) is known.
    */
    @Composable
    fun progressAppearance(): LoaderAppearance = LoaderAppearance(
        color = ProgressIndicatorDefaults.circularColor, // Can also use circularDeterminateColor if needed for specific theme
        strokeWidth = ProgressIndicatorDefaults.CircularStrokeWidth,
        trackColor = ProgressIndicatorDefaults.circularDeterminateTrackColor, // Default track for determinate
        strokeCap = ProgressIndicatorDefaults.CircularDeterminateStrokeCap // Default stroke cap for determinate
    )
}
```

### Usage & Customization

`LoaderAppearance` is applied to components that display circular loaders.

**1. Using a Default Appearance:**

```Kotlin
// Assuming SdkLoader is a component that uses LoaderAppearance 
SdkLoader(appearance = LoaderAppearanceDefaults.appearance() ) // For an indeterminate loader

SdkLoader(progress = { 0.75f }, appearance = LoaderAppearanceDefaults.appearance() ) // 75% progress
```

**2. Creating a Custom LoaderAppearance from Scratch:**

```Kotlin
val customLoader = LoaderAppearance( 
    color = Color.Magenta, 
    strokeWidth = 6.dp, 
    trackColor = Color.LightGray.copy(alpha = 0.5f),
     strokeCap = StrokeCap.Round 
)
// Assuming SdkLoader is a component that uses LoaderAppearance 
SdkLoader(appearance = customLoader )
```

**3. Modifying a Default Appearance using `copy()`:**

If your `LoaderAppearance` class has a `copy()` method, it's useful for minor adjustments.

```Kotlin
val modifiedLoaderAppearance = LoaderAppearanceDefaults.appearance().copy( 
    color = Color.Cyan,
    strokeWidth = 5.dp
)
// Assuming SdkLoader is a component that uses LoaderAppearance 
SdkLoader(appearance = modifiedLoaderAppearance )
```

---

# OverlayLoaderAppearance

The `OverlayLoaderAppearance` class defines the full-screen overlay loader shown while a widget is processing (for example, during 3DS or tokenisation). It controls the dimmed background, an optional card container, the spinner, and the loading text.

### Appearance Definition

```Kotlin
@Immutable
class OverlayLoaderAppearance(
    val backgroundColor: Color,
    val showCard: Boolean = true,
    val cardSize: DpSize? = null,
    val cardAppearance: CardAppearance,
    val loaderAppearance: LoaderAppearance,
    val loaderSize: Dp,
    val loaderSpacing: Dp,
    val loaderText: String,
    val loaderTextAppearance: TextAppearance,
)
```

### Properties

| Property               | Type                                       | Description                                                                                                                            |
|------------------------|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| `backgroundColor`      | `androidx.compose.ui.graphics.Color`       | The dimmed full-screen background behind the loader.                                                                                  |
| `showCard`             | `Boolean`                                  | Whether the spinner and text sit inside a card container. Default `true`.                                                             |
| `cardSize`             | `androidx.compose.ui.unit.DpSize?`         | The card size. When `null` (default) the card dynamically hugs its content (spinner + text + padding); set a `DpSize` for a fixed, centered card. |
| `cardAppearance`       | `CardAppearance`                           | Styling of the card container (corner radius, color, content padding) when `showCard` is `true`.                                      |
| `loaderAppearance`     | `LoaderAppearance`                         | Styling of the spinner itself.                                                                                                         |
| `loaderSize`           | `androidx.compose.ui.unit.Dp`              | The size of the spinner. Default `32.dp`.                                                                                             |
| `loaderSpacing`        | `androidx.compose.ui.unit.Dp`              | Spacing between the spinner and the loading text. Default `16.dp`.                                                                    |
| `loaderText`           | `String`                                   | The text shown beneath the spinner. Default `"Loading..."`.                                                                          |
| `loaderTextAppearance` | `TextAppearance`                           | Typography for the loading text.                                                                                                      |

### Default Appearance

```Kotlin
object OverlayLoaderAppearanceDefaults {
    @Composable
    fun appearance(): OverlayLoaderAppearance = OverlayLoaderAppearance(
        backgroundColor = Color.Black.copy(alpha = 0.45f),
        showCard = true,
        // null = dynamic sizing: the card hugs its content (spinner + text + padding).
        // Set a DpSize for a fixed card.
        cardSize = null,
        cardAppearance = CardAppearanceDefaults.appearance().copy(
            contentPadding = PaddingValues(32.dp)
        ),
        loaderAppearance = LoaderAppearanceDefaults.appearance(),
        loaderSize = 32.dp,
        loaderSpacing = 16.dp,
        loaderText = "Loading...",
        loaderTextAppearance = TextAppearanceDefaults.appearance()
    )
}
```