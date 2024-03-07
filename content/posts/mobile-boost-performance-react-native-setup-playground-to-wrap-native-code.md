+++
title = 'Mobile: Boost Performance React Native Setup Playground to Wrap Native Code'
date = 2024-03-05T15:22:59+07:00
draft = true
+++

## Intro

This is the first part to boost your React Native app with native performance using native screen. Let's check this out!

## Prerequisites

Make sure you have the `react-native environment`, `Xcode`, and `Android Studio` installed to follow this thread. And you can either create a new React Native project or work with an existing one.

## Android

### Jetpack Compose Installation

Add installation [**Jetpack Compose**](https://developer.android.com/jetpack/compose) version at your project at `android/app/build.gradle`

```bash
...
def compose_version = '1.4.0'
```

```bash
android {
...
  buildFeatures {
      compose true
  }

  composeOptions {
      kotlinCompilerExtensionVersion compose_version
      kotlinCompilerVersion "1.8.0"
  }
}
```

```bash
dependencies {
    ...
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.11.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
    implementation "androidx.recyclerview:recyclerview:1.2.1"
    implementation "androidx.compose.ui:ui:$compose_version"
    implementation "androidx.compose.material:material:$compose_version"
    implementation "androidx.compose.ui:ui-tooling:$compose_version"
    implementation "androidx.activity:activity-compose:1.3.0-alpha03"
    implementation 'androidx.compose.ui:ui-text-android:1.6.1'
    implementation "androidx.navigation:navigation-compose:2.4.0-alpha01"
}
```

After this, sync the project and download the Jetpack Compose dependency using [**Sync Project at Android Studio**](https://www.youtube.com/watch?app=desktop&v=97OHx3JXzuI) or using command

> Note: Macbook user using ./gradlew for Windows using gradlew

```bash
cd android && ./gradlew assembleDebug
```

And you can run the project to check if the Jetpack Compose installation was successful.

#####

### Create Bridge Custom Module

> Note: The latest versions of React Native use Kotlin for native handling. If you're using an older version that uses Java, you can switch to Java and implement the necessary code in Java.

First, Navigate to `android/app/src/main/java/com/your-project` directory and create a file with name `CustomModule.kt`, `CustomModulePackage.kt` for handle bridge all native module for your app

> Tips: You can use ChatGPT to translate Kotlin code to Java code.

`CustomModule.kt`

```kotlin
package com.your-project

import android.content.Intent
import androidx.compose.runtime.Composable
import androidx.navigation.NavHostController
import com.facebook.react.bridge.Arguments
import com.facebook.react.bridge.ReactApplicationContext
import com.facebook.react.bridge.ReactContextBaseJavaModule
import com.facebook.react.bridge.ReactMethod
import com.facebook.react.bridge.ReadableArray

class CustomModule(reactContext: ReactApplicationContext) : ReactContextBaseJavaModule(reactContext) {
    private val reactContext: ReactApplicationContext = reactContext

    override fun getName(): String {
        return "CustomModule"
    }

    @ReactMethod
    fun checkBridge(data: ReadableArray) {
        println("Hello World")
    }
}
```

`CustomModulePackage.kt`

```kotlin
package com.your-project

import android.view.View
import com.facebook.react.ReactPackage
import com.facebook.react.bridge.NativeModule
import com.facebook.react.bridge.ReactApplicationContext
import com.facebook.react.uimanager.ReactShadowNode
import com.facebook.react.uimanager.ViewManager
import java.util.Collections

class CustomModulePackage: ReactPackage {
    override fun createNativeModules(reactContext: ReactApplicationContext): MutableList<NativeModule> {
        val modules: MutableList<NativeModule> = ArrayList()
        modules.add(CustomModule(reactContext))
        return modules
    }
}
```

And call the bridge custom module into `MainApplication.kt`

```kotlin
override fun getPackages(): List<ReactPackage> =
    PackageList(this).packages.apply {
        // Packages that cannot be autolinked yet can be added manually here
        add(CustomModulePackage())
    }
```

#####

## IOS

### Setup UIKit

Add setup [**UIKit**](https://developer.apple.com/documentation/uikit) by navigate to `ios/sinaureact/AppDelegate.h` and change all code with

```swift
#import <UserNotifications/UNUserNotificationCenter.h>
#import <React/RCTBridgeDelegate.h>
#import <UIKit/UIKit.h>

@interface AppDelegate : UIResponder <UIApplicationDelegate, RCTBridgeDelegate, UNUserNotificationCenterDelegate>

@property (nonatomic, strong) UIWindow *window;

@end
```

#####

### Create Bridge Custom Module

For ios the step is more easy than android, you only navigate to `ios` directory and create file `CustomModule.h`, `CustomModule.m` and fill the code with

`CustomModule.m`

```swift
#import "CustomModule.h"
#import <React/RCTLog.h>
#import <UIKit/UIKit.h>

@implementation CustomModule

RCT_EXPORT_MODULE();

RCT_EXPORT_METHOD(navigateToStatistic:()) {
    RCTLogInfo(@"Hello World");
}

@end
```

`CustomModule.h`

```swift
#import <Foundation/Foundation.h>
#import <React/RCTBridgeModule.h>

NS_ASSUME_NONNULL_BEGIN

@interface CustomModule : NSObject <RCTBridgeModule>

@end

NS_ASSUME_NONNULL_END
```

#####

## Checking the bridge work

After creating the bridge custom module you can call the module at React Native side and execute the native function at your clickable component

`bridge-utils.ts`

```typescript
import { NativeModules } from "react-native";

const { CustomModule } = NativeModules;

export const checkBridge = () => {
  CustomModule.checkBridge(data);
};
```

Voila, the log output will be **"Hello World"** at logcat Android Studio and Xcode

#####

## Next Step

The roadmap still on the track, you can follow this at the next thread

- Mobile: Boost Performance React Native Integrate With Navigating Between Native Screen
  - Passing parameter into native side
- Mobile: Boost Performance React Native Create Your First Native Screen
  - Jetpack Compose
  - Swift UIKIT
- Mobile: Boost Performance React Native Call Your Cache Data Across Native Screen
  - Deals with react-native-asyncstorage between native side
