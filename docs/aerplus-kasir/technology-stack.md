---
sidebar_position: 3
---

# Technology Stack

Dokumen ini menjelaskan teknologi, framework, dan library yang digunakan dalam pengembangan Aerplus Kasir.

## Core Framework

### React Native

- **Version**: 0.72.5
- **Description**: Framework untuk membangun aplikasi mobile native menggunakan React
- **Website**: https://reactnative.dev
- **Why React Native**:
  - Single codebase untuk iOS dan Android
  - Performance mendekati native app
  - Hot reloading untuk development lebih cepat
  - Large ecosystem dan community support

### Expo

- **Version**: 49.0.13
- **Description**: Platform dan tools untuk React Native development
- **Website**: https://expo.dev
- **Key Features**:
  - Simplified development workflow
  - Over-the-air (OTA) updates
  - Easy access to native APIs
  - EAS (Expo Application Services) untuk build dan deployment

## JavaScript Runtime

### Hermes

- **Description**: JavaScript engine dioptimasi untuk React Native
- **Benefits**:
  - Faster app launch time
  - Reduced memory usage
  - Smaller app size
  - Better performance

## UI Framework & Components

### React

- **Version**: 18.2.0
- **Description**: JavaScript library untuk membangun user interfaces

### React Navigation

- **Version**: 6.x
- **Packages**:
  - `@react-navigation/native`: Core navigation
  - `@react-navigation/native-stack`: Stack navigator
  - `@react-navigation/bottom-tabs`: Bottom tabs navigation
  - `@react-navigation/material-bottom-tabs`: Material bottom tabs
  - `@react-navigation/material-top-tabs`: Material top tabs

### React Native Paper

- **Version**: 5.0.0
- **Description**: Material Design components untuk React Native
- **Components Used**:
  - Buttons, Cards, TextInput
  - Dialogs, Snackbars
  - DataTable, List items

### React Native Vector Icons

- **Version**: 9.2.0
- **Description**: Icon library dengan berbagai icon sets
- **Icon Sets**: Material Icons, FontAwesome, Ionicons, dll

## State Management

### Zustand

- **Version**: 4.2.0
- **Description**: Lightweight state management solution
- **Stores**:
  - `authStore`: Authentication state
  - `userStore`: User data
  - `productStore`: Products and inventory
  - `saleStore`: Sales transactions
  - `customerStore`: Customer data
  - `depositStore`: Deposit management
  - `expenseStore`: Expense tracking
  - `goodStore`: Goods management
  - `outletStore`: Outlet information
  - `transferStore`: Stock transfer

### React Query

- **Version**: 3.39.3
- **Description**: Data fetching and caching library
- **Features**:
  - Automatic caching
  - Background refetching
  - Request deduplication
  - Optimistic updates

## HTTP Client

### Axios

- **Version**: 1.3.0
- **Description**: Promise-based HTTP client
- **Usage**:
  - API requests ke backend
  - Request/response interceptors
  - Error handling
  - Timeout configuration

## Date & Time

### Day.js

- **Version**: 1.11.7
- **Description**: Lightweight date library
- **Features**:
  - Date parsing dan formatting
  - Relative time
  - Locale support
  - Plugin system

## Forms & Validation

### React Native Paper Dates

- **Version**: 0.12.0
- **Description**: Date picker component
- **Features**:
  - Date range picker
  - Single date picker
  - Material Design style

## Image Handling

### Expo Image Picker

- **Version**: 14.3.2
- **Description**: Access device camera dan photo library
- **Features**:
  - Camera capture
  - Gallery selection
  - Multiple image selection

### Expo Image Manipulator

- **Version**: 11.3.0
- **Description**: Image manipulation dan resizing
- **Features**:
  - Crop, rotate, flip
  - Resize dan compress
  - Format conversion

### Expo Media Library

- **Version**: 15.4.1
- **Description**: Save dan access device media library
- **Permissions**: Photo library access

## Barcode Scanning

### Expo Barcode Scanner

- **Version**: 12.5.3
- **Description**: Barcode dan QR code scanner
- **Supported Formats**:
  - EAN-13, EAN-8
  - UPC-A, UPC-E
  - Code 39, Code 128
  - QR Code

## Printing

### Expo Print

- **Version**: 12.4.2
- **Description**: Print documents dan receipts
- **Features**:
  - HTML to PDF conversion
  - Native print dialog
  - Bluetooth printer support (via third-party libraries)

## Storage

### Expo Secure Store

