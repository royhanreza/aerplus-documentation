---
sidebar_position: 5
---

# Configuration

Panduan konfigurasi Aerplus Supplier untuk development dan production.

## Application Configuration (pubspec.yaml)

File `pubspec.yaml` adalah konfigurasi utama untuk Flutter project.

### Basic Information

```yaml
name: aerplus_supplier
description: "A new Flutter project."
publish_to: "none"  # Private package
version: 1.0.0+1
```

**Version Format**: `MAJOR.MINOR.PATCH+BUILD_NUMBER`
- `1.0.0`: User-facing version (semantic versioning)
- `+1`: Build number (internal, increment setiap build)

### SDK Constraints

```yaml
environment:
  sdk: ">=3.4.3 <4.0.0"
```

### Dependencies Management

```yaml
dependencies:
  flutter:
    sdk: flutter
  
  # State Management
  get: ^4.6.6
  
  # HTTP Client
  dio: ^5.7.0
  
  # UI/UX
  google_fonts: ^6.2.1
  shimmer: ^3.0.0
  loading_animation_widget: ^1.3.0
  infinite_scroll_pagination: ^4.0.0
  timelines: ^0.1.0
  
  # Image Handling
  image_picker: ^1.1.2
  flutter_image_compress: ^2.3.0
  
  # Data Serialization
  freezed_annotation: ^2.4.4
  json_annotation: ^4.9.0
  
  # Utilities
  intl: ^0.19.0
  url_launcher: ^6.3.1
  path_provider: ^2.1.4
  shared_preferences: ^2.3.2
  package_info_plus: ^8.1.0
  
  # Icons
  cupertino_icons: ^1.0.6
```

## API Configuration

### Base URL Configuration

Edit `lib/src/services/api.dart`:

```dart
class ApiService {
  // Development
  static const String baseUrl = "http://localhost:8000/api";
  
  // Staging
  // static const String baseUrl = "https://staging-api.aerplus.com/api";
  
  // Production
  // static const String baseUrl = "https://api.aerplus.com/api";
  
  final Dio _dio = Dio(BaseOptions(
    baseUrl: baseUrl,
    connectTimeout: const Duration(seconds: 30),
    receiveTimeout: const Duration(seconds: 30),
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json',
    },
  ));
}
```

### Environment-Based Configuration

Create environment config file:

```dart
// lib/config/environment.dart
enum Environment {
  development,
  staging,
  production,
}

class EnvironmentConfig {
  static Environment currentEnvironment = Environment.development;
  
  static String get apiBaseUrl {
    switch (currentEnvironment) {
      case Environment.development:
        return 'http://localhost:8000/api';
      case Environment.staging:
        return 'https://staging-api.aerplus.com/api';
      case Environment.production:
        return 'https://api.aerplus.com/api';
    }
  }
  
  static bool get isProduction => 
      currentEnvironment == Environment.production;
  
  static bool get isDevelopment => 
      currentEnvironment == Environment.development;
}
```

Usage:

```dart
import 'package:aerplus_supplier/config/environment.dart';

final apiUrl = EnvironmentConfig.apiBaseUrl;
```

## Authentication Configuration

### Token Storage

Using SharedPreferences untuk save auth token:

```dart
// Save token
final prefs = await SharedPreferences.getInstance();
await prefs.setString('auth_token', token);

// Get token
final token = prefs.getString('auth_token');

// Remove token (logout)
await prefs.remove('auth_token');
```

### Auth Interceptor

Add token to all requests:

```dart
_dio.interceptors.add(InterceptorsWrapper(
  onRequest: (options, handler) async {
    final prefs = await SharedPreferences.getInstance();
    final token = prefs.getString('auth_token');
    
    if (token != null) {
      options.headers['Authorization'] = 'Bearer $token';
    }
    
    return handler.next(options);
  },
  onError: (error, handler) async {
    if (error.response?.statusCode == 401) {
      // Token expired, logout user
      // Navigate to login screen
    }
    return handler.next(error);
  },
));
```

## Theme Configuration

File `lib/theme.dart` contains theme definitions.

### Color Scheme

```dart
class AppColors {
  static const Color primary = Color(0xFF009688);
  static const Color secondary = Color(0xFF00796B);
  static const Color accent = Color(0xFF4CAF50);
  
  static const Color background = Color(0xFFF5F5F5);
  static const Color surface = Colors.white;
  
  static const Color error = Color(0xFFD32F2F);
  static const Color success = Color(0xFF388E3C);
  static const Color warning = Color(0xFFF57C00);
  
  static const Color textPrimary = Color(0xFF212121);
  static const Color textSecondary = Color(0xFF757575);
}
```

