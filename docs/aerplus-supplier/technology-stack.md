---
sidebar_position: 3
---

# Technology Stack

Dokumen ini menjelaskan teknologi, framework, dan library yang digunakan dalam pengembangan Aerplus Supplier.

## Core Framework

### Flutter

- **Version**: 3.x (SDK &gt;=3.4.3 &lt;4.0.0)
- **Description**: Google's UI toolkit untuk building natively compiled applications
- **Website**: https://flutter.dev
- **Why Flutter**:
  - Single codebase untuk iOS, Android, Web, Desktop
  - Fast development dengan hot reload
  - Beautiful, native-like UI
  - High performance (compiled to native code)
  - Large ecosystem dan community

### Dart

- **Version**: 3.4.3+
- **Description**: Client-optimized programming language
- **Features**:
  - Strong typing dengan null safety
  - Async/await support
  - Modern syntax
  - AOT (Ahead-of-Time) compilation

## State Management & Architecture

### GetX

- **Version**: 4.6.6
- **Package**: `get`
- **Website**: https://pub.dev/packages/get
- **Usage**:
  - State management
  - Dependency injection
  - Route management
  - Dialog/Snackbar management
- **Why GetX**:
  - Minimal boilerplate
  - High performance
  - Easy to learn
  - Complete solution (state + routing + DI)

### Controllers

Located in `lib/src/controller/`:
- `auth_controller.dart`: Authentication state
- Other domain-specific controllers

## HTTP Client

### Dio

- **Version**: 5.7.0
- **Package**: `dio`
- **Description**: Powerful HTTP client untuk Dart
- **Features**:
  - Request/Response interceptors
  - Request cancellation
  - File uploading/downloading
  - Timeout handling
  - Error handling
  - Form data support
- **Usage**: API communication dengan backend Aerplus

## Data Serialization

### Freezed

- **Version**: 2.5.2
- **Package**: `freezed` + `freezed_annotation` (2.4.4)
- **Description**: Code generation untuk immutable classes
- **Features**:
  - Immutable models
  - Union types
  - Copy with method
  - Equality comparison
- **Usage**: Model classes untuk data structure

### JSON Serialization

- **Version**: 4.9.0 / 6.8.0
- **Packages**: `json_annotation` + `json_serializable`
- **Description**: JSON to Dart class serialization
- **Usage**: Parse API responses

## UI Components & Libraries

### Material Design

- **cupertino_icons**: 1.0.6
- Flutter's built-in Material Design widgets
- iOS-style widgets (Cupertino)

### Google Fonts

- **Version**: 6.2.1
- **Package**: `google_fonts`
- **Fonts Used**:
  - Inter (18pt, 24pt, 28pt variants)
  - Syne
- **Features**:
  - Easy integration
  - No manual font file management
  - Supports all Google Fonts

### Shimmer Loading

- **Version**: 3.0.0
- **Package**: `shimmer`
- **Description**: Shimmer effect untuk loading states
- **Usage**: Skeleton loading screens

### Loading Animation Widget

- **Version**: 1.3.0
- **Package**: `loading_animation_widget`
- **Description**: Beautiful loading animations
- **Usage**: Loading indicators

### Timelines

- **Version**: 0.1.0
- **Package**: `timelines`
- **Description**: Timeline UI component
- **Usage**: Delivery status tracking timeline

## List & Pagination

### Infinite Scroll Pagination

- **Version**: 4.0.0
- **Package**: `infinite_scroll_pagination`
- **Description**: Lazy loading dengan pagination
- **Features**:
  - Infinite scroll
  - Pull to refresh
  - Error handling
  - Empty state
- **Usage**: Water delivery list

## Image Handling

### Image Picker

- **Version**: 1.1.2
- **Package**: `image_picker`
- **Description**: Pick images dari camera atau gallery
- **Features**:
  - Camera capture
  - Gallery selection
  - Multiple images
  - Video support
- **Usage**: Upload delivery proof photos

### Flutter Image Compress

- **Version**: 2.3.0
- **Package**: `flutter_image_compress`
- **Description**: Compress images untuk upload
- **Features**:
  - Reduce file size
  - Maintain quality
  - Fast compression
- **Usage**: Optimize images before upload

## Storage & Persistence

### Shared Preferences

- **Version**: 2.3.2
- **Package**: `shared_preferences`
- **Description**: Key-value storage
- **Usage**:
  - Save authentication token
  - App settings
  - User preferences
  - Cache data

### Path Provider

- **Version**: 2.1.4
- **Package**: `path_provider`
- **Description**: Get device storage paths
- **Usage**: File storage locations

## Utilities

### Intl