- **Version**: 12.3.1
- **Description**: Secure storage untuk sensitive data
- **Usage**:
  - Authentication tokens
  - User credentials
  - API keys

### Expo File System

- **Version**: 15.4.4
- **Description**: File system access
- **Features**:
  - Read/write files
  - Download files
  - File information

## Notifications

### Expo Notifications

- **Version**: 0.20.1
- **Description**: Local dan push notifications
- **Features**:
  - Schedule local notifications
  - Handle push notifications
  - Notification permissions
  - Custom notification sounds

## Device Information

### Expo Device

- **Version**: 5.4.0
- **Description**: Device information
- **Data**:
  - Device model dan manufacturer
  - OS version
  - Device name

### Expo Constants

- **Version**: 14.4.2
- **Description**: App constants dan configuration

### Expo Application

- **Version**: 5.3.0
- **Description**: Application information
- **Data**:
  - App version
  - Build number
  - Application ID

## UI/UX Libraries

### React Native Reanimated

- **Version**: 3.3.0
- **Description**: Advanced animations library
- **Features**:
  - Smooth 60 FPS animations
  - Gesture-driven animations
  - Worklet support

### React Native Gesture Handler

- **Version**: 2.12.0
- **Description**: Native touch gesture system

### React Native Safe Area Context

- **Version**: 4.6.3
- **Description**: Handle safe area insets (notch, status bar)

### @gorhom/bottom-sheet

- **Version**: 4.x
- **Description**: Performant bottom sheet component
- **Features**:
  - Smooth gestures
  - Customizable design
  - Keyboard handling

## List Performance

### @shopify/flash-list

- **Version**: 1.7.2
- **Description**: Fast and performant list component
- **Benefits**:
  - Better performance than FlatList
  - Reduced memory usage
  - Smooth scrolling

## Utility Libraries

### Immer

- **Version**: 9.0.17
- **Description**: Immutable state updates
- **Usage**: Simplify state management in Zustand stores

### Lodash.groupBy

- **Version**: 4.6.0
- **Description**: Group array elements
- **Usage**: Data transformation dan grouping

### React Native Mask Text

- **Version**: 0.13.1
- **Description**: Text masking untuk input
- **Usage**:
  - Phone number formatting
  - Currency formatting
  - Custom masks

### MIME

- **Version**: 3.0.0
- **Description**: MIME type detection
- **Usage**: File type detection

## Development Tools

### Metro Bundler

- **Description**: JavaScript bundler untuk React Native
- **Features**:
  - Fast refresh
  - Source maps
  - Code splitting

### Babel

- **Version**: 7.12.9+
- **Description**: JavaScript compiler
- **Config**: `babel.config.js`

## Build & Deployment

### EAS (Expo Application Services)

- **Description**: Build dan deployment services
- **Features**:
  - Cloud builds untuk iOS dan Android
  - OTA updates
  - Submit to stores

### EAS Build Profiles

Di `eas.json`:
- **development**: Development builds dengan debugging
- **preview**: Internal testing builds (APK untuk Android)
- **production**: Production builds untuk stores

## Platform-Specific

### iOS

- **Deployment Target**: iOS 13.4+
- **CocoaPods**: Dependency manager
- **Info.plist**: Permissions configuration

### Android

- **Target SDK**: 34 (Android 14)
- **Build Tools**: 34.0.0
- **Gradle**: Build system
- **Permissions**: Camera, storage, notifications

## API Integration

### Backend Integration

- **REST API**: Axios untuk HTTP requests
- **Authentication**: Token-based (JWT)
- **API Files** (di folder `api/`):
  - `auth.js`: Authentication endpoints
  - `product.js`: Product management
  - `sale.js`: Sales transactions
  - `customer.js`: Customer management
  - `stockOpname.js`: Stock opname
  - `transfer.js`: Stock transfers
  - `expense.js`: Expense tracking

## Code Quality (Optional)

### ESLint

- **Description**: JavaScript linting tool
- **Config**: `.eslintrc.js`

### Prettier

- **Description**: Code formatter
- **Config**: `.prettierrc`

## Version Control

### Git

- **Description**: Source control
- **Config**: `.gitignore`

## Performance Monitoring (Optional)

### Sentry

- **Description**: Error tracking and performance monitoring
- **Integration**: Via Sentry SDK for React Native

## Push Notifications (Optional)

### Firebase Cloud Messaging

- **Config**: `google-services.json` (Android)
- **Usage**: Push notifications via Firebase

---

**Selanjutnya**: [Development Setup](./development-setup)
