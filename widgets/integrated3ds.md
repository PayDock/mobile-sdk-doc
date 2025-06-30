# Integrated 3D Secure

> 
>
> Complete Integrated 3D Secure (3DS) challenges with the Paydock Integrated 3DS Widget. 

The 3DS Widget integrates with the Paydock JS client-sdk within a WebView component. Through this integration, your customer can authenticate the charge using the Integrated 3DS widget, which then communicates with the client-sdk, completes authentication and returns the charge event result.

## iOS

## How to use Integrated 3DS in your iOS application

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `Integrated3DSWidget` view in your application. The widget performs payment verification using 3DS service.

It validates the token to ensure it's valid and of the correct format before rending the UI.

The following sample code demonstrates the definition of the `Integrated3DSWidget`:

```Swift
Integrated3DSWidget(
    config: ThreeDSConfig,
    appearance: ThreeDSWidgetAppearance = ThreeDSWidgetAppearance(),
    completion: @escaping (Result<Integrated3DSResult, Integrated3DSError>) -> Void)
) {...}
```

The following sample code example demonstrates the usage within your application:

```Swift
Integrated3DSWidget(
    config: .init(token: viewModel.token3DS),
    appearance: ThreeDSWidgetAppearance(),
    completion: { result in
        switch result {
        case .success(let result):
            viewModel.handle3dsEvent(result)
                                                
        case .failure(let error):
            viewModel.handleFailure(error: error)
    }
})
```

The widget returns an object that contains the status of the 3DS flow and the 3DS token.


### 2. Parameter definitions

#### Integrated3DSWidget 

| Name                | Definition                                                                       | Type                                                           | Mandatory/Optional |
| :------------------ | :------------------------------------------------------------------------------- | :------------------------------------------------------------- | :----------------  |
| config              |  Configuration options for the integrated 3ds widget                             | `ThreeDSConfig`                                                | Mandatory          |
| appearance          |  Object for visual customization of the widget.                                  | `MobileSDK.ThreeDSWidgetAppearance`                            | Optional           |
| completion          |  Result callback with the 3DS authentication if successful, or error if not.     | `(Result<Integrated3DSResult, Integrated3DSError>) -> Void`    | Mandatory          |

#### ThreeDSConfig 

| Name                | Definition                                                                       | Type                                                           | Mandatory/Optional |
| :------------------ | :------------------------------------------------------------------------------- | :------------------------------------------------------------- | :----------------  |
| token               |  The Integrated 3DS token used for Integrated 3DS widget initialization.         | `String`                                                       | Mandatory          |

#### MobileSDK.Integrated3DSResult

| Name         | Definition                                                                      | Type                        | Mandatory/Optional |
| :----------- | :------------------------------------------------------------------------------ | :-------------------------- | :----------------  |
| event        |  Type of the event that happened in the 3DS flow                                | EventType                   | Mandatory          |
| charge3dsId  |  The Charge ID associated with the 3DS transaction to return to the merchant    | String                      | Mandatory          |

`EventType` enum represents all the possible outcomes of a 3DS flow allowing you to handle it.

#### MobileSDK.EventType

| Name                         | Definition                                                         | Type                             | Mandatory/Optional |
| :--------------------------- | :--------------------------------------------------------------------- | :--------------------------- | :----------------  |
| chargeAuthSuccess            |  Represents a successful 3DS charge authorization                     | EnumCase                      | Mandatory          |
| chargeAuthReject             |  Represents a rejected 3DS charge authorization                       | EnumCase                      | Mandatory          |
| chargeAuthCancelled          |  Represents a charge cancellation event in the 3DS flow               | EnumCase                      | Mandatory          |
| additionalDataCollectSuccess |  Indicates that additional data collection was successfully completed | EnumCase                      | Mandatory          |
| additionalDataCollectReject  |  Indicates that additional data collection was rejected               | EnumCase                      | Mandatory          |
| chargeAuth                   |  Represents a general charge authorization event in the 3DS flow      | EnumCase                      | Mandatory          |

### 3. Callback Explanation

#### Completion Callback

The `completion` callback is invoked after the Integrated 3DS is completed. It receives a `Result<Integrated3DSResult, Integrated3DSError>` if the payment is authenticated. The callback handles the outcome of the payment operation.

### 4. Error/Exceptions Mapping

The following describes Integrated 3DS exceptions that can be thrown. 

#### MobileSDK.Integrated3DSError

| Name                      | Description                                                                        | Error Result            |
| :------------------------ | :--------------------------------------------------------------------------------- | :---------------------- |
| webViewFailed             |  Error thrown when there is an error while communicating with a WebView.           |  NSError                |
| invalidToken              |  Exception thrown when the token is invalid and/or is of the incorrect format/type |  nil                    |
| mappingFailed             |  Exception thrown when there is an issue mapping a web event a SDK expected event. |  nil                    |