- **Version**: 0.19.0
- **Package**: `intl`
- **Description**: Internationalization dan formatting
- **Features**:
  - Date/time formatting
  - Number formatting
  - Currency formatting
  - Localization
- **Usage**: Format tanggal untuk delivery schedule

### URL Launcher

- **Version**: 6.3.1
- **Package**: `url_launcher`
- **Description**: Launch URLs, phone calls, emails
- **Features**:
  - Open URLs in browser
  - Make phone calls
  - Send emails
  - Open maps
- **Usage**: Contact support, open external links

### Package Info Plus

- **Version**: 8.1.0
- **Package**: `package_info_plus`
- **Description**: App metadata
- **Usage**: Get app version, build number

## Native Configuration

### Flutter Native Splash

- **Version**: 2.4.1
- **Package**: `flutter_native_splash`
- **Description**: Generate native splash screens
- **Config**: Defined in `pubspec.yaml`
- **Features**:
  - Android 12+ support
  - Dark mode support
  - Custom branding

### Flutter Launcher Icons

- **Version**: 0.14.1
- **Package**: `flutter_launcher_icons`
- **Description**: Generate app icons untuk all platforms
- **Config**: Defined in `pubspec.yaml`
- **Platforms**: Android, iOS, Web, Windows, macOS

## Development Tools

### Build Runner

- **Version**: 2.4.11
- **Package**: `build_runner`
- **Description**: Code generation tool
- **Usage**: Generate Freezed dan JSON serialization code

```bash
# Generate code
flutter pub run build_runner build

# Watch for changes
flutter pub run build_runner watch
```

### Flutter Lints

- **Version**: 3.0.0
- **Package**: `flutter_lints`
- **Description**: Recommended lints for Flutter
- **Usage**: Code quality dan best practices

## Project Structure

```
lib/
├── core/
│   ├── app_asset.dart      # Asset paths
│   ├── app_icon.dart       # Icon definitions
│   └── app_route.dart      # Route definitions
├── src/
│   ├── controller/         # GetX controllers
│   ├── model/             # Data models (Freezed)
│   ├── services/          # API services
│   │   └── api.dart
│   └── view/
│       ├── screen/        # Screen widgets
│       └── widget/        # Reusable widgets
├── enums.dart             # App enums
├── main.dart              # Entry point
├── theme.dart             # App theme
└── util.dart              # Utility functions
```

## Architecture Pattern

### GetX Pattern

- **Controllers**: Business logic dan state management
- **Views**: UI components (Screens & Widgets)
- **Models**: Data structures dengan Freezed
- **Services**: API communication dengan Dio

### Separation of Concerns

- **Core**: App-wide constants dan configurations
- **Controller**: State management
- **Model**: Data structures
- **View**: UI layer
- **Services**: External communication

## Platform Support

### Android

- **Min SDK**: 21 (Android 5.0)
- **Target SDK**: Latest
- **Package**: `com.magentamediatama.aerplus_supplier`

### iOS

- **Deployment Target**: 12.0+
- **Bundle Identifier**: `com.magentamediatama.aerplusSupplier`

### Web

- **Supported**: Yes
- **Renderer**: CanvasKit atau HTML

### Desktop

- **Windows**: Supported
- **macOS**: Supported
- **Linux**: Supported

## API Integration

### REST API

- **Client**: Dio
- **Service Layer**: `lib/src/services/api.dart`
- **Authentication**: Token-based (saved in SharedPreferences)

### Endpoints

Centralized di `api.dart`:
- Authentication endpoints
- Water delivery endpoints
- Profile endpoints
- Image upload endpoints

## Performance Optimization

### Code Generation

- Freezed untuk immutable data classes
- JSON serialization dengan code generation
- Reduced runtime reflection

### Image Optimization

- Image compression sebelum upload
- Lazy loading dengan pagination
- Cached images

### State Management

- GetX untuk minimal rebuilds
- Efficient state updates
- Memory management

## Theme & Styling

### Custom Theme

Defined di `theme.dart`:
- Color schemes
- Typography
- Button styles
- Input decoration
- Card styles

### Fonts

- Inter (multiple weights)
- Syne (multiple weights)
- Loaded dari local assets

## Testing

### Unit Testing

- `flutter_test`: Included dengan Flutter SDK
- Test files: `test/widget_test.dart`

### Running Tests

```bash
# Run all tests
flutter test

# Run with coverage
flutter test --coverage
```

## Code Quality

### Linting

- `flutter_lints`: Enforces best practices
- Configuration: `analysis_options.yaml`

### Code Generation

- `build_runner`: Generate boilerplate code
- `freezed`: Immutable models
- `json_serializable`: JSON parsing

---

**Selanjutnya**: [Development Setup](./development-setup)
