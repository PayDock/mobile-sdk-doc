# Install the SDK

To begin using the SDK, you must first add it as a dependency to your app, integrating it into your existing application. A pre-requisite to using the SDK is setting up the Paydock API, the instructions for which are linked in this guide.  


## Setup the Paydock API Integration

Get up and running using the PayDock API in just a few minutes.

1. Signup for a sandbox account by [contacting PayDock](https://paydock.com/contact/). This gives you access to the admin portal.

2. Once you have an account, follow our [integration guide](https://docs.paydock.com/#getting-started) on how to configure and setup your server integration.

3. After authenticating your API keys, integrate the SDK into either your [iOS](#setup-the-paydock-ios-sdk) or [Android](#setup-the-paydock-android-sdk) application by following the instructions in this guide.

## Setup the Paydock iOS SDK

This guide walks you through how to integrate the Paydock iOS SDK into your iOS application.

### Step 1: Configure Repository Access

You can configure repository access using Swift Package Manager.

#### Adding package through Xcode

1. In Xcode, select __File > Add Packages__. Enter the following:

```
https://github.com/PayDock/ios-mobile-sdk
```

2. Select the latest version number from our release page and enter it into the __Dependency Rule__ field.

![Add package](/img/Package_url.png)

3. Confirm by clicking on __Add Package__.

![Add package confirm](/img/Package_add.png)


#### Adding MobileSDK to your own Package.swift

In case your project is a package and you want to include MobileSDK dependency to it you should add MobileSDK directly to your own __Package.swift__.
Example of how to add it to __YourProject__ Package.swift

```Swift
let package = Package(
    name: "YourProject",
    platforms: [
        .iOS(.v16)
    ],
    products: [
        .library(
            name: "YourProject",
            targets: ["YourProject"]),
    ],
    dependencies: [
        .package(url: "https://github.com/PayDock/ios-mobile-sdk", exact: "2.1.1") // Insert latest version number
    ],
    targets: [
        .target(
            name: "YourProject",
            dependencies: [
                .product(name: "MobileSDK", package: "mobile-sdk-ios")
            ],
            path: "Sources"
        ),
    ]
)
```

### Step 2: Add SDK Dependency

Choose the project file where you want to access the SDK components. In this file, add the following import:

```Swift
import MobileSDK
```

The SDK has now been added to your project. To use the SDK in your project, you must include `import MobileSDK` at the top of the file where you will use the SDK. You can then move to the next step and [initialize](/setup/initialise.md) your MobileSDK.

## Setup the Paydock Android SDK

This guide walks you through the steps to add the Paydock Android MobileSDK into your Android application. Follow the instructions below to ensure a smooth setup process.

### Step 1: Repository Configuration

The Paydock Android SDK is distributed via [Maven Central](https://central.sonatype.com/artifact/com.paydock/mobile-sdk), which is included by default in all modern Android projects. **No additional repository configuration is required.**

If you're working with an older project that doesn't include Maven Central by default, you can add it explicitly to your project-level `build.gradle` or `settings.gradle.kts` file:

```groovy
// build.gradle (Project level)
allprojects {
    repositories {
        mavenCentral()
        // ... other repositories
    }
}
```

```kotlin
// settings.gradle.kts (Kotlin DSL)
dependencyResolutionManagement {
    repositories {
        mavenCentral()
        // ... other repositories
    }
}
```

### Step 2: Add the SDK Dependency

Add the Paydock MobileSDK as a dependency in your app module's `build.gradle` file.

#### Find the Latest Version

Always use the latest version available on [Maven Central](https://central.sonatype.com/artifact/com.paydock/mobile-sdk). Check the "Versions" tab for the most recent release.

#### Add the Dependency

**Groovy DSL** (`build.gradle`):
```groovy
dependencies {
    implementation 'com.paydock:mobile-sdk:<latest_version>'
}
```

**Kotlin DSL** (`build.gradle.kts`):
```kotlin
dependencies {
    implementation("com.paydock:mobile-sdk:<latest_version>")
}
```

Replace `<latest_version>` with the actual version number from Maven Central.

### Step 3: Sync Your Project

After adding the dependency:

1. **Sync** your Gradle project by clicking "Sync Now" in Android Studio
2. **Verify** the dependency was resolved successfully in the build output
3. **Import** the SDK in your code where needed:
   ```kotlin
   import com.paydock.mobilesdk.*
   ```

### Next Steps

Your Android project is now configured to use the Paydock Mobile SDK. You can proceed to [initialize the SDK](/setup/initialise.md) and start implementing payment functionality in your application.

**Note:** The SDK requires Android API level 21 (Android 5.0) or higher.