### 5. Widget Styling

Defines the visual appearance for the `Integrated3DSWidget`. It handles customizing the overlat loader loading indicator displayed during its operation.

#### Appearance Contract

The `ThreeDSWidgetAppearance` class encapsulates the configurable style properties for the widget.

```Swift
public struct ThreeDSWidgetAppearance {
    public var loader: Theme.OverlayLoaderAppearance
}
```

#### Default Appearance & Customisation

A default appearance is provided by `GlobalTheme` default values. This configures the overlay loader.

##### Using Default Appearance


```Swift
    Integrated3DSWidget( 
        ...
        appearance: ThreeDSWidgetAppearance = ThreeDSWidgetAppearance() // Uses the default appearance
    )
```

##### Customising Appearance

You can create a custom `ThreeDSWidgetAppearance` by providing a specific `OverlayLoaderAppearance`.

```Swift
struct MyCustom3DSScreen: View { 
    private func myCustomAppearance() -> ThreeDSWidgetAppearance {
        let loader = Theme.OverlayLoaderAppearance(color: .red, overlayColor: .gray.opacity(0.1))
        let appearance = ThreeDSWidgetAppearance(loader: loader)
        return appearance
    }
    
    var body: some View {
        Integrated3DSWidget( 
            ...
            appearance: ThreeDSWidgetAppearance = myCustomAppearance()
            ...
        )
    }
}
```

#### Style Attributes

The following attributes can be configured within `ThreeDSWidgetAppearance`:

 Name                | Description                                                                                              | Type                               | Default Value (from `GlobalTheme`)  |
---------------------|----------------------------------------------------------------------------------------------------------|------------------------------------|-------------------------------------|
 `loader`            | Defines the appearance of the loading indicator shown when the widget is processing or loading content.  | `MobileSDK.Theme.OverlayLoader`    | `Theme.loader`                      |

---

**Note:**
* The `OverlayLoader` has it's own detailed documentation explaining configurable attributes (like colors, shapes, typography if applicable, stroke width, etc.).*  

## Android

## How to use the Integrated 3DS Widget

### 1. Overview

This section provides a step-by-step guide on how to initialize and use the `Integrated3DSWidget` composable in your application. The widget performs verifying payment using 3DS service.

It validates the token to ensure it's valid and of the correct format before rending the UI.

The following sample code demonstrates the definition of the `Integrated3DSWidget`:

```Kotlin
@Composable
fun Integrated3DSWidget(
    config: ThreeDSConfig,
    appearance: ThreeDSWidgetAppearance = ThreeDSAppearanceDefaults.appearance(),
    completion: (Result<Integrated3DSResult>) -> Unit,
) {...}
```

The following sample code example demonstrates the usage within your application:

```Kotlin
// Initialize the Integrated3DSWidget
Integrated3DSWidget(
    config = ThreeDSConfig(token = threeDSToken),
    appearance = currentOrDefaultAppearance
) { result ->
    result.onSuccess { threeDSResult ->
         // Handle success - Update UI or perform actions
        Log.d("Integrated3DSWidget", "Payment successful. $threeDSResult")
    }.onFailure { exception ->
        // Handle failure - Show error message or take appropriate action
        Log.e("Integrated3DSWidget", "Payment failed. Error: ${exception.message}")
    }
}
```

### 2. Parameter definitions

This subsection describes the various parameters required by the `Integrated3DSWidget` composable. It provides information on the purpose of each parameter and its significance in configuring the behavior of the `Integrated3DSWidget`.

#### Integrated3DSWidget 

| Name                | Definition                                                                       | Type                                 | Mandatory/Optional |
| :------------------ | :------------------------------------------------------------------------------- | :----------------------------------- | :----------------  |
| config              |  Configuration options for the integrated 3ds widget                                                        | `ThreeDSConfig`     | Mandatory          |
| appearance          |  Customization options for the visual appearance of the widget                                            | `ThreeDSWidgetAppearance` | Optional           |
| completion          |  Result callback with the Integrated 3DS authentication if successful, or error if not.     | `(Result<Integrated3DSResult>) -> Unit`    | Mandatory          |

#### ThreeDSConfig

| Name                | Definition                                                                                       | Type                        | Mandatory/Optional      |
| ------------------- | ------------------------------------------------------------------------------------------------ | --------------------------- |----------------------   |
| token               |  The Integrated 3DS token used for Integrated 3DS widget initialization.                               | String                               | Mandatory          |

#### Integrated3DSResult

This subsection outlines the structure of the result or response object returned by the `Integrated3DSWidget` composable. It details the format and components of the object, enabling you to handle the response effectively within your application.

