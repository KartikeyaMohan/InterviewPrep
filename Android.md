# Android Questions

## 1. What is Android?

Android is an open-source mobile operating system developed by Google,
based on the Linux kernel. It provides APIs for building apps in Java,
Kotlin, or C++, and includes built-in frameworks, libraries, and tools
for UI, networking, data, and more.

## 2. What is Dalvik and ART? What's the difference?

-   **Dalvik**: The original Android runtime. It executes `.dex` files
    (Dalvik Executable) using a **Just-In-Time (JIT)** compiler.\
-   **ART (Android Runtime)**: Replaced Dalvik from Android 5.0
    (Lollipop). Uses **Ahead-Of-Time (AOT)** compilation --- apps are
    compiled to native code at install time, improving performance and
    reducing startup time.\
    **Difference**: ART is faster, more memory-efficient, and has better
    debugging and garbage collection.

## 3. How is MVVM better than the MVC architecture?

-   **MVC** tightly couples UI (View) and logic (Controller), making
    testing and scaling harder.
-   **MVVM** introduces a **ViewModel** that holds UI data and business
    logic separate from UI components.
-   **Benefits:**
    -   Lifecycle-aware (ViewModel survives configuration changes).
    -   Easier testing and maintainability.
    -   Cleaner separation of concerns.

## 4. How do ViewModels handle data consistency on screen configuration changes?

ViewModels survive configuration changes (like rotation) because they
are scoped to the lifecycle of the `Activity` or `Fragment`, not
recreated during configuration changes. This preserves data consistency
without manual handling (e.g., saving/restoring in
`onSaveInstanceState`).

## 5. What is the purpose of a Proguard file? What do you usually add in it?

Proguard shrinks, optimizes, and obfuscates the code to: - Reduce APK
size - Prevent reverse engineering\
You add rules for: - Keeping model classes used in reflection (e.g.,
JSON serialization) - Excluding libraries like Gson, Room, Retrofit
annotations\
Example:

``` proguard
-keep class com.example.model.** { *; }
-keepattributes Signature
```

## 6. How to run background tasks in Android efficiently?

-   Use **WorkManager** for deferrable, guaranteed background work
    (e.g., syncing data).
-   Use **Coroutines** or **Executors** for lightweight background
    threads.
-   Use **ForegroundService** for ongoing user-visible tasks (e.g.,
    music playback).
-   Avoid `AsyncTask` (deprecated).

## 7. What is the difference between background service and foreground service?

- **Background Service**: Runs tasks not visible to the user; may 
  be killed by system
- **Foreground Service**: Displays a persistent notification; not easily killed

## 8. What is the use of Broadcast Receivers?

Broadcast Receivers listen for system-wide or app-specific broadcast
messages (events).\
Examples: - System events: `BOOT_COMPLETED`, `CONNECTIVITY_CHANGE` -
Custom events between app components\
They allow your app to react even when not running in the foreground.

## 9. What are Content Providers?

Content Providers manage **shared app data** and offer a **standardized
interface** for querying or modifying it using URIs.\
Examples: - Accessing contacts or media files. - Sharing data between
apps.

## 10. What is Dependency Injection?

A design pattern where dependencies (objects) are **provided rather than
created** inside a class.\
Benefits: - Increases testability - Reduces coupling\
Frameworks: **Hilt**, **Dagger**, **Koin**

## 11. How to securely store information in an Android app/device?

-   Use **EncryptedSharedPreferences** or **EncryptedFile** for
    sensitive data.
-   Use **Android Keystore** for storing cryptographic keys.
-   Avoid hardcoding secrets or tokens in code or resources.

## 12. What is the difference between DataStore and SharedPreferences? What is better and why?

- **SharedPreferences**
    - API Type: Synchronous
    - Data Loss: Possible (commit failure)
    - Performance: Slower for large data
    - Thread Safety: Manual
- **DataStore**
    - API Type: Asynchronous (Coroutine/Flow based)
    - Data Loss: Safe and consistent
    - Performance: Better performance
    - Thread Safety: Automatic

**DataStore** (Proto or Preferences) is better --- modern, non-blocking,
and consistent.

## 13. Flow vs StateFlow vs SharedFlow vs LiveData vs Channel vs MutableLiveData

- **Flow**
    - Start: Cold
    - Replay: No
    - Lifecycle aware: No
    - Common Use: Stream data sequentially
- **StateFlow**
    - Start: Hot
    - Replay: Latest value
    - Lifecycle aware: No
    - Common Use: UI state management
- **SharedFlow**
    - Start: Hot
    - Replay: Configurable
    - Lifecycle aware: No
    - Common Use: One-to-many event sharing
- **LiveData**
    - Start: Hot
    - Replay: Latest value
    - Lifecycle aware: Yes
    - Common Use: UI data binding
- **MutableLiveData**
    - Mutable version of Live Data
    - Common Use: Update UI data
- **Channel**
    - Start: Cold-ish
    - Replay: One-time
    - Lifecycle aware: No
    - Common Use: Send one-shot events between coroutines

**Rule of thumb:** - Use **Flow** for streams from data sources.\
- Use **StateFlow** for state in ViewModels.\
- Use **SharedFlow** for UI events (e.g., toasts).\
- Use **LiveData** only for legacy/Jetpack binding.\
- Use **Channel** for one-time data transfer.

## 14. What are Coroutines? How are they different from threading?

Coroutines are **lightweight concurrency primitives** in Kotlin for
asynchronous tasks. - Suspend/resume instead of blocking threads. -
Thousands of coroutines can run on few threads.\
**Difference:** Threads are OS-level, expensive, and blocking.
Coroutines are language-level, cheaper, and non-blocking.

## 15. How to use biometrics or password protection of a device with the app?

Use **BiometricPrompt API** (supports fingerprint, face, PIN, pattern).\
Steps: 1. Create a `BiometricPrompt` instance.\
2. Show the authentication dialog.\
3. Handle callbacks in `onAuthenticationSucceeded()`.

## 16. How to do network calls securely?

-   Always use **HTTPS** (TLS).\
-   Validate SSL certificates (enable certificate pinning if needed).\
-   Store API keys securely (not in source code).\
-   Use libraries like **Retrofit + OkHttp + Moshi/Gson** with proper
    interceptors.

## 17. Difference between sealed, open, and abstract classes?

- **Sealed**
    - Restricts subclassing to same file (used for state/result classes)
    - Example usage: State Handling
- **Open**
    - Allows inheritance
    - Example usage: Extendable base classes
- **Abstract**
    - Cannot be instantiated, can contain abstract methods
    - Example usage: Template or base implementation

## 18. What is the purpose of `static`?

In Java, `static` means the member belongs to the **class, not an
instance**.\
Used for constants, utility methods, and singleton-like behaviors.\
In Kotlin, similar behavior is achieved via **companion objects**.

## 19. How to manage deeplinking in Android?

Use: - **Intent Filters** in `AndroidManifest.xml` for HTTP/URI
schemes. - **Navigation Component Deep Links** for in-app navigation. -
**App Links** (verified deep links) for HTTPS links that open directly
in the app.

## 20. What are the most common Jetpack components?

-   **Lifecycle**
-   **ViewModel**
-   **LiveData / StateFlow**
-   **Room** (database)
-   **WorkManager**
-   **Navigation Component**
-   **DataStore**
-   **Paging**
-   **CameraX**
-   **Compose** (modern UI toolkit)
