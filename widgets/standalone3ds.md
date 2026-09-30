# Standalone 3D Secure

> 
>
> Complete Standalone 3D Secure (3DS) challenges with the Paydock Standalone 3DS Widget. 

The 3DS Widget integrates with the Paydock JS client-sdk within a WebView component. Through this integration, your customer can authenticate the charge using the Standalone 3DS widget, which then communicates with the client-sdk, completes authentication and returns the charge event result.

The widget performs the whole authentication: device fingerprinting, the frictionless path, the bank's challenge page (when the issuer requires one) and decoupled (out-of-band) approval. Only the challenge page needs to be visible to the shopper, so you can keep the widget hidden behind your own UI until then — see **Loading and progress** in each platform section.

## iOS

## How to use Standalone 3DS in your iOS application

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `Standalone3DSWidget` view in your application. The widget performs payment verification using the 3DS service.

It validates the token to ensure it's valid and of the correct format before rendering the UI.

The following sample code demonstrates the definition of the `Standalone3DSWidget`:

```Swift
Standalone3DSWidget(
    config: ThreeDSConfig,
    appearance: Standalone3dsWidgetAppearance = Standalone3dsWidgetAppearance(),
    loadingDelegate: WidgetLoadingDelegate? = nil,
    onProgress: ((Standalone3DSProgress) -> Void)? = nil,
    completion: @escaping (Result<Standalone3DSResult, Standalone3DSError>) -> Void
) {...}
```

The following sample code example demonstrates the usage within your application:

```Swift
Standalone3DSWidget(
    config: .init(token: viewModel.token3DS),
    onProgress: { progress in
        viewModel.handle3dsProgress(progress)   // optional: drive your own loader / copy
    },
    completion: { result in
        switch result {
        case .success(let result):
            viewModel.handle3dsEvent(result)
        case .failure(let error):
            viewModel.handleFailure(error: error)
        }
    })
```

The widget returns an object that contains the event of the 3DS flow, the charge 3DS ID and, when reported by the 3DS service, the raw status and a result description.

### 2. Parameter definitions

#### Standalone3DSWidget 

| Name                | Definition                                                                                                   | Type                                                           | Mandatory/Optional |
| :------------------ | :----------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------- | :----------------  |
| config              |  Configuration options for the standalone 3DS widget                                                         | `ThreeDSConfig`                                                | Mandatory          |
| appearance          |  Customization options for the built-in overlay loader                                                       | `Standalone3dsWidgetAppearance`                                | Optional           |
| loadingDelegate     |  Delegate control of showing loaders to this instance. When set, the built-in overlay loader is not shown.   | `WidgetLoadingDelegate`                                        | Optional           |
| onProgress          |  Callback for intermediate progress of the authentication (challenge started / loaded / completed, decoupled). Final outcomes are never delivered here. | `((Standalone3DSProgress) -> Void)?` | Optional |
| completion          |  Result callback with the 3DS authentication events if successful, or an error if not.                       | `(Result<Standalone3DSResult, Standalone3DSError>) -> Void`    | Mandatory          |

#### ThreeDSConfig

| Name                | Definition                                                                                       | Type                        | Mandatory/Optional    |
| ------------------- | ------------------------------------------------------------------------------------------------ | --------------------------- |---------------------- |
| token               |  The standalone 3DS token used for standalone 3DS widget initialisation.                         | String                      | Mandatory             |


#### MobileSDK.Standalone3DSResult

| Name               | Definition                                                                                                                                  | Type                        | Mandatory/Optional |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------ | :-------------------------- | :----------------  |
| event              |  Type of the event that happened in the 3DS flow                                                                                            | `EventType`                 | Mandatory          |
| charge3dsId        |  The Charge ID associated with the 3DS transaction to return to the merchant. Empty (`""`) only when the event carried no ID and none was seen earlier in the session (possible for `chargeAuthInfo` / `chargeError`). | String | Mandatory |
| status             |  Raw authentication status reported by the 3DS service for this event (e.g. `success`, `pending`, `rejected`). Informational — branch on `event`, not on this value. | String? | Optional |
| resultDescription  |  Human-readable outcome description when provided (e.g. `frictionless`). Typically only present on frictionless / first-step outcomes; `nil` on challenge results. | String? | Optional |

