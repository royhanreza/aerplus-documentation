---
sidebar_position: 4
---

# Development Setup

Panduan lengkap untuk setup development environment Aerplus Supplier dengan Flutter.

## Prerequisites

Pastikan Anda telah memenuhi semua [System Requirements](./system-requirements) sebelum melanjutkan.

## Step 1: Install Flutter SDK

### Download Flutter

1. Download Flutter SDK dari https://flutter.dev/docs/get-started/install
2. Extract ke lokasi yang diinginkan
3. Add Flutter ke PATH

**macOS/Linux**:
```bash
export PATH="$PATH:`pwd`/flutter/bin"

# Tambahkan ke ~/.zshrc atau ~/.bashrc untuk permanent
echo 'export PATH="$PATH:$HOME/development/flutter/bin"' >> ~/.zshrc
source ~/.zshrc
```

**Windows**:
- Add Flutter bin directory ke Environment Variables PATH
- Contoh: `C:\src\flutter\bin`

### Verify Installation

```bash
flutter doctor
```

Output akan show status dari:
- Flutter SDK
- Android toolchain
- Xcode (macOS only)
- Chrome
- IDE plugins
- Connected devices

## Step 2: Setup Platform-Specific Tools

### Android Setup

1. **Install Android Studio**
   - Download dari https://developer.android.com/studio
   - Install Android SDK, Android SDK Platform-Tools, Android SDK Build-Tools

2. **Configure Android SDK**
   ```bash
   # Set ANDROID_HOME environment variable
   export ANDROID_HOME=$HOME/Library/Android/sdk  # macOS
   export ANDROID_HOME=$HOME/Android/Sdk  # Linux
   # Windows: Set via Environment Variables GUI
   
   export PATH=$PATH:$ANDROID_HOME/tools
   export PATH=$PATH:$ANDROID_HOME/tools/bin
   export PATH=$PATH:$ANDROID_HOME/platform-tools
   ```

3. **Accept Android Licenses**
   ```bash
   flutter doctor --android-licenses
   ```

4. **Setup Android Emulator**
   - Open Android Studio → AVD Manager
   - Create Virtual Device
   - Select hardware (Pixel 4 recommended)
   - Select system image (API 30+ recommended)
   - Finish setup

### iOS Setup (macOS Only)

1. **Install Xcode**
   ```bash
   # Install dari App Store atau download dari developer.apple.com
   
   # Setup command line tools
   sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
   sudo xcodebuild -runFirstLaunch
   ```

2. **Install CocoaPods**
   ```bash
   sudo gem install cocoapods
   ```

3. **Setup iOS Simulator**
   ```bash
   # List available simulators
   xcrun simctl list devices
   
   # Open simulator
   open -a Simulator
   ```

## Step 3: Install IDE & Plugins

### Option 1: Android Studio

1. Install Android Studio
2. Install plugins:
   - Flutter plugin
   - Dart plugin
3. Restart Android Studio

### Option 2: Visual Studio Code

1. Install VS Code
2. Install extensions:
   - Flutter (by Dart Code)
   - Dart (by Dart Code)
3. Reload VS Code

### VS Code Settings (Optional)

Create `.vscode/settings.json`:
```json
{
  "dart.flutterSdkPath": "/path/to/flutter",
  "dart.lineLength": 120,
  "editor.formatOnSave": true,
  "editor.rulers": [80, 120]
}
```

## Step 4: Clone Repository

```bash
git clone <repository-url> aerplus_supplier
cd aerplus_supplier
```

## Step 5: Install Dependencies

```bash
# Get all packages
flutter pub get
```

Ini akan install semua packages yang listed di `pubspec.yaml`:
- GetX untuk state management
- Dio untuk HTTP client
- Image picker dan compress
- Dan lain-lain

## Step 6: Generate Code

Karena menggunakan Freezed dan JSON serialization, generate code:

```bash
# One-time generation
flutter pub run build_runner build

# Or watch mode (regenerate on changes)
flutter pub run build_runner watch

# Clean build (jika ada conflict)
flutter pub run build_runner build --delete-conflicting-outputs
```

## Step 7: Configure API Endpoint

Edit file `lib/src/services/api.dart` untuk set API base URL:

```dart
class ApiService {
  static const String baseUrl = "https://your-api-endpoint.com/api";
  
  // ... rest of the code
}
```

Atau buat environment configuration file.

## Step 8: Run Application

### Check Connected Devices

```bash
flutter devices
```

Output akan show available devices:
- Physical devices (connected via USB)
- Emulators/Simulators
- Chrome (for web)

### Run on Device/Emulator

```bash
# Run in debug mode (default)
flutter run

# Run on specific device
flutter run -d <device-id>

# Run in release mode
flutter run --release

# Run with hot reload enabled
flutter run --hot
```

### Hot Reload

Saat app running:
- Press `r` untuk hot reload
- Press `R` untuk hot restart
- Press `q` untuk quit

### Common Run Commands

```bash
# Android
flutter run  # If only Android device connected

# iOS (macOS only)
flutter run -d iPhone  # iPhone simulator
flutter run -d <device-id>  # Physical iPhone

# Web
flutter run -d chrome

# Windows (Windows only)
flutter run -d windows

# macOS (macOS only)
flutter run -d macos
```

## Step 9: Generate App Icons

Jika perlu regenerate icons:

```bash
flutter pub run flutter_launcher_icons
```

