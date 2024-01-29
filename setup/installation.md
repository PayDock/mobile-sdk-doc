# Install the SDK

To begin using the SDK, you must first add it as a dependency to your app, integrating it into your existing application. A pre-requisite to using the SDK is setting up the Paydock API, the instructions for which are linked in this guide.  

## Setup the Paydock API Integration

Get up and running using the PayDock API in just a few minutes.

1. Signup for a sandbox account by [contacting PayDock](https://paydock.com/contact/). This gives you access to the admin portal.

2. Once you have an account, follow our [integration guide](https://docs.paydock.com/#getting-started) on how to configure and setup your server integration.

3. After authenticating your API keys, integrate the SDK into either your [iOS](#ios) or [Android](#android) application by following the instructions in this guide.

## Setup the Paydock iOS SDK

This guide walks you through how to integrate the Paydock Android SDK into your Android application. 

### Step 1: Configure Repository Access

You can configure repository access using either Swift Package Manager or CocoaPods.

Note:
Before you can access the Paydock SDK, you need to create a **Private-Token** for your merchant account. This token provides access to our private GitLab repository's package registry. Replace `<private_token>` in the instructions with your pre-generated **Private-Token**.

#### Swift Package Manager

1. In Xcode, select _File > Add Packages_. Enter the following:
```
https://gitlab.paydock.com/envoyrecharge/mobile-sdk-ios.git
```
2. Select the latest version number from our release page. 

3. Add the version number when you are prompted and click _Add Package_. 

#### CocoaPods

1. If you haven't already, install the latest version of CocoaPods.

2. If you don't have an existing Podfile, run the following command from the terminal in the project root folder to create one:

```bash
pod init
```

3. Add this line to your Podfile and replace the `<release_version>` in the following line:

```ruby
pod 'MobileSDK', :git => 'git@gitlab.paydock.com:envoyrecharge/mobile-sdk-ios.git', :tag => '<release_version>'
```

4. Run the following command in the terminal to install the dependency:

```bash
pod install
```
5. Now that the SDK is installed, use the .xcworkspace file to open your project in Xcode instead of the .xcodeproj file. 

### Step 2: Add SDK Dependency

Choose the project file where you want to access the SDK components. In this file, add the following import:

```Swift
import MobileSDK
```

5. The SDK has now been added to your project. To use the SDK in your project, you must include `import MobileSDK` at the top of the file where you will use the SDK. You can then move to the next step and [initialize](/setup/initialise) your MobileSDK.

## Setup the Paydock Android SDK

This guide walks you through the steps to add the Paydock Android MobileSDK into your Android application. 

### Step 1: Configure Repository Access

Before you can access the MobileSDK, you need to:

1. Create a **Private-Token** for your merchant account. This is your authentication, enabling you to access the MobileSDK private GitLab repository's package registry, which is where the dependencies for the MobileSDK are located. 

2. Replace `<private_token>` in the instructions with the **Private-Token** you have created in the previous step. 

The following examples in both Groovy and Kotlin demonstrate how this looks like. 

#### Project Level `build.gradle`

#### Groovy

```groovy
repositories {
  // ... other repositories
  maven {
      url "https://gitlab.paydock.com/api/v4/projects/329/packages/maven"
      name "Gitlab"
      credentials(HttpHeaderCredentials) {
          name = "Private-Token"
          value = "<private_token>"
      }
      authentication {
          header(HttpHeaderAuthentication)
      }
  }
}
```

#### Kotlin DSL
```kotlin
repositories {
    maven {
        url = uri("https://gitlab.repo.com/path")
        name = "Gitlab"
        credentials(HttpHeaderCredentials::class) {
            name = "Private-Token"
            value = "<private_token>"
        }
        authentication {
            create("header") { type = HttpHeaderAuthentication::class }
        }
    }
}
```

### Step 2: Add SDK Dependency

In your app/module level _build.gradle_, add the Paydock SDK dependency. Replace `<latest_version>` with the latest version of the SDK.

Note:
You can find the latest version of the SDK in our documentation or on GitLab.

#### Project Level `build.gradle`

#### Groovy
```groovy
dependencies {
  implementation 'com.paydock:paydock-sdk:<latest_version>'
}
```
#### Kotlin DSL
```kotlin
dependencies {
  implementation("com.paydock:paydock-sdk:<latest_version>")
}
```

That's it! You have successfully configured your project to use the Paydock Android SDK. You can now start using the SDK to integrate Paydock's payment processing capabilities into your Android application.