`EventType` enum represents all the possible outcomes of a 3DS flow allowing you to handle it.

#### MobileSDK.EventType

| Name                         | Definition                                                                                                   | Type                          | Mandatory/Optional |
| :--------------------------- | :----------------------------------------------------------------------------------------------------------- | :---------------------------- | :----------------  |
| chargeAuthSuccess            |  Represents a successful 3DS charge authentication. Final event.                                             | EnumCase                      | Mandatory          |
| chargeAuthReject             |  Represents a rejected 3DS charge authentication. Final event.                                               | EnumCase                      | Mandatory          |
| chargeAuthChallenge          |  The issuer requires a challenge; the widget will now show the bank's challenge page. Not final.             | EnumCase                      | Mandatory          |
| chargeAuthDecoupled          |  The authentication must be approved out-of-band (e.g. in the shopper's banking app). Not final.             | EnumCase                      | Mandatory          |
| chargeAuthInfo               |  Informational event related to the 3DS charge. Not final; carries no charge ID of its own.                   | EnumCase                      | Mandatory          |
| chargeError                  |  The 3DS service reported an error. Final event.                                                              | EnumCase                      | Mandatory          |

Treat `chargeAuthSuccess`, `chargeAuthReject` and `chargeError` as the end of the flow. `chargeAuthChallenge`, `chargeAuthDecoupled` and `chargeAuthInfo` are delivered on the way and are always followed by a final event.

#### MobileSDK.Standalone3DSProgress

Delivered through `onProgress`. These are never final outcomes — the final result is always delivered through `completion`. New cases may be added in future minor versions, so include a `default` case when switching over it.

| Case                                                | Definition                                                                                                                                                  |
| :-------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `challengeStarted(charge3dsId:)`                    |  The issuer requires a challenge. The bank's page is still loading — keep any loader visible. Emitted right after `completion` receives `chargeAuthChallenge`. |
| `challengeLoaded(charge3dsId:reason:)`              |  The bank's challenge page has loaded and is visible. Reveal the widget here. `reason` is `.load` (page painted) or `.timeout` (2.5 s safety fallback elapsed — treat both the same). Never emitted for decoupled authentications. |
| `challengeCompleted(charge3dsId:source:)`           |  The shopper finished the challenge (or approved a decoupled authentication) and the result is being confirmed. Cover the widget with your own "completing verification" state. `source` is `.poll` or `.callback` — informational. A wrong OTP does **not** emit it. Emitted at most once, always before the final event. |
| `decoupled(charge3dsId:description:)`               |  The authentication must be approved out-of-band. `description` is shopper-facing copy from the issuer — display it when present.                            |

Typical sequences:

* Frictionless: no progress events → `completion(.chargeAuthSuccess | .chargeAuthReject)`
* Challenge: `challengeStarted` → `challengeLoaded` → `challengeCompleted` → `completion(.chargeAuthSuccess | .chargeAuthReject | .chargeError)`
* Decoupled: `decoupled` → `challengeCompleted` → `completion(final event)`

### 3. Callback Explanation

#### Completion Callback

The `completion` callback receives every `Standalone3DSResult` event of the flow (intermediate and final) as `.success`, and a `Standalone3DSError` as `.failure` when the widget itself could not run (invalid token, WebView failure, unmappable response). Errors reported by the 3DS service arrive as `.success` with `event == .chargeError`, not as `.failure`.

#### onProgress Callback

The optional `onProgress` callback reports the intermediate steps listed under `Standalone3DSProgress`. Use it to drive your own loader, copy and layout during a challenge. If you do not need that level of detail, omit it — `completion` alone is enough to complete the flow.

#### Loading and progress

The widget has to stay in the view hierarchy from the moment it is created, because fingerprinting and the frictionless path run inside its WebView. Only the bank's challenge page needs to be visible.

Loading is reported at these moments:

| Moment                                                          | Built-in overlay (no `loadingDelegate`) | `loadingDelegate`      |
| :-------------------------------------------------------------- | :-------------------------------------- | :--------------------- |
| Widget appears (fingerprinting, frictionless, challenge loading) | shown                                   | `loadingDidStart()`    |
| Challenge page loaded (`challengeLoaded`)                       | hidden                                  | `loadingDidFinish()`   |
| Shopper completed the challenge (`challengeCompleted`)          | shown again                             | `loadingDidStart()`    |
| Final event (`chargeAuthSuccess` / `chargeAuthReject` / `chargeError`) | hidden                           | `loadingDidFinish()`   |
| Decoupled authentication (`chargeAuthDecoupled`)                | hidden (so you can show the instructions) | `loadingDidFinish()` |
| `chargeAuthInfo`                                                | unchanged                               | —                      |

Start/finish calls are always balanced.

##### WidgetLoadingDelegate

When you supply a `loadingDelegate`, the widget draws no loader at all and only reports the moments above. Your app owns the UI entirely.

```Swift
public protocol WidgetLoadingDelegate: AnyObject {
    func loadingDidStart()
    func loadingDidFinish()
}
```

##### Recommended pattern: hide the widget until the challenge

Keep the widget mounted but invisible under your own overlay, reveal it on `challengeLoaded`, and cover it again on `challengeCompleted`. A no-op delegate suppresses the built-in overlay so your overlay is the single loader.

```Swift
final class SilentLoadingDelegate: WidgetLoadingDelegate {
    func loadingDidStart() {}
    func loadingDidFinish() {}
}

struct ThreeDSSheet: View {
    @ObservedObject var viewModel: CheckoutVM   // holds `token3DS` and `phase`
    private let loadingDelegate = SilentLoadingDelegate()

    var body: some View {
        ZStack {
            Standalone3DSWidget(
                config: .init(token: viewModel.token3DS),
                loadingDelegate: loadingDelegate,
                onProgress: { progress in
                    switch progress {
                    case .challengeStarted:    viewModel.phase = .challengeLoading
                    case .challengeLoaded:     viewModel.phase = .challenge        // reveal
                    case .challengeCompleted:  viewModel.phase = .finalizing       // cover again
                    case .decoupled(_, let description): viewModel.phase = .decoupled(description)
                    default: break
                    }
                },
                completion: { result in viewModel.handle(result) })
            .id(viewModel.token3DS)                              // a new token = a fresh widget (retry)
            .opacity(viewModel.phase == .challenge ? 1 : 0)      // visible only during the challenge
            .accessibilityHidden(viewModel.phase != .challenge)

            if viewModel.phase != .challenge {
                MyPhaseOverlay(phase: viewModel.phase)           // your own loader / copy / retry
            }
        }
    }
}
```

### 4. Error/Exceptions Mapping

The following describes Standalone 3DS errors that can be returned through `completion(.failure)`. Every error exposes a stable `code`, a user-facing `customMessage` and a technical `debugDescription` (see the [Errors guide](../errors.md)).

#### MobileSDK.Standalone3DSError

| Name                      | Code                                     | Description                                                                                 | Error Result            |
| :------------------------ | :--------------------------------------- | :------------------------------------------------------------------------------------------ | :---------------------- |
| webViewFailed             | `STANDALONE_3DS_WEBVIEW_FAILED`          |  Error returned when there is an error while loading or communicating with the WebView.     |  NSError                |
| invalidToken              | `STANDALONE_3DS_INVALID_TOKEN`           |  Error returned when the token is invalid and/or is of the incorrect format/type.           |  nil                    |
| mappingFailed             | `STANDALONE_3DS_RESPONSE_MAPPING_FAILED` |  Error returned when a web event could not be mapped to an SDK event.                       |  nil                    |

Notes:

* Errors reported by the 3DS service itself (the web `error` event) are **not** failures: they arrive as `.success` with `event == .chargeError`.
* `chargeAuthInfo` carries no charge 3DS ID in its payload; it is delivered with the last ID seen in the session (or `""`).
* Events the SDK does not know (e.g. from a newer web SDK) are ignored and never fail the flow.

### 5. Widget Styling

Defines the visual appearance for the `Standalone3DSWidget`. It customises the built-in overlay loader displayed while the widget is loading. When a `loadingDelegate` is supplied, the overlay is not shown and this appearance has no effect.

#### Appearance Contract

The `Standalone3dsWidgetAppearance` struct encapsulates the configurable style properties for the widget.

```Swift
public struct Standalone3dsWidgetAppearance {
    public var overlayLoader: Theme.OverlayLoaderAppearance
}
```

#### Default Appearance & Customisation

A default appearance is provided by `GlobalTheme` default values, with the loader text set to "Processing payment...".

##### Using Default Appearance


```Swift
    Standalone3DSWidget( 
        ...
        appearance: Standalone3dsWidgetAppearance() // Uses the default appearance
    )
```

##### Customising Appearance

You can create a custom `Standalone3dsWidgetAppearance` by providing a specific `Theme.OverlayLoaderAppearance`.

```Swift
struct MyCustom3DSScreen: View { 
    private func myCustomAppearance() -> Standalone3dsWidgetAppearance {
        var loader = GlobalTheme.shared.globalTheme.overlayLoader
        loader.loaderText = "Verifying your card..."
        loader.backgroundColor = .gray.opacity(0.1)
        return Standalone3dsWidgetAppearance(overlayLoader: loader)
    }
    
    var body: some View {
        Standalone3DSWidget( 
            ...
            appearance: myCustomAppearance()
            ...
        )
    }
}
```

#### Style Attributes

The following attributes can be configured within `Standalone3dsWidgetAppearance`:

 Name                | Description                                                                                              | Type                                | Default Value (from `GlobalTheme`)        |
---------------------|----------------------------------------------------------------------------------------------------------|-------------------------------------|-------------------------------------------|
 `overlayLoader`     | Defines the appearance of the built-in overlay loader shown while the widget is loading.                 | `Theme.OverlayLoaderAppearance`     | `GlobalTheme.overlayLoader` + "Processing payment..." |

---

**Note:**
* The `OverlayLoaderAppearance` has its own detailed documentation explaining configurable attributes (colours, card, loader type, text, accessibility label, etc.) — see [Overlay Loader Appearance](../theming/ios/overlayloaderappearance.md).*  

## Android

## How to use the Standalone 3DS Widget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `Standalone3DSWidget` composable in your application. The widget performs payment verification using the 3DS service.

It validates the token to ensure it's valid and of the correct format before rendering the UI.

The following sample code demonstrates the definition of the `Standalone3DSWidget`:

```Kotlin
@Composable
fun Standalone3DSWidget(
    modifier: Modifier = Modifier,
    config: ThreeDSConfig,
    appearance: StandaloneThreeDSWidgetAppearance = StandaloneThreeDSWidgetAppearanceDefaults.appearance(),
    loadingDelegate: WidgetLoadingDelegate? = null,
    onProgress: ((Standalone3DSProgress) -> Unit)? = null,
    completion: (Result<Standalone3DSResult>) -> Unit
) {...}
```

The following sample code example demonstrates the usage within your application:

```Kotlin
Standalone3DSWidget(
    config = ThreeDSConfig(token = threeDSToken),
    onProgress = { progress ->
        // optional: drive your own loader / copy
    },
    completion = { result ->
        result.onSuccess { threeDSResult ->
            // Handle each event - see StandaloneEventType
            Log.d("Standalone3DSWidget", "3DS event: ${threeDSResult.event} (${threeDSResult.charge3dsId})")
        }.onFailure { exception ->
            // Handle failure - Show error message or take appropriate action
            Log.e("Standalone3DSWidget", "3DS failed. Error: ${exception.message}")
        }
    }
)
```

### 2. Parameter definitions

This subsection describes the various parameters required by the `Standalone3DSWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `Standalone3DSWidget`.

#### Standalone3DSWidget 

| Name                | Definition                                                                                                   | Type                                        | Mandatory/Optional |
| :------------------ | :----------------------------------------------------------------------------------------------------------- | :------------------------------------------ | :----------------  |
| modifier            |  Compose modifier applied to the widget (e.g. `fillMaxSize()`, `alpha(...)`).                                | `Modifier`                                  | Optional           |
| config              |  Configuration options for the standalone 3DS widget                                                         | `ThreeDSConfig`                             | Mandatory          |
| appearance          |  Customization options for the built-in loader                                                               | `StandaloneThreeDSWidgetAppearance`         | Optional           |
| loadingDelegate     |  Delegate control of showing loaders to this instance. When set, the built-in loader is not shown.           | `WidgetLoadingDelegate`                     | Optional           |
| onProgress          |  Callback for intermediate progress of the authentication (challenge started / loaded / completed, decoupled). Final outcomes are never delivered here. | `((Standalone3DSProgress) -> Unit)?` | Optional |
| completion          |  Result callback with the Standalone 3DS authentication events if successful, or an error if not.            | `(Result<Standalone3DSResult>) -> Unit`     | Mandatory          |

#### ThreeDSConfig

| Name                | Definition                                                                                       | Type                        | Mandatory/Optional    |
| ------------------- | ------------------------------------------------------------------------------------------------ | --------------------------- |---------------------- |
| token               |  The standalone 3DS token used for standalone 3DS widget initialisation.                         | String                      | Mandatory             |

#### Standalone3DSResult

This subsection outlines the structure of the result or response object returned by the `Standalone3DSWidget` composable. It details the format and components of the object, enabling you to handle the response effectively within your application.

The following sample code demonstrates the response structure:

```Kotlin
data class Standalone3DSResult(
    val event: StandaloneEventType,
    val charge3dsId: String?,
    val status: String? = null,
    val resultDescription: String? = null
)
```

#### Definition

| Name                | Definition                                                                                                                                              | Type                    | 
| :------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------ | :---------------------- | 
| event               |  The type of event that occurred during Standalone 3DS processing                                                                                       | `StandaloneEventType`   | 
| charge3dsId         |  The Charge ID associated with the 3DS transaction to return to the merchant. `null` when the event carried no ID (possible for `CHARGE_AUTH_INFO`).     | String?                 | 
| status              |  Raw authentication status reported by the 3DS service for this event (e.g. `success`, `pending`, `rejected`). Informational — branch on `event`, not on this value. | String? |
| resultDescription   |  Human-readable outcome description when provided (e.g. `frictionless`). Typically only present on frictionless / first-step outcomes; `null` on challenge results. | String? |

#### StandaloneEventType

| Name                   | Definition                                                                                              | 
| :--------------------- | :------------------------------------------------------------------------------------------------------ |
| CHARGE_AUTH_SUCCESS    |  Represents a successful 3DS charge authentication. Final event.                                        |
| CHARGE_AUTH_REJECT     |  Represents a rejected 3DS charge authentication. Final event.                                          |
| CHARGE_AUTH_CHALLENGE  |  The issuer requires a challenge; the widget will now show the bank's challenge page. Not final.        |
| CHARGE_AUTH_DECOUPLED  |  The authentication must be approved out-of-band (e.g. in the shopper's banking app). Not final.        |
| CHARGE_AUTH_INFO       |  Informational event related to the 3DS charge. Not final.                                              |
| CHARGE_ERROR           |  The 3DS service reported an error. Final event.                                                        |

Treat `CHARGE_AUTH_SUCCESS`, `CHARGE_AUTH_REJECT` and `CHARGE_ERROR` as the end of the flow. The other events are delivered on the way and are always followed by a final event.

#### Standalone3DSProgress

Delivered through `onProgress`. These are never final outcomes — the final result is always delivered through `completion`. New subclasses may be added in future minor versions, so include an `else` branch when using `when`.

```Kotlin
sealed class Standalone3DSProgress {
    abstract val charge3dsId: String?
    data class ChallengeStarted(override val charge3dsId: String?) : Standalone3DSProgress()
    data class ChallengeLoaded(override val charge3dsId: String?, val reason: ChallengeLoadedReason) : Standalone3DSProgress()
    data class ChallengeCompleted(override val charge3dsId: String?, val source: ChallengeCompletedSource) : Standalone3DSProgress()
    data class Decoupled(override val charge3dsId: String?, val description: String?) : Standalone3DSProgress()
}

enum class ChallengeLoadedReason { LOAD, TIMEOUT, UNKNOWN }
enum class ChallengeCompletedSource { POLL, CALLBACK, UNKNOWN }
```

| Class                  | Definition                                                                                                                                                  |
| :--------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ChallengeStarted`     |  The issuer requires a challenge. The bank's page is still loading — keep any loader visible. Emitted right after `completion` receives `CHARGE_AUTH_CHALLENGE`. |
| `ChallengeLoaded`      |  The bank's challenge page has loaded and is visible. Reveal the widget here. `reason` is `LOAD` (page painted) or `TIMEOUT` (2.5 s safety fallback elapsed — treat both the same). Never emitted for decoupled authentications. |
| `ChallengeCompleted`   |  The shopper finished the challenge (or approved a decoupled authentication) and the result is being confirmed. Cover the widget with your own "completing verification" state. `source` is `POLL` or `CALLBACK` — informational. A wrong OTP does **not** emit it. Emitted at most once, always before the final event. |
| `Decoupled`            |  The authentication must be approved out-of-band. `description` is shopper-facing copy from the issuer — display it when present.                            |

`UNKNOWN` is used for values not known to the installed SDK version.

Typical sequences:

* Frictionless: no progress events → `completion(CHARGE_AUTH_SUCCESS | CHARGE_AUTH_REJECT)`
* Challenge: `ChallengeStarted` → `ChallengeLoaded` → `ChallengeCompleted` → `completion(CHARGE_AUTH_SUCCESS | CHARGE_AUTH_REJECT | CHARGE_ERROR)`
* Decoupled: `Decoupled` → `ChallengeCompleted` → `completion(final event)`

### 3. Callback Explanation

#### Completion Callback

The `completion` callback receives every `Standalone3DSResult` event of the flow (intermediate and final) as `Result.success`, and a `Standalone3DSException` as `Result.failure` when the widget itself could not run (invalid token, WebView failure, unmappable response). Errors reported by the 3DS service arrive as `Result.success` with `event == CHARGE_ERROR`, not as a failure.

#### onProgress Callback

The optional `onProgress` callback reports the intermediate steps listed under `Standalone3DSProgress`. Use it to drive your own loader, copy and layout during a challenge. If you do not need that level of detail, omit it — `completion` alone is enough to complete the flow.

#### Loading and progress

The widget has to stay in the composition from the moment it is created, because fingerprinting and the frictionless path run inside its WebView. Only the bank's challenge page needs to be visible.

Loading is reported at these moments:

| Moment                                                          | Built-in loader (no `loadingDelegate`)  | `loadingDelegate`            |
| :-------------------------------------------------------------- | :-------------------------------------- | :--------------------------- |
| Widget launches (fingerprinting, frictionless, challenge loading) | shown                                 | `widgetLoadingDidStart()`    |
| Challenge page loaded (`ChallengeLoaded`)                       | hidden                                  | `widgetLoadingDidFinish()`   |
| Shopper completed the challenge (`ChallengeCompleted`)          | shown again                             | `widgetLoadingDidStart()`    |
| Final event (`CHARGE_AUTH_SUCCESS` / `CHARGE_AUTH_REJECT` / `CHARGE_ERROR`) | hidden                      | `widgetLoadingDidFinish()`   |
| Decoupled authentication (`CHARGE_AUTH_DECOUPLED`)              | hidden (so you can show the instructions) | `widgetLoadingDidFinish()` |
| `CHARGE_AUTH_INFO`                                              | unchanged                               | —                            |

Start/finish calls are always balanced.

##### WidgetLoadingDelegate

When you supply a `loadingDelegate`, the widget draws no loader at all and only reports the moments above. Your app owns the UI entirely.

```Kotlin
interface WidgetLoadingDelegate {
    // Called when a widget's loading process starts.
    fun widgetLoadingDidStart()

    // Called when a widget's loading process finishes.
    fun widgetLoadingDidFinish()
}
```

##### Recommended pattern: hide the widget until the challenge

Keep the widget composed but invisible under your own overlay, reveal it on `ChallengeLoaded`, and cover it again on `ChallengeCompleted`. A no-op delegate suppresses the built-in loader so your overlay is the single loader.

```Kotlin
private object SilentLoadingDelegate : WidgetLoadingDelegate {
    override fun widgetLoadingDidStart() {}
    override fun widgetLoadingDidFinish() {}
}

@Composable
fun ThreeDSSheet(
    threeDSToken: String,
    phase: Phase,
    onPhase: (Phase) -> Unit,
    onResult: (Result<Standalone3DSResult>) -> Unit
) {
    Box(Modifier.fillMaxSize()) {
        key(threeDSToken) {                                   // a new token = a fresh widget (retry)
            Standalone3DSWidget(
                modifier = Modifier
                    .fillMaxSize()
                    .alpha(if (phase == Phase.CHALLENGE) 1f else 0f),   // visible only during the challenge
                config = ThreeDSConfig(token = threeDSToken),
                loadingDelegate = SilentLoadingDelegate,
                onProgress = { progress ->
                    when (progress) {
                        is Standalone3DSProgress.ChallengeStarted -> onPhase(Phase.CHALLENGE_LOADING)
                        is Standalone3DSProgress.ChallengeLoaded -> onPhase(Phase.CHALLENGE)      // reveal
                        is Standalone3DSProgress.ChallengeCompleted -> onPhase(Phase.FINALIZING)  // cover again
                        is Standalone3DSProgress.Decoupled -> onPhase(Phase.DECOUPLED)
                        else -> Unit
                    }
                },
                completion = onResult
            )
        }
        if (phase != Phase.CHALLENGE) {
            MyPhaseOverlay(phase = phase)                     // your own loader / copy / retry
        }
    }
}
```

### 4. Error/Exceptions Mapping

The following describes Standalone 3DS exceptions that can be returned through `completion` as `Result.failure`. 

```Kotlin
WebViewException(code: Int?, displayableMessage: String) : Standalone3DSException(displayableMessage)
InvalidTokenException(displayableMessage: String) : Standalone3DSException(displayableMessage)
EventMappingException(displayableMessage: String) : Standalone3DSException(displayableMessage)
```

| Exception               | Description                                                                              | Error Model          |
| :---------------------- | :--------------------------------------------------------------------------------------- | :------------------- |
| WebViewException        |  Exception thrown when there is an error while loading or communicating with the WebView. |  ThreeDSError        |
| InvalidTokenException   |  Exception thrown when the token is invalid and/or is of the incorrect format/type.      |  ThreeDSError        |
| EventMappingException   |  Exception thrown when there is an issue mapping a web event to an SDK expected event.   |  ThreeDSError        |

Notes:

* Errors reported by the 3DS service itself (the web `error` event) are **not** failures: they arrive as `Result.success` with `event == CHARGE_ERROR`.
* Events the SDK does not know (e.g. from a newer web SDK), or events without a `status` (e.g. `CHARGE_AUTH_INFO`), are logged and never fail the flow.

### 5. Widget Styling

Defines the visual appearance for specific elements within the `Standalone3DSWidget`. Currently, this primarily involves customising the built-in loader displayed during Standalone 3DS operations. When a `loadingDelegate` is supplied, the built-in loader is not shown and this appearance has no effect.

#### Appearance Contract

The `StandaloneThreeDSWidgetAppearance` class encapsulates the configurable style properties for the widget.

```Kotlin
@Immutable
class StandaloneThreeDSWidgetAppearance(
    val loader: OverlayLoaderAppearance
)
```

#### Default Appearance & Customisation

A default appearance is provided by `StandaloneThreeDSWidgetAppearanceDefaults.appearance()`. You can use this as a starting point or provide a completely custom loader configuration.


##### Using Default Appearance


```Kotlin
    Standalone3DSWidget( 
        ...
        appearance = StandaloneThreeDSWidgetAppearanceDefaults.appearance() // Uses the default appearance
    )
```

##### Customising Appearance

You can create a custom `StandaloneThreeDSWidgetAppearance` or modify the default one using its `copy` method.

```Kotlin
@Composable 
fun MyCustomStandalone3DSScreen() { 
    // Create appearance by using provided defaults, with custom changes
    val customAppearance = StandaloneThreeDSWidgetAppearanceDefaults.appearance().copy(
        loader = OverlayLoaderAppearanceDefaults.appearance().copy(
            // e.g. colours, size, text
        )
    )

    Standalone3DSWidget(
        ...
        appearance = customAppearance, // Use your custom appearance
    )
}
```

#### Style Attributes

The following attributes can be configured within `StandaloneThreeDSWidgetAppearance`:

 Name     | Description                                                                                             | Type                        | Default Value (from `StandaloneThreeDSWidgetAppearanceDefaults`) |
----------|---------------------------------------------------------------------------------------------------------|-----------------------------|------------------------------------------------------------------|
 `loader` | Defines the appearance of the built-in loader shown while the widget is loading.                        | `OverlayLoaderAppearance`   | `OverlayLoaderAppearanceDefaults.appearance()`                   |

---

**Note:**
*   The `OverlayLoaderAppearance` itself has its own detailed documentation explaining its configurable attributes — see [Loader](../theming/android/loader.md). This documentation focuses on how it's used within the `StandaloneThreeDSWidgetAppearance`.