### Typography

```dart
import 'package:google_fonts/google_fonts.dart';

final textTheme = GoogleFonts.interTextTheme().copyWith(
  displayLarge: GoogleFonts.syne(
    fontSize: 32,
    fontWeight: FontWeight.bold,
  ),
  bodyLarge: GoogleFonts.inter(
    fontSize: 16,
    fontWeight: FontWeight.normal,
  ),
  // ... other text styles
);
```

### Theme Data

```dart
final lightTheme = ThemeData(
  useMaterial3: true,
  colorScheme: ColorScheme.fromSeed(
    seedColor: AppColors.primary,
    brightness: Brightness.light,
  ),
  textTheme: textTheme,
  appBarTheme: AppBarTheme(
    backgroundColor: AppColors.primary,
    foregroundColor: Colors.white,
    elevation: 0,
  ),
  // ... other theme properties
);
```

## App Icons Configuration

### Launcher Icons

Configuration di `pubspec.yaml`:

```yaml
flutter_launcher_icons:
  image_path: "assets/app_icon/aerplus_supplier_icon_500.png"
  
  android: "launcher_icon"
  image_path_android: "assets/app_icon/aerplus_supplier_icon_500.png"
  min_sdk_android: 21
  adaptive_icon_background: "assets/app_icon/aerplus_supplier_icon_500.png"
  adaptive_icon_foreground: "assets/app_icon/aerplus_supplier_icon_500.png"
  
  ios: true
  remove_alpha_channel_ios: true
  
  web:
    generate: true
    image_path: "assets/app_icon/aerplus_supplier_icon_500.png"
  
  windows:
    generate: true
    icon_size: 48
  
  macos:
    generate: true
```

Generate icons:

```bash
flutter pub run flutter_launcher_icons
```

**Icon Requirements**:
- PNG format
- 500x500 atau 1024x1024 pixels
- No transparency untuk iOS

## Splash Screen Configuration

Configuration di `pubspec.yaml`:

```yaml
flutter_native_splash:
  color: "#009688"
  image: assets/images/aerplus_supplier_splash_960.png
  branding: assets/images/aerplus_supplier_splash_960.png
  
  color_dark: "#009688"
  image_dark: assets/images/aerplus_supplier_splash_960.png
  
  android_12:
    image: assets/images/aerplus_supplier_splash_960.png
    icon_background_color: "#ffffff"
  
  web: false
  android: true
  ios: true
```

Generate splash:

```bash
flutter pub run flutter_native_splash:create
```

**Splash Image Requirements**:
- PNG format dengan transparency
- 960x960 pixels
- Logo centered

## Android Configuration

### App ID & Versioning

File: `android/app/build.gradle`

```gradle
android {
    namespace = "com.magentamediatama.aerplus_supplier"
    compileSdk = 34
    
    defaultConfig {
        applicationId = "com.magentamediatama.aerplus_supplier"
        minSdk = 21
        targetSdk = 34
        versionCode = flutterVersionCode.toInteger()
        versionName = flutterVersionName
    }
}
```

### Permissions

File: `android/app/src/main/AndroidManifest.xml`

```xml
<manifest>
    <!-- Internet permission -->
    <uses-permission android:name="android.permission.INTERNET" />
    
    <!-- Camera permission -->
    <uses-permission android:name="android.permission.CAMERA" />
    
    <!-- Storage permissions -->
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    
    <application
        android:label="Aerplus Supplier"
        android:icon="@mipmap/launcher_icon">
        <!-- ... -->
    </application>
</manifest>
```

### App Signing (Production)

Create `android/key.properties`:

```properties
storePassword=your_keystore_password
keyPassword=your_key_password
keyAlias=your_key_alias
storeFile=/path/to/your/keystore.jks
```

**Important**: Add `key.properties` to `.gitignore`!

Update `android/app/build.gradle`:

```gradle
def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}

android {
    signingConfigs {
        release {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
            storePassword keystoreProperties['storePassword']
        }
    }
    
    buildTypes {
        release {
            signingConfig signingConfigs.release
        }
    }
}
```

## iOS Configuration

### Bundle ID & Versioning

File: `ios/Runner/Info.plist`