The following sample code demonstrates the response structure:

```Kotlin
data class Integrated3DSResult(
    val event: IntegratedEventType,
    val charge3dsId: String?
)
```

#### Definition

| Name                | Definition                                                                     | Type                        | 
| :------------------ | :----------------------------------------------------------------------------- | :-------------------------- | 
| event               |  The type of event that occurred during Integrated 3DS processing              | `IntegratedEventType`                 | 
| charge3dsId         |  The Charge ID associated with the 3DS transaction to return to the merchant   | String?                      | 

#### IntegratedEventType

| Name                              | Definition                                                                | 
| :-------------------------------- | :------------------------------------------------------------------------ |
| CHARGE_AUTH_SUCCESS               |  Represents a successful 3DS charge authorization                         |
| CHARGE_AUTH_REJECT                |  Represents a rejected 3DS charge authorization                           |
| CHARGE_AUTH                       |  Represents a general charge authorization event in the 3DS flow          |
| CHARGE_AUTH_CANCELLATION          |  Represents a charge cancellation event in the 3DS flow                   |
| ADDITIONAL_DATA_COLLECT_SUCCESS   |  Indicates that additional data collection was successfully completed     |
| ADDITIONAL_DATA_COLLECT_REJECT    |  Indicates that additional data collection was rejected                   |

### 3. Callback Explanation

#### Completion Callback

The `completion` callback is invoked after the Integrated 3DS is completed. It receives a `Result<Integrated3DSResult>` if the payment is authenticated. The callback handles the outcome of the payment operation.

### 4. Error/Exceptions Mapping

The following describes Integrated 3DS exceptions that can be thrown. 

```Kotlin
WebViewException(code: Int?, displayableMessage: String) : Integrated3DSException(displayableMessage)
InvalidTokenException(displayableMessage: String) : Integrated3DSException(displayableMessage)
EventMappingException(displayableMessage: String) : Integrated3DSException(displayableMessage)
```

| Exception               | Description                                                                              | Error Model          |
| :---------------------- | :--------------------------------------------------------------------------------------- | :------------------- |
| WebViewException        |  Exception thrown when there is an error while communicating with a WebView.             |  ThreeDSError        |
| InvalidTokenException   |  Exception thrown when the token is invalid and/or is of the incorrect format/type.      |  ThreeDSError        |
| EventMappingException   |   Exception thrown when there is an issue mapping a web event a SDK expected event.      |  ThreeDSError        |

### 5. Widget Styling

Defines the visual appearance for specific elements within the `Integrated3DSWidget`. Currently, this primarily involves customising the loading indicator displayed during Integrated 3DS operations.

#### Appearance Contract

The `ThreeDSWidgetAppearance` class encapsulates the configurable style properties for the widget.

```Kotlin
@Immutable
class ThreeDSWidgetAppearance(
    val loader: LoaderAppearance
)
```

#### Default Appearance & Customisation

A default appearance is provided by `ThreeDSWidgetAppearance`. This uses a standard loader appearance. You can use this as a starting point or provide a completely custom loader configuration.


##### Using Default Appearance


```Kotlin
    Integrated3DSWidget( 
        ...
        appearance = ThreeDSAppearanceDefaults.appearance() // Uses the default appearance
    )
```

##### Customising Appearance

You can create a custom `ThreeDSWidgetAppearance` or modify the default one using its `copy` method (if your class has one, as seen in the initially provided context).

```Kotlin
@Composable 
fun MyCustomIntegrated3DSScreen() { 
    // Create appearance by using provided defaults, with custom changes
    val customLoaderAppearance = LoaderAppearanceDefaults.appearance().copy(
        // type = LoaderType.Circular, 
        // color = Color.Magenta, 
        // size = 48.dp 
    )
    val customAppearance = ThreeDSWidgetAppearance(
        loader = customLoaderAppearance
    )

    Integrated3DSWidget(
        ...
        appearance = customAppearance, // Use your custom appearance
    )
}
```

#### Style Attributes

The following attributes can be configured within `ThreeDSWidgetAppearance`:

 Name     | Description                                                                                             | Type                                                       | Default Value (from `ThreeDSAppearanceDefaults`) |
----------|---------------------------------------------------------------------------------------------------------|------------------------------------------------------------|-------------------------------------------------------|
 `loader` | Defines the appearance of the loading indicator shown when the widget is processing or loading content. | `LoaderAppearance`      | `LoaderAppearanceDefaults.appearance()`               |

---

**Note:**
*   The `LoaderAppearance` itself would have its own detailed documentation explaining its configurable attributes (like color, size, type, stroke width, etc.). This documentation focuses on how it's used within the `ThreeDSWidgetAppearance`.
