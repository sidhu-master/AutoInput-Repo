# AutoInput Library

A powerful Android library for simulating comprehensive visual keyboard input and text automation, supporting both English and Chinese input methods via ML Kit OCR.

**Current Version**: `1.0.0`

## Features

*   **Visual Keyboard Simulation**: Uses ML Kit to scan the screen, identify keyboard keys, and simulate taps to input text.
*   **Chinese Input Support**: Automatically converts Chinese text to Pinyin, types it, and selects the correct candidate from the suggestion bar via OCR.
*   **Robust & Resilient**:
    *   Handles "Grouped" keys (e.g. `qwerty` detected as one block) by inferring key positions.
    *   Tolerates occasional OCR misses (continues typing instead of aborting).
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
            text = "ÄãºÃWorld", 
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