```xml
<key>CFBundleIdentifier</key>
<string>com.magentamediatama.aerplusSupplier</string>

<key>CFBundleShortVersionString</key>
<string>$(FLUTTER_BUILD_NAME)</string>

<key>CFBundleVersion</key>
<string>$(FLUTTER_BUILD_NUMBER)</string>
```

### Permissions

File: `ios/Runner/Info.plist`

```xml
<!-- Camera -->
<key>NSCameraUsageDescription</key>
<string>App needs camera access to capture delivery photos</string>

<!-- Photo Library -->
<key>NSPhotoLibraryUsageDescription</key>
<string>App needs photo library access to select images</string>

<!-- Photo Library Add -->
<key>NSPhotoLibraryAddUsageDescription</key>
<string>App needs permission to save photos</string>
```

### Deployment Target

File: `ios/Podfile`

```ruby
platform :ios, '12.0'
```

## Route Configuration

File: `lib/core/app_route.dart`

```dart
import 'package:get/get.dart';
import 'package:aerplus_supplier/src/view/screen/login_screen.dart';
import 'package:aerplus_supplier/src/view/screen/home_screen.dart';
// ... import other screens

class AppRoute {
  static const String login = '/login';
  static const String home = '/home';
  static const String waterDelivery = '/water-delivery';
  static const String waterDeliveryDetail = '/water-delivery-detail';
  static const String profile = '/profile';
  static const String changePassword = '/change-password';
  
  static List<GetPage> routes = [
    GetPage(name: login, page: () => const LoginScreen()),
    GetPage(name: home, page: () => const HomeScreen()),
    GetPage(name: waterDelivery, page: () => const WaterDeliveryScreen()),
    // ... other routes
  ];
}
```

Usage:

```dart
// Navigate
Get.toNamed(AppRoute.home);

// Navigate with arguments
Get.toNamed(AppRoute.waterDeliveryDetail, arguments: {'id': 123});

// Navigate and remove all previous routes
Get.offAllNamed(AppRoute.home);

// Go back
Get.back();
```

## Asset Configuration

File: `pubspec.yaml`

```yaml
flutter:
  uses-material-design: true
  
  assets:
    - assets/google_fonts/
    - assets/images/
    - assets/icons/
    - assets/app_icon/
```

Usage di code:

```dart
// Images
Image.asset('assets/images/keep_water_transparent.png')

// Icons
Image.asset('assets/icons/box-truck.png')
```

## Logging Configuration

### Debug Logging

```dart
import 'dart:developer' as developer;

void log(String message, {String name = 'App'}) {
  if (!kReleaseMode) {  // Only log in debug mode
    developer.log(message, name: name);
  }
}

// Usage
log('User logged in', name: 'Auth');
```

### Dio Logging

```dart
import 'package:dio/dio.dart';

final dio = Dio();

// Add logger interceptor in debug mode
if (!kReleaseMode) {
  dio.interceptors.add(LogInterceptor(
    request: true,
    requestHeader: true,
    requestBody: true,
    responseHeader: true,
    responseBody: true,
    error: true,
  ));
}
```

## Build Flavors (Optional)

Create different build configurations:

### Android Flavors

`android/app/build.gradle`:

```gradle
android {
    flavorDimensions "default"
    productFlavors {
        development {
            dimension "default"
            applicationIdSuffix ".dev"
            versionNameSuffix "-dev"
        }
        staging {
            dimension "default"
            applicationIdSuffix ".stg"
            versionNameSuffix "-stg"
        }
        production {
            dimension "default"
        }
    }
}
```

Run with flavor:

```bash
flutter run --flavor development
flutter build apk --flavor production
```

## Performance Configuration

### Release Mode Optimization

Automatically enabled in release builds:
- Code minification
- Obfuscation
- Tree shaking
- AOT compilation

### Network Timeouts

```dart
final dio = Dio(BaseOptions(
  connectTimeout: const Duration(seconds: 30),
  receiveTimeout: const Duration(seconds: 30),
  sendTimeout: const Duration(seconds: 30),
));
```

## Checklist Konfigurasi

Sebelum production:

- [ ] Update `version` di `pubspec.yaml`
- [ ] Set production API URL
- [ ] Configure app icons
- [ ] Configure splash screen
- [ ] Set proper permissions (Android & iOS)
- [ ] Configure app signing (Android keystore, iOS certificates)
- [ ] Test di real devices (Android & iOS)
- [ ] Remove debug logs
- [ ] Test release build
- [ ] Prepare store listings

---

**Selanjutnya**: [Build & Deployment](./build-deployment)
