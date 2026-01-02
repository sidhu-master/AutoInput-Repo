# AutoInput Library

A powerful Android library for simulating comprehensive visual keyboard input and text automation, supporting both English and Chinese input methods.

**Current Version**: `1.0.0`

## Features

*   **Visual Keyboard Simulation**: Intelligently scans the screen to identify keyboard keys and simulates natural taps to input text.
*   **Chinese Input Support**: Automatically handles Chinese text input, including Pinyin conversion, typing, and smart candidate selection from the suggestion bar.
*   **Robust & Resilient**:
    *   Handles complex keyboard layouts and "grouped" keys effectively.
    *   Tolerates occasional recognition variances to ensure continuous typing flow.
    *   Optimized logic for special keys (`Enter`, `Space`, `Search` in both English and Chinese).
*   **Privacy Focused**: Closed-source distribution via Maven Repo.

## Integration Guide

### 1. Add Repository

Add the following Maven repository specific to this library in your `settings.gradle` (or `settings.gradle.kts`) within the `dependencyResolutionManagement` block:

**Kotlin DSL (`settings.gradle.kts`):**
```kotlin
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        // Add AutoInput Repository
        maven { url = uri("https://raw.githubusercontent.com/sidhu-master/AutoInput-Repo/main") }
    }
}
```

**Groovy DSL (`settings.gradle`):**
```groovy
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        // Add AutoInput Repository
        maven { url "https://raw.githubusercontent.com/sidhu-master/AutoInput-Repo/main" }
    }
}
```

### 2. Add Dependency

Add the library dependency to your module`s `build.gradle` (usually `app/build.gradle`):

**Kotlin DSL:**
```kotlin
dependencies {
    implementation("com.sidhu.autoinput:library:1.0.0")
}
```

**Groovy DSL:**
```groovy
dependencies {
    implementation "com.sidhu.autoinput:library:1.0.0"
}
```

## Usage Example

The core class is `KeyboardAgent`. Typically, you would use it within an AccessibilityService context.

```kotlin
// 1. Initialize the agent with your AccessibilityService
val keyboardAgent = KeyboardAgent(service)

// 2. Perform text input
scope.launch {
    // Take a screenshot of the keyboard area
    val screenshot = takeScreenshot() 
    
    if (screenshot != null) {
        val success = keyboardAgent.type(
            text = "你好World", 
            screenshot = screenshot,
            screenshotProvider = { 
                // Callback to provide fresh screenshots for candidate selection (Crucial for Chinese)
                delay(500)
                takeScreenshot() 
            }
        )
        
        if (success) {
            Log.d("AutoInput", "Typing completed successfully!")
        }
    }
}
```

## Requirements

*   **Min SDK**: 30 (Android 11)
*   **Permissions**: Requires `AccessibilityService` permissions to dispatch gestures.

