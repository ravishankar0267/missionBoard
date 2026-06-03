# Mobile App - Kotlin Multiplatform Mobile

## Tech Stack
- **Kotlin Multiplatform Mobile (KMM)**: Shared code for iOS & Android
- **Jetpack Compose**: Modern Android UI
- **SwiftUI**: iOS UI framework
- **Koin**: Dependency injection
- **Ktor Client**: HTTP client
- **Sentry**: Error logging and crash reporting
- **Kotlinx Serialization**: JSON serialization

## Project Structure

```
app/
├── shared/                          # Shared KMM code
│   ├── src/
│   │   ├── commonMain/             # Common code (Android & iOS)
│   │   │   ├── kotlin/
│   │   │   │   ├── data/
│   │   │   │   │   ├── api/        # API clients
│   │   │   │   │   ├── models/     # Data models
│   │   │   │   │   └── repository/ # Data repositories
│   │   │   │   ├── domain/
│   │   │   │   │   └── usecase/    # Business logic
│   │   │   │   ├── presentation/   # Shared UI logic
│   │   │   │   └── di/            # Dependency injection
│   │   │   └── resources/
│   │   ├── androidMain/            # Android-specific code
│   │   ├── iosMain/                # iOS-specific code
│   │   └── commonTest/             # Shared tests
│   ├── build.gradle.kts
│   └── gradle.properties
│
├── androidApp/                      # Android app
│   ├── src/
│   │   ├── main/
│   │   │   ├── kotlin/
│   │   │   │   ├── com/
│   │   │   │   │   └── missionboard/
│   │   │   │   │       ├── MainActivity.kt
│   │   │   │   │       ├── screens/   # Screen components
│   │   │   │   │       ├── ui/        # UI components
│   │   │   │   │       ├── viewmodel/ # ViewModels
│   │   │   │   │       └── utils/     # Utilities
│   │   │   └── AndroidManifest.xml
│   │   └── res/
│   ├── build.gradle.kts
│   └── proguard-rules.pro
│
├── iosApp/                          # iOS app (Swift)
│   ├── MissionBoard/
│   │   ├── Screens/
│   │   ├── Components/
│   │   ├── ViewModels/
│   │   └── Utils/
│   ├── MissionBoard.xcodeproj/
│   └── Podfile
│
├── build.gradle.kts
├── settings.gradle.kts
├── gradle.properties
└── README.md                        # This file
```

## Setup

### Prerequisites
- Kotlin 1.9+
- Android Studio 2023.1+
- Xcode 14+ (for iOS)
- JDK 11+

### Configuration

1. **Clone and navigate**:
```bash
cd missionBoard/app
```

2. **Install dependencies**:
```bash
# Android
./gradlew build

# iOS
cd iosApp
pod install
```

3. **Configure Sentry**:
- See [Error Logging Setup](#error-logging-setup)

## Development

### Android Development

```bash
# Run on emulator
cd androidApp
./gradlew installDebug

# Or open in Android Studio and run
```

### iOS Development

```bash
# Open project
cd iosApp
open MissionBoard.xcworkspace

# Run in Xcode
```

## Error Logging Setup

See [SENTRY_SETUP.md](SENTRY_SETUP.md) for complete Sentry configuration.

## Key Dependencies

### Shared (build.gradle.kts)
```kotlin
val kotlinVersion = "1.9.0"
val composeVersion = "1.5.0"

commonMain.dependencies {
    // Core
    implementation("org.jetbrains.kotlin:kotlin-stdlib:$kotlinVersion")
    
    // Networking
    implementation("io.ktor:ktor-client-core:2.3.0")
    implementation("io.ktor:ktor-client-json:2.3.0")
    
    // Serialization
    implementation("org.jetbrains.kotlinx:kotlinx-serialization-json:1.5.1")
    
    // DI
    implementation("io.insert-koin:koin-core:3.4.0")
    
    // Error tracking
    implementation("io.sentry:sentry:7.0.0")
    
    // Testing
    implementation("junit:junit:4.13.2")
    implementation("org.jetbrains.kotlin:kotlin-test:$kotlinVersion")
}
```

## Features

- ✅ Google OAuth authentication
- ✅ Course browsing and enrollment
- ✅ Progress tracking
- ✅ Video playback
- ✅ PDF viewing
- ✅ Certificate viewing
- ✅ Offline mode (cached data)
- ✅ Push notifications
- ✅ Error reporting with Sentry

## Testing

```bash
# Run shared tests
./gradlew commonTest

# Run Android tests
./gradlew androidTest

# Run iOS tests
cd iosApp && xcodebuild test
```

## Deployment

### Android
- Build: `./gradlew assembleRelease`
- Sign: Configure signing in `build.gradle.kts`
- Upload: Google Play Console

### iOS
- Build: Xcode build settings
- Archive: Product → Archive
- Submit: App Store Connect

## Error Logging & Monitoring

The app uses Sentry for:
- **Crash reporting** - Automatic crash detection
- **Error tracking** - Custom error logging
- **Performance monitoring** - Slow transaction detection
- **Release tracking** - Track issues per release

See [SENTRY_SETUP.md](SENTRY_SETUP.md) for detailed setup.

## Contributing

1. Follow the code structure above
2. Write tests for new features
3. Test on both Android and iOS
4. Document new screens/features
5. Commit with meaningful messages

## References

- [Kotlin Multiplatform Docs](https://kotlinlang.org/docs/multiplatform.html)
- [Jetpack Compose](https://developer.android.com/jetpack/compose)
- [SwiftUI](https://developer.apple.com/swiftui/)
- [Sentry Documentation](https://docs.sentry.io/)
- [Ktor Client](https://ktor.io/docs/client.html)