Icons akan generated dari `assets/app_icon/aerplus_supplier_icon_500.png`.

## Step 10: Generate Splash Screen

Jika perlu regenerate splash screen:

```bash
flutter pub run flutter_native_splash:create
```

Splash configuration ada di `pubspec.yaml` section `flutter_native_splash`.

## Development Workflow

### Project Structure Overview

```
aerplus_supplier/
├── android/           # Android native code
├── ios/              # iOS native code
├── lib/              # Dart source code
│   ├── core/         # Core utilities
│   ├── src/          # Main source
│   ├── main.dart     # Entry point
│   ├── theme.dart    # Theme config
│   └── util.dart     # Utilities
├── assets/           # Images, fonts, icons
├── test/             # Test files
└── pubspec.yaml      # Dependencies
```

### Hot Reload & Hot Restart

**Hot Reload** (r):
- Fast (~1 second)
- Preserves app state
- Updates UI changes
- Good for: UI tweaks, styling

**Hot Restart** (R):
- Slower (~5 seconds)
- Resets app state
- Rebuilds entire app
- Good for: Logic changes, initialization

### Debugging

**In Android Studio/VS Code**:
- Set breakpoints
- Click Debug button
- Use Debug console
- Inspect variables

**Flutter DevTools**:
```bash
# Start DevTools
flutter pub global activate devtools
flutter pub global run devtools

# Or via VS Code: Command Palette → "Flutter: Open DevTools"
```

Features:
- Widget inspector
- Timeline view
- Memory profiler
- Network monitor
- Logging

## Common Development Tasks

### Add New Package

1. Add to `pubspec.yaml`:
   ```yaml
   dependencies:
     new_package: ^1.0.0
   ```

2. Get package:
   ```bash
   flutter pub get
   ```

### Update Packages

```bash
# Check outdated packages
flutter pub outdated

# Update packages
flutter pub upgrade

# Update to major versions
flutter pub upgrade --major-versions
```

### Create New Screen

```dart
// lib/src/view/screen/new_screen.dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class NewScreen extends StatelessWidget {
  const NewScreen({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('New Screen')),
      body: const Center(child: Text('Hello World')),
    );
  }
}
```

### Add Route

```dart
// lib/core/app_route.dart
class AppRoute {
  static const String newScreen = '/new-screen';
  
  static List<GetPage> routes = [
    GetPage(name: newScreen, page: () => const NewScreen()),
    // ... other routes
  ];
}
```

### Create Controller

```dart
// lib/src/controller/new_controller.dart
import 'package:get/get.dart';

class NewController extends GetxController {
  // Observable state
  final count = 0.obs;
  
  // Method
  void increment() {
    count.value++;
  }
  
  @override
  void onInit() {
    super.onInit();
    // Initialize
  }
  
  @override
  void onClose() {
    // Cleanup
    super.onClose();
  }
}
```

## Troubleshooting

### Common Issues

**1. Flutter Doctor Issues**

```bash
flutter doctor -v  # Verbose output

# Fix Android licenses
flutter doctor --android-licenses

# Update Flutter
flutter upgrade
```

**2. Build Runner Conflicts**

```bash
# Clean and rebuild
flutter pub run build_runner clean
flutter pub run build_runner build --delete-conflicting-outputs
```

**3. Package Version Conflicts**

```bash
# Clean everything
flutter clean
flutter pub get
```

**4. iOS Pod Issues (macOS)**

```bash
cd ios
pod deintegrate
pod install
cd ..
```

**5. Gradle Issues (Android)**

```bash
cd android
./gradlew clean
cd ..
flutter clean
flutter pub get
```

**6. Cache Issues**

```bash
# Clear Flutter cache
flutter clean

# Clear pub cache
flutter pub cache clean
flutter pub get
```

### Performance Issues

```bash
# Build in profile mode for performance testing
flutter run --profile

# Analyze performance
flutter analyze
```

## Testing on Physical Devices

### Android Physical Device

1. Enable Developer Options:
   - Settings → About Phone
   - Tap "Build Number" 7 times

2. Enable USB Debugging:
   - Settings → Developer Options
   - Enable "USB Debugging"

3. Connect via USB:
   ```bash
   # Check device is detected
   flutter devices
   
   # Run on device
   flutter run
   ```

### iOS Physical Device (macOS)

1. Connect iPhone via USB
2. Trust computer on iPhone
3. Open Xcode → Window → Devices and Simulators
4. Select device
5. Run from Xcode or:
   ```bash
   flutter run -d <device-id>
   ```

## Development Best Practices

### Code Organization

- Keep widgets small dan focused
- Separate business logic ke controllers
- Use const constructors when possible
- Follow Flutter naming conventions

### State Management

- Use GetX reactive programming (`.obs`)
- Keep controllers thin
- Avoid business logic di widgets

### Performance

- Use `const` constructors
- Avoid unnecessary rebuilds
- Lazy load images
- Implement pagination

### Git Workflow

```bash
# Create feature branch
git checkout -b feature/new-feature

# Make changes
# Commit frequently
git add .
git commit -m "Description of changes"

# Push to remote
git push origin feature/new-feature

# Create PR untuk review
```

## Next Steps

Setelah development setup selesai:

1. **[Configuration](./configuration)** - Configure app settings
2. Test basic functionality
3. Implement features
4. Prepare untuk build production

---

**Selanjutnya**: [Configuration](./configuration)
