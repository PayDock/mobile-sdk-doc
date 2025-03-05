# Install the SDK

To begin using the SDK, you must first add it as a dependency to your app, integrating it into your existing application. A pre-requisite to using the SDK is setting up the Paydock API, the instructions for which are linked in this guide.  


## Setup the Paydock API Integration

Get up and running using the PayDock API in just a few minutes.

1. Signup for a sandbox account by [contacting PayDock](https://paydock.com/contact/). This gives you access to the admin portal.

2. Once you have an account, follow our [integration guide](https://docs.paydock.com/#getting-started) on how to configure and setup your server integration.

3. After authenticating your API keys, integrate the SDK into either your [iOS](#setup-the-paydock-ios-sdk) or [Android](#setup-the-paydock-android-sdk) application by following the instructions in this guide.

## Setup the Paydock iOS SDK

This guide walks you through how to integrate the Paydock Android SDK into your Android application. 

### Step 1: Configure Repository Access

You can configure repository access using either Swift Package Manager or CocoaPods.

#### Swift Package Manager

1. In Xcode, select _File > Add Packages_. Enter the following:
```
https://github.com/PayDock/ios-mobile-sdk
```
2. Select the latest version number from our release page. 

3. Add the version number when you are prompted and click _Add Package_. 

### Step 2: Add SDK Dependency

Choose the project file where you want to access the SDK components. In this file, add the following import:

```Swift
import MobileSDK
```

The SDK has now been added to your project. To use the SDK in your project, you must include `import MobileSDK` at the top of the file where you will use the SDK. You can then move to the next step and [initialize](/setup/initialise) your MobileSDK.

## Setup the Paydock Android SDK

This guide walks you through the steps to add the Paydock Android MobileSDK into your Android application. Follow the instructions below to ensure a smooth setup process.

### Step 1: Configure Repository Access

The Paydock Android SDK is publicly available via [Jitpack](https://www.jitpack.io/#PayDock/android-mobile-sdk), eliminating the need for authentication. To include it in your project, you must add the Jitpack repository to your project-level configuration.

#### Updating the Project-Level `build.gradle` (Groovy)

Add the following Jitpack repository to your `build.gradle` file at the project level:

```groovy
allprojects {
    repositories {
        // ... other repositories
        maven { url 'https://jitpack.io' }
    }
}
```

#### Updating the Project-Level `build.gradle` (Kotlin DSL)

If you are using Kotlin DSL, modify your `settings.gradle.kts` file as shown below:

```kotlin
dependencyResolutionManagement {
    repositories {
        // Include Jitpack to fetch the SDK
        maven("https://jitpack.io")
    }
}
```

This ensures that Gradle can resolve dependencies from Jitpack.

### Step 2: Add the Paydock SDK Dependency

Once Jitpack is configured, you need to add the Paydock MobileSDK as a dependency in your module-level `build.gradle` file.

#### Finding the Latest Version

You can always find the latest version of the SDK on [Jitpack](https://www.jitpack.io/#PayDock/android-mobile-sdk). Replace `<latest_version>` in the examples below with the most recent version available.

You can find the latest version of the SDK on .

#### Adding the Dependency in build.gradle (Groovy)

#### Groovy
```groovy
dependencies {
    // Add the Paydock SDK dependency
    implementation 'com.github.PayDock:android-mobile-sdk:<latest_version>'
}
```

#### Adding the Dependency in build.gradle.kts (Kotlin DSL)
```kotlin
dependencies {
    // Add the Paydock SDK dependency
    implementation("com.github.PayDock:android-mobile-sdk:<latest_version>")
}
```

### Step 3: Sync and Verify

After adding the dependency, sync your Gradle project to fetch the required files. Ensure that the dependency is correctly resolved before proceeding with development.

That's it! Your project is now set up to use the Paydock Android SDK via Jitpack. You can start integrating Paydock's payment processing features into your Android application.