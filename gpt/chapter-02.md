# **Chapter 2: Setting Up the Development Environment**

## **Overview**

In this chapter, we'll set up the development environment required to work with the Player framework on Android using Kotlin and Jetpack Compose. We'll ensure that all necessary tools and dependencies are correctly configured so that you can follow along with the examples and exercises in subsequent chapters.

---

## **Objectives**

- Install and configure the required tools and technologies.
- Initialize a new Android project with Jetpack Compose support.
- Integrate the Player framework libraries into the project.
- Verify the setup by running a simple "Hello World" application.

---

## **2.1 Required Tools and Technologies**

Before we begin, make sure you have the following tools installed on your development machine:

### **2.1.1 Android Studio**

- **Version**: Android Studio Arctic Fox (2020.3.1) or later.
- **Download Link**: [Android Studio Download](https://developer.android.com/studio)

Android Studio is the official Integrated Development Environment (IDE) for Android app development, based on IntelliJ IDEA. It provides the fastest tools for building apps on every type of Android device.

### **2.1.2 Java Development Kit (JDK)**

- **Version**: JDK 8 or higher.

Android Studio includes an embedded JDK, but ensure that your system's JDK is up-to-date if you plan to use external tools that depend on it.

### **2.1.3 Kotlin**

Kotlin is the preferred language for Android app development. Android Studio comes with Kotlin support out of the box.

### **2.1.4 Jetpack Compose**

Jetpack Compose is Android's modern toolkit for building native UI. It simplifies and accelerates UI development on Android with less code, powerful tools, and intuitive Kotlin APIs.

- **Version**: Compose 1.0.5 or later.

### **2.1.5 Player Framework Libraries**

We will use the Player framework libraries to integrate dynamic content rendering into our Android application.

---

## **2.2 Project Initialization**

Let's create a new Android project with Jetpack Compose support.

### **2.2.1 Creating a New Project**

1. **Open Android Studio**.

2. **Start a New Project**:

   - On the welcome screen, click **"Create New Project"**.
   - If you have a project open, go to **File > New > New Project**.

3. **Select Project Template**:

   - In the **"Select a Project Template"** window, choose **"Empty Compose Activity"**.
   - Click **"Next"**.

4. **Configure Your Project**:

   - **Name**: `PlayerFrameworkTutorial`
   - **Package name**: `com.example.playerframeworktutorial`
   - **Save location**: Choose a suitable location on your machine.
   - **Language**: Select **Kotlin**.
   - **Minimum SDK**: API 21: Android 5.0 (Lollipop)
     - This ensures compatibility with a wide range of devices.
   - Ensure that **"Use legacy android.support libraries"** is unchecked.
   - Click **"Finish"**.

### **2.2.2 Project Structure**

Once the project is created, Android Studio will generate the basic structure, including:

- **MainActivity.kt**: The entry point of your application.
- **AndroidManifest.xml**: Application manifest file.
- **build.gradle (Project and Module)**: Gradle build scripts.

### **2.2.3 Verifying Jetpack Compose Setup**

Open `MainActivity.kt`. You should see code similar to the following:

```kotlin
package com.example.playerframeworktutorial

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.material.Text
import androidx.compose.material.MaterialTheme
import androidx.compose.material.Surface
import androidx.compose.runtime.Composable
import androidx.compose.ui.tooling.preview.Preview
import com.example.playerframeworktutorial.ui.theme.PlayerFrameworkTutorialTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            PlayerFrameworkTutorialTheme {
                // A surface container using the 'background' color from the theme
                Surface(color = MaterialTheme.colors.background) {
                    Greeting("Android")
                }
            }
        }
    }
}

@Composable
fun Greeting(name: String) {
    Text(text = "Hello $name!")
}

@Preview(showBackground = true)
@Composable
fun DefaultPreview() {
    PlayerFrameworkTutorialTheme {
        Greeting("Android")
    }
}
```

### **2.2.4 Running the Default App**

To ensure everything is set up correctly:

1. **Connect a Device or Start an Emulator**:

   - Connect your Android device via USB and enable USB debugging.
   - Or use an Android Virtual Device (AVD) emulator.

2. **Run the App**:

   - Click the **"Run"** button (green arrow) in the toolbar.
   - Select your device or emulator.

3. **Verify Output**:

   - The app should launch and display "Hello Android!" on the screen.

---

## **2.3 Integrating the Player Framework Libraries**

Now that we have a basic Android project with Jetpack Compose support, we'll integrate the Player framework libraries.

### **2.3.1 Understanding the Player Framework Libraries**

The Player framework consists of several modules:

- **player-core**: The core engine that processes content.
- **player-android**: Android-specific implementations and utilities.
- **player-plugins**: Collection of plugins to extend the framework's functionality.
- **player-reference-assets**: Reference implementations of assets for demonstration purposes.

### **2.3.2 Adding Maven Repository**

First, we need to ensure that the repositories hosting the Player framework libraries are included in our project.

1. **Open `build.gradle` (Project level)**:

   ```gradle
   // Project-level build.gradle
   buildscript {
       repositories {
           google()
           mavenCentral()
           // Add any other required repositories
       }
       dependencies {
           classpath "com.android.tools.build:gradle:7.0.3"
           classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:1.5.31"
       }
   }

   allprojects {
       repositories {
           google()
           mavenCentral()
           // Add any other required repositories
       }
   }
   ```

   Ensure that `mavenCentral()` is included in the `repositories` sections.

2. **Sync the Project**:

   - Click **"Sync Now"** to ensure the repositories are updated.

### **2.3.3 Adding Player Framework Dependencies**

Next, we'll add the required dependencies to our app module.

1. **Open `build.gradle` (Module: app)**:

   ```gradle
   // Module-level build.gradle
   plugins {
       id 'com.android.application'
       id 'kotlin-android'
   }

   android {
       compileSdkVersion 31

       defaultConfig {
           applicationId "com.example.playerframeworktutorial"
           minSdkVersion 21
           targetSdkVersion 31
           versionCode 1
           versionName "1.0"

           testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
       }

       buildTypes {
           release {
               minifyEnabled false
               proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
           }
       }

       buildFeatures {
           compose true
       }

       composeOptions {
           kotlinCompilerExtensionVersion '1.0.5'
       }

       packagingOptions {
           resources {
               excludes += '/META-INF/{AL2.0,LGPL2.1}'
           }
       }
   }

   dependencies {
       implementation "androidx.core:core-ktx:1.6.0"
       implementation "androidx.compose.ui:ui:1.0.5"
       implementation "androidx.compose.material:material:1.0.5"
       implementation "androidx.compose.ui:ui-tooling-preview:1.0.5"
       implementation "androidx.activity:activity-compose:1.3.1"

       // Player framework dependencies
       implementation "com.intuit.player:player-core:1.0.0"
       implementation "com.intuit.player:player-android:1.0.0"
       implementation "com.intuit.player:player-plugins:1.0.0"
       implementation "com.intuit.player:player-reference-assets:1.0.0"

       // Other dependencies
       implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.5.2"

       // Testing dependencies
       testImplementation 'junit:junit:4.13.2'
       androidTestImplementation 'androidx.test.ext:junit:1.1.3'
       androidTestImplementation 'androidx.test.espresso:espresso-core:3.4.0'
       androidTestImplementation "androidx.compose.ui:ui-test-junit4:1.0.5"
   }
   ```

   **Note**: Replace the version numbers (`1.0.0`, `1.5.2`, etc.) with the latest versions available at the time of setup.

2. **Sync the Project**:

   - Click **"Sync Now"** to download and integrate the dependencies.

### **2.3.4 Resolving Dependency Issues**

If you encounter any dependency conflicts or issues:

- Ensure that the version numbers are compatible.
- Check for any transitive dependencies that might conflict.
- Consult the Player framework documentation or repositories for any specific setup instructions.

### **2.3.5 Verifying the Integration**

To verify that the Player framework is correctly integrated:

1. **Import a Player Framework Class**:

   Open `MainActivity.kt` and add the following import statement:

   ```kotlin
   import com.intuit.player.android.AndroidPlayer
   ```

   If Android Studio recognizes the `AndroidPlayer` class without errors, the dependencies are correctly integrated.

2. **Initialize the Player Instance**:

   We'll test the integration by initializing a basic `AndroidPlayer` instance.

   ```kotlin
   class MainActivity : ComponentActivity() {
       private lateinit var player: AndroidPlayer

       override fun onCreate(savedInstanceState: Bundle?) {
           super.onCreate(savedInstanceState)
           setContent {
               PlayerFrameworkTutorialTheme {
                   Surface(color = MaterialTheme.colors.background) {
                       Greeting("Android")
                   }
               }
           }

           // Initialize the Player
           player = AndroidPlayer()
       }
   }
   ```

   If there are no compilation errors, the Player framework is successfully integrated.

---

## **2.4 Running a Simple "Hello World" Compose Application**

Before moving on, let's ensure that our environment is fully functional by running a simple "Hello World" application using Jetpack Compose.

### **2.4.1 Updating the Greeting Composable**

In `MainActivity.kt`, update the `Greeting` composable function to display a custom message.

```kotlin
@Composable
fun Greeting(name: String) {
    Text(text = "Welcome to the Player Framework Tutorial, $name!")
}
```

### **2.4.2 Updating the Preview**

Update the `DefaultPreview` function accordingly.

```kotlin
@Preview(showBackground = true)
@Composable
fun DefaultPreview() {
    PlayerFrameworkTutorialTheme {
        Greeting("Developer")
    }
}
```

### **2.4.3 Running the App**

1. **Run the App**:

   - Click the **"Run"** button.

2. **Verify Output**:

   - The app should display: "Welcome to the Player Framework Tutorial, Developer!"

3. **Check the Preview**:

   - In Android Studio, you can also check the preview in the design editor to see how the UI looks.

---

## **Exercises**

### **Exercise 2.1: Set Up a New Android Project with the Necessary Dependencies**

- **Objective**: Reinforce the steps for setting up a new project and integrating the Player framework.

- **Instructions**:

  1. Create a new Android project named `PlayerFrameworkExercise`.
  2. Configure the project to use Jetpack Compose.
  3. Add the Player framework dependencies to the project.
  4. Initialize an `AndroidPlayer` instance in the `MainActivity`.

- **Verification**:

  - Ensure that there are no compilation errors.
  - Verify that the `AndroidPlayer` class is recognized.

### **Exercise 2.2: Verify the Environment by Running a Simple "Hello World" Compose Application**

- **Objective**: Confirm that the development environment is correctly set up.

- **Instructions**:

  1. Modify the `MainActivity` to display a custom welcome message using a composable function.
  2. Run the application on an emulator or physical device.
  3. Observe the output and ensure it matches your expectations.

- **Verification**:

  - The app should display your custom message.
  - There should be no runtime errors.

---

## **Summary**

In this chapter, we have:

- Installed and configured the necessary tools and technologies.
- Created a new Android project with Jetpack Compose support.
- Integrated the Player framework libraries into the project.
- Verified the setup by running a simple "Hello World" application.

With the development environment set up, you're now ready to delve deeper into the Player framework. In the next chapter, we'll explore the layered architecture of the Player framework, understanding how its core components interact and contribute to dynamic content rendering.

---

## **Next Steps**

- **Proceed to Chapter 3**: We'll take a deep dive into the Player framework's architecture, examining the layers and components that make it a powerful tool for dynamic content rendering.

- **Preparation**: Familiarize yourself with Kotlin and Jetpack Compose if needed. Ensure that your development environment is functioning correctly.

If you have any questions or encounter any issues during the setup process, consult the official documentation or seek assistance from the community.

---
