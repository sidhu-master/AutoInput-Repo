# AutoInput Library

A powerful Android library for simulating comprehensive visual keyboard input and text automation, supporting both English and Chinese input methods.

**Current Version**: `1.1.0`

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
    implementation("com.sidhu.autoinput:library:1.1.0")
}
```

**Groovy DSL:**
```groovy
dependencies {
    implementation "com.sidhu.autoinput:library:1.1.0"
}
```

## Usage Example

The core class is `KeyboardAgent`. Use it within an AccessibilityService context.

```kotlin
// 1. Initialize the agent with your AccessibilityService
val keyboardAgent = KeyboardAgent(service)

// 2. Simple text input (auto handles screenshot and segmentation)
scope.launch {
    val success = keyboardAgent.type("你好World")
    if (success) {
        Log.d("AutoInput", "Typing completed!")
    }
}

// 3. Sentence input with segmentation mode
scope.launch {
    // SMART mode: auto segments Chinese text for better candidate matching
    keyboardAgent.typeSentence("今天天气真好", KeyboardAgent.SegmentMode.SMART)
    
    // BY_CHAR mode: input character by character
    keyboardAgent.typeSentence("你好", KeyboardAgent.SegmentMode.BY_CHAR)
    
    // NO_SEGMENT mode: treat as single word
    keyboardAgent.typeSentence("Hello", KeyboardAgent.SegmentMode.NO_SEGMENT)
}

// 4. Input pre-segmented words
scope.launch {
    val segments = listOf("今天", "天气", "真好")
    keyboardAgent.typeSegments(segments)
}
```

### Segmentation Modes

| Mode | Description |
|------|-------------|
| `SMART` | Auto segments Chinese text intelligently (recommended) |
| `ACCURATE` | More accurate segmentation, slightly slower |
| `BY_CHAR` | Input character by character |
| `NO_SEGMENT` | No segmentation, treat input as single word |

## Requirements

*   **Min SDK**: 30 (Android 11)
*   **Permissions**: Requires `AccessibilityService` permissions to dispatch gestures.

