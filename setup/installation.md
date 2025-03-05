# Install the SDK

To begin using the SDK, you must first add it as a dependency to your app, integrating it into your existing application. A pre-requisite to using the SDK is setting up the Paydock API, the instructions for which are linked in this guide.  


## Setup the Paydock API Integration

Get up and running using the PayDock API in just a few minutes.

1. Signup for a sandbox account by [contacting PayDock](https://paydock.com/contact/). This gives you access to the admin portal.

2. Once you have an account, follow our [integration guide](https://docs.paydock.com/#getting-started) on how to configure and setup your server integration.

3. After authenticating your API keys, integrate the SDK into either your [iOS](#ios) or [Android](#android) application by following the instructions in this guide.

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

This guide walks you through the steps to add the Paydock Android MobileSDK into your Android application. 

### Step 1: Configure Repository Access

Before you can access the MobileSDK, you need to:

1. Create a [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) on your Github account. This is your authentication, enabling you to access the MobileSDK through Github's repository package registry. The package registry is where the dependencies for the MobileSDK are located. 

2. Replace the `<username>` in the instructions with your Github username. 

3. Replace the `<access_token>` in the instructions with the **Personal Access Token** you have created in step 1. 

The following examples in both Groovy and Kotlin demonstrate how this looks like. 

#### Project Level `build.gradle`

#### Groovy

```groovy
repositories {
  // ... other repositories
  maven {
      url "https://maven.pkg.github.com/Paydock/android-mobile-sdk"
      name "GitHubPackages"
      credentials {
            username "<username>"
            password "<access_token>"
      }
  }
}
```

#### Kotlin DSL
```kotlin
repositories {
    // ... other repositories
    maven {
        name = "GitHubPackages"
        url = uri("https://maven.pkg.github.com/Paydock/android-mobile-sdk")
        credentials(HttpHeaderCredentials) {
            username = "<username>"
            password = "<access_token>"
        }
    }
}
```

### Step 2: Add SDK Dependency

In your app/module level _build.gradle_, add the MobileSDK dependency. Replace `<latest_version>` with the latest version of the SDK.

Note:
You can find the latest version of the SDK in our documentation or on [Github](https://github.com/PayDock/android-mobile-sdk).

#### Project Level `build.gradle`

#### Groovy
```groovy
dependencies {
  implementation 'com.paydock:mobile-sdk:<latest_version>'
}
```
#### Kotlin DSL
```kotlin
dependencies {
  implementation("com.paydock:mobile-sdk:<latest_version>")
}
```

That's it! You have successfully configured your project to use the Paydock Android SDK. You can now start using the SDK to integrate Paydock's payment processing capabilities into your Android application.
