---
sidebar_position: 2
---

# System Requirements

Dokumen ini menjelaskan persyaratan sistem untuk development dan deployment Aerplus Supplier.

## Development Requirements

### Operating System

Flutter support development di:
- **Windows**: Windows 10 atau lebih baru (64-bit)
- **macOS**: macOS 10.14 (Mojave) atau lebih baru
- **Linux**: Ubuntu 20.04+ atau distribusi Linux 64-bit lainnya

### Flutter SDK

- **Flutter**: 3.0 atau lebih baru (recommended: latest stable)
- **Dart SDK**: 3.4.3 atau lebih baru (included dengan Flutter)

Install Flutter:
```bash
# Download Flutter SDK dari https://flutter.dev
# Extract dan tambahkan ke PATH

# Verify installation
flutter --version
dart --version
```

### IDE / Code Editor

**Pilihan 1: Android Studio** (Recommended)
- Android Studio Dolphin (2021.3.1) atau lebih baru
- Flutter dan Dart plugins

**Pilihan 2: Visual Studio Code**
- VS Code latest version
- Flutter extension
- Dart extension

**Pilihan 3: IntelliJ IDEA**
- IntelliJ IDEA 2021.3 atau lebih baru
- Flutter dan Dart plugins

### Git

- **Git**: 2.20 atau lebih baru

## iOS Development Requirements (macOS Only)

### Hardware

- **Mac Computer**: MacBook Pro/Air atau iMac/Mac Mini
- **RAM**: Minimal 8 GB (recommended 16 GB)
- **Storage**: Minimal 50 GB free space (SSD recommended)

### Software

- **Xcode**: 14.0 atau lebih baru
- **CocoaPods**: 1.11+ (dependency manager)
- **Command Line Tools**: Install via Xcode

Install CocoaPods:
```bash
sudo gem install cocoapods
```

Setup Xcode:
```bash
# Install Xcode dari App Store
# Setup command line tools
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
sudo xcodebuild -runFirstLaunch
```

### iOS Simulator

- Included dengan Xcode
- Recommended: iPhone 14 atau lebih baru

### Apple Developer Account

- **Apple Developer Account**: Required untuk deploy ke App Store
- **Annual Fee**: $99/tahun

## Android Development Requirements

### Android SDK

- **Android SDK**: API level 21 (Android 5.0) hingga latest
- **Android SDK Build-Tools**: Latest version
- **Android SDK Platform-Tools**: Latest version

### Android Studio

- **Android Studio**: Dolphin (2021.3.1) atau lebih baru
- **Android SDK**: Installed via Android Studio
- **Android Emulator**: Included dengan Android Studio

### JDK

- **JDK**: OpenJDK 11 atau 17 (installed with Android Studio)

### Android Device/Emulator

**Physical Device**:
- Android 5.0 (API 21) atau lebih baru
- Minimal 2 GB RAM
- Developer Mode enabled
- USB Debugging enabled

**Android Emulator**:
- AVD dengan Android 5.0+ (API 21+)
- Recommended: Pixel 4 atau lebih baru
- Minimal 2 GB RAM allocated

### Google Play Console Account

- **Google Play Console**: Required untuk deploy ke Play Store
- **One-time Fee**: $25

## Hardware Requirements

### Minimum (Development)

- **CPU**: Dual-core processor
- **RAM**: 8 GB
- **Storage**: 20 GB free space
- **Internet**: Required untuk downloading packages

### Recommended (Development)

- **CPU**: Quad-core processor atau lebih
- **RAM**: 16 GB atau lebih
- **Storage**: 50 GB+ free space (SSD highly recommended)
- **Internet**: Stable connection

### For iOS Development (macOS)

- **Mac dengan Apple Silicon (M1/M2)**: Recommended untuk performa terbaik
- **Intel-based Mac**: Still supported

## Network Requirements

### Development

- **Internet Connection**: Required untuk:
  - Flutter pub packages
  - SDK updates
  - API testing
  - Simulator/Emulator downloads

### Runtime

- **API Connection**: Akses ke Aerplus Backend API
- **Minimum Bandwidth**: 1 Mbps (recommended 5+ Mbps untuk smooth operation)

## Optional Tools

### Version Control

- **Git**: Source control
- **GitHub Desktop**: GUI untuk Git (optional)

### API Testing

- **Postman**: API testing tool
- **Insomnia**: Alternative API client

### Design Tools

- **Figma**: Untuk view design mockups
- **Adobe XD**: Alternative design tool

## Flutter Doctor Check

Setelah install Flutter, verify installation:

```bash
flutter doctor
```

Expected output (semua ✓):
```
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 3.x.x, on macOS 13.x, locale en-US)
[✓] Android toolchain - develop for Android devices (Android SDK version 33.x.x)
[✓] Xcode - develop for iOS and macOS (Xcode 14.x)
[✓] Chrome - develop for the web
[✓] Android Studio (version 2021.3)
[✓] VS Code (version 1.x.x)
[✓] Connected device (1 available)
[✓] Network resources
```

Fix issues:
```bash
# Jika ada masalah, ikuti instruksi dari flutter doctor
flutter doctor -v  # Verbose output untuk detail

# Accept Android licenses
flutter doctor --android-licenses

# Install missing components
# Flutter akan provide command untuk install missing dependencies
```

## Minimum Device Specifications (End Users)

### iOS Devices

- **iOS Version**: 12.0 atau lebih baru
- **iPhone Models**: iPhone 5s atau lebih baru
- **iPad**: iPad Air atau lebih baru, iPad mini 2 atau lebih baru
- **Storage**: Minimal 100 MB free space
- **RAM**: 1 GB atau lebih

### Android Devices

- **Android Version**: 5.0 (API level 21) atau lebih baru
- **RAM**: Minimal 1 GB (recommended 2 GB+)
- **Storage**: Minimal 100 MB free space
- **Screen**: Minimal 4 inch display
- **Processor**: ARMv7 atau ARM64

## Checklist Sebelum Development

Pastikan Anda telah:

- [ ] Install Flutter SDK dan verify dengan `flutter doctor`
- [ ] (iOS) Install Xcode dan CocoaPods
- [ ] (Android) Install Android Studio dan Android SDK
- [ ] Install IDE/Editor dengan Flutter plugin
- [ ] Install Git
- [ ] Setup emulator atau connect physical device
- [ ] Configure API endpoint access
- [ ] Accept Android licenses (`flutter doctor --android-licenses`)
- [ ] Test dengan `flutter run` di sample project

## Verifikasi Installation

```bash
# Check Flutter
flutter --version
flutter doctor -v

# Check Dart
dart --version

# List available devices
flutter devices

# Check connected devices
adb devices  # Android
xcrun xctrace list devices  # iOS (macOS only)

# Test run
flutter create test_app
cd test_app
flutter run
```

---

**Selanjutnya**: [Technology Stack](./technology-stack)
