# GNSS Sharing System

English | [中文](README_zh.md)

A client-server Android application system that shares GNSS location data from a smartphone to a car multimedia system over WiFi hotspot connection.

## System Overview

**Server (Smartphone):**
- Collects high-precision GNSS data
- Runs as background service with foreground notification
- Streams location updates to connected clients via TCP
- Shows debugging information in notification panel
- Automatically manages power consumption
- Handles multiple client connections with individual heartbeat monitoring

**Client (Car Multimedia System):**
- Implements robust connection management with auto-reconnection
- Uses WiFi-aware reconnection
- Receives location data and provides system-wide mock GPS
- Shows detailed debugging information in main activity
- Displays connection status and reconnection attempts

## Setup Instructions

### Development Environment

- **JDK:** 21 (declared as the Gradle daemon toolchain in `gradle/gradle-daemon-jvm.properties`; provisioned automatically on the first build, so a local JDK 21 installation is not strictly required)
- **Gradle:** 9.3.1 via the included wrapper — no local Gradle installation needed
- **Android Gradle Plugin:** 9.1.0
- **Android SDK:** API 36 (compileSdk / targetSdk); install via Android Studio or the command-line tools and either set the `ANDROID_HOME` environment variable or create a `local.properties` file with `sdk.dir=<path to SDK>`
- **minSdk:** 24 (server app) / 28 (client app)

### Building the Applications

1. **Clone and Setup:**
```bash
git clone 
cd gnss-share
```

2. **Build Server & Client Apps (debug):**
```bash
./gradlew assembleDebug
```

3. **Build signed release APKs** (requires `keystore.jks` in the repository root and the `KEY_PASSWORD` environment variable):
```bash
./gradlew assembleRelease
```

Output APKs are written to:

- `client-app/build/outputs/apk/<variant>/gnss-client-<version>.apk`
- `server-app/build/outputs/apk/<variant>/gnss-server-<version>.apk`

The app version is taken from the `VERSION_NAME` environment variable (for example `v2.10.2`); when unset, the default from the root `build.gradle` is used. Signed release APKs are also built automatically by GitHub Actions whenever a `v*` tag is pushed.

## Usage Instructions

### Starting the System

1. **On Smartphone (Server):**
   - Launch "GNSS Server" app
   - Grant all requested permissions
   - Tap "Start Server"
   - Check notification shows "Server Running"
   - The service will continue running in background

2. **On Car System (Client):**
   - Ensure WiFi is connected to smartphone hotspot
   - Launch "GNSS Client" app
   - Grant all requested permissions
   - App will automatically connect to server (watch for connection toast)
   - Verify mock location provider is active

### Language Switching

Both apps support English and Chinese. Tap the language button in the top-right corner of the main screen to switch: the button shows the target language name ("中文" in the English UI, "English" in the Chinese UI). The choice is saved and restored on the next launch; by default the apps follow the system language.

### Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

### License

This project is licensed under [GNU General Public License v3.0](LICENSE).
