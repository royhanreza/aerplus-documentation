---
sidebar_position: 2
---

# System Requirements

Dokumen ini menjelaskan persyaratan sistem untuk development dan deployment Aerplus Kasir.

## Development Requirements

### Operating System

**macOS** (untuk iOS development):
- macOS 12.0 (Monterey) atau lebih baru
- Xcode 14+ (untuk iOS build)

**Windows/Linux** (untuk Android development):
- Windows 10/11 atau Ubuntu 20.04+ (atau distribusi Linux lainnya)
- Android Studio Arctic Fox atau lebih baru

### Node.js & NPM

- **Node.js**: 16.x atau lebih baru (disarankan 18.x LTS)
- **NPM**: 8.x atau lebih baru
- **Yarn**: 1.22+ (opsional, alternatif NPM)

Verifikasi instalasi:
```bash
node -v
npm -v
```

### Expo CLI

- **Expo CLI**: Latest version
- **EAS CLI**: Latest version (untuk build production)

Install Expo CLI:
```bash
npm install -g expo-cli eas-cli
```

### Git

- **Git**: 2.20 atau lebih baru

## iOS Development Requirements

### Hardware

- **Mac Computer**: MacBook Pro/Air atau Mac Mini dengan processor Apple Silicon (M1/M2) atau Intel
- **RAM**: Minimal 8 GB (disarankan 16 GB)
- **Storage**: Minimal 50 GB free space (SSD recommended)

### Software

- **Xcode**: 14.0 atau lebih baru
- **iOS Simulator**: Included dengan Xcode
- **CocoaPods**: 1.11+ (dependency manager untuk iOS)

Install CocoaPods:
```bash
sudo gem install cocoapods
```

### Apple Developer Account

- **Apple Developer Account**: Diperlukan untuk deploy ke App Store
- **Annual Fee**: $99/tahun

## Android Development Requirements

### Hardware

- **RAM**: Minimal 8 GB (disarankan 16 GB)
- **Storage**: Minimal 30 GB free space (SSD recommended)
- **Processor**: Multi-core processor (Intel/AMD)

### Software

- **Android Studio**: Arctic Fox (2020.3.1) atau lebih baru
- **Android SDK**: SDK 34 (Android 14)
- **Android Build Tools**: 34.0.0
- **JDK**: JDK 11 atau lebih baru

### Android Device/Emulator

- **Physical Device**: Android 14 atau lebih baru (untuk testing)
- **Android Emulator**: AVD dengan Android 14

### Google Play Console Account

- **Google Play Console**: Diperlukan untuk deploy ke Google Play Store
- **One-time Fee**: $25

## Testing Devices

### Physical Devices (Recommended)

**iOS**:
- iPhone (iOS 13.4+)
- iPad (opsional, jika mendukung tablet)

**Android**:
- Android phone dengan OS 14+ (API level 34)
- Minimal 2 GB RAM
- Screen resolution minimal 720x1280

### Expo Go App

Untuk development testing:
- Install **Expo Go** dari App Store (iOS) atau Google Play Store (Android)
- Tidak perlu Xcode/Android Studio untuk testing via Expo Go

## Network Requirements

### Development

- **Internet Connection**: Diperlukan untuk:
  - Download dependencies (NPM packages)
  - Expo Metro Bundler
  - EAS Build service
  - Testing dengan Expo Go

### Runtime

- **API Connection**: Koneksi ke Aerplus Backend API
- **Minimum Bandwidth**: 1 Mbps (recommended 5+ Mbps)

## Optional Tools

### Code Editor

- **Visual Studio Code**: Recommended
- **VS Code Extensions**:
  - React Native Tools
  - ESLint
  - Prettier
  - React Native Snippet

### Debugging Tools

- **React Native Debugger**: Standalone debugging tool
- **Flipper**: Mobile app debugging platform
- **Reactotron**: React Native debugging tool

### Design Tools

- **Figma**: Untuk melihat design mockups
- **Zeplin**: Alternative design handoff tool

## Backend Requirements

Aerplus Kasir memerlukan akses ke:

- **Aerplus Backend API**: REST API endpoint
- **Authentication Server**: Untuk login dan authorization
- **Firebase**: Untuk push notifications (opsional)

## Minimum Device Specifications (End Users)

### iOS Devices

- **iOS Version**: 13.4 atau lebih baru
- **iPhone Models**: iPhone 6s atau lebih baru
- **Storage**: Minimal 100 MB free space
- **RAM**: 2 GB atau lebih

### Android Devices

- **Android Version**: Android 14 (API level 34) atau lebih baru
- **RAM**: Minimal 2 GB
- **Storage**: Minimal 100 MB free space
- **Screen**: Minimal 4.5 inch display

## Peripheral Requirements (Optional)

### Bluetooth Printer

- **Thermal Printer**: 58mm atau 80mm thermal printer
- **Bluetooth**: Bluetooth 4.0 atau lebih baru
- **Supported Models**:
  - Epson TM-series
  - Star Micronics TSP series
  - Generic ESC/POS compatible printers

### Barcode Scanner

- **Built-in Camera**: Untuk scan barcode via kamera device
- **External Scanner**: Bluetooth barcode scanner (opsional)

## Checklist Sebelum Development

Pastikan Anda telah:

- [ ] Install Node.js 16+ dan NPM
- [ ] Install Expo CLI dan EAS CLI
- [ ] Install Git
- [ ] (iOS) Install Xcode dan CocoaPods
- [ ] (Android) Install Android Studio dan Android SDK
- [ ] Install code editor (VS Code)
- [ ] Punya akses ke Aerplus Backend API
- [ ] Punya test device atau emulator
- [ ] Install Expo Go di test device

## Verifikasi Requirements

Jalankan command berikut untuk verifikasi instalasi:

```bash
# Node & NPM
node -v
npm -v

# Expo CLI
expo --version
eas --version

# Git
git --version

# CocoaPods (macOS only)
pod --version

# Android (check ANDROID_HOME)
echo $ANDROID_HOME
```

---

**Selanjutnya**: [Technology Stack](./technology-stack)
