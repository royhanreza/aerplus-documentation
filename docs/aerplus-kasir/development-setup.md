---
sidebar_position: 4
---

# Development Setup

Panduan lengkap untuk setup development environment Aerplus Kasir.

## Prerequisites

Pastikan Anda telah memenuhi semua [System Requirements](./system-requirements) sebelum melanjutkan.

## Step 1: Clone Repository

Clone repository Aerplus Kasir ke local machine:

```bash
git clone <repository-url> aerplus-kasir
cd aerplus-kasir
```

## Step 2: Install Dependencies

Install semua dependencies menggunakan NPM atau Yarn:

```bash
npm install
# atau
yarn install
```

Proses ini akan menginstall:
- React Native dependencies
- Expo packages
- Third-party libraries
- Native modules

## Step 3: Environment Configuration

### Create Configuration File

Buat file konfigurasi API endpoint di `resources/utils.js`:

```javascript
export const BASE_API_URL = "https://your-api-endpoint.com/api";
```

Atau jika ada file `.env`:

```env
API_URL=https://your-api-endpoint.com/api
API_TIMEOUT=30000
```

### Configure Firebase (Optional)

Jika menggunakan Firebase untuk push notifications:

1. Download `google-services.json` dari Firebase Console
2. Letakkan di root project directory
3. Pastikan file sudah ditambahkan di `app.json`:

```json
{
  "android": {
    "googleServicesFile": "./google-services.json"
  }
}
```

## Step 4: iOS Setup (macOS Only)

### Install CocoaPods Dependencies

```bash
cd ios
pod install
cd ..
```

**Catatan**: Jika menggunakan Expo managed workflow, step ini biasanya tidak diperlukan.

### Configure iOS Signing

1. Open project di Xcode
2. Select target "aerplus-pos-2"
3. Go to "Signing & Capabilities"
4. Select your Team
5. Ensure Bundle Identifier is correct

## Step 5: Android Setup

### Configure Android SDK

Pastikan Android SDK sudah terinstall dan environment variable sudah diset:

```bash
# Cek ANDROID_HOME
echo $ANDROID_HOME

# Jika belum diset, tambahkan ke .bashrc atau .zshrc
export ANDROID_HOME=$HOME/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

### Accept Android Licenses

```bash
$ANDROID_HOME/tools/bin/sdkmanager --licenses
```

## Step 6: Start Development Server

### Start Expo Dev Server

```bash
npm start
# atau
expo start
```

Ini akan membuka Expo Dev Tools di browser.

### Development Options

**Option 1: Expo Go (Recommended for Testing)**

1. Install Expo Go app di device (iOS/Android)
2. Scan QR code dari terminal atau Expo Dev Tools
3. App akan load di Expo Go

**Option 2: iOS Simulator** (macOS only)

```bash
npm run ios
# atau
expo start --ios
```

**Option 3: Android Emulator**

```bash
npm run android
# atau
expo start --android
```

## Step 7: Configure Development Tools

### VS Code Extensions

Install recommended extensions:

```json
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "msjsdiag.vscode-react-native",
    "styled-components.vscode-styled-components"
  ]
}
```

### VS Code Settings

Tambahkan ke `.vscode/settings.json`:

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
```

## Step 8: Testing Setup

### Run on Physical Device

**iOS**:
1. Connect iPhone via USB
2. Trust computer di iPhone
3. Run: `expo start --ios`
4. Select your device dari device list

**Android**:
1. Enable Developer Mode di Android device
2. Enable USB Debugging
3. Connect via USB
4. Run: `expo start --android`
5. Accept USB debugging prompt

### Testing Over WiFi

**iOS** (via Expo Go):
- Ensure device dan computer di network yang sama
- Scan QR code dari Expo Dev Tools

**Android** (via Expo Go):
- Ensure device dan computer di network yang sama
- Scan QR code atau enter URL manually

## Development Workflow

### Hot Reloading

Expo supports hot reloading:
- Edit code dan save
- App akan automatically reload
- State akan preserved (dalam kebanyakan kasus)

### Clear Cache

Jika ada issue dengan caching:

```bash
# Clear Metro bundler cache
npm start -- --clear

# Clear Expo cache
expo start -c
```

### Debug Mode

Enable debug mode:
1. Shake device atau press `Cmd+D` (iOS) / `Cmd+M` (Android)
2. Select "Debug Remote JS"
3. Debug di Chrome DevTools

## Common Development Commands

```bash
# Start development server
npm start
expo start

# Start with cache cleared
npm start -- --clear
expo start -c

# Run on iOS simulator
npm run ios
expo start --ios

# Run on Android emulator
npm run android
expo start --android

# Check for issues
expo doctor

# Update Expo SDK
expo upgrade
```

## Troubleshooting

### Port Already in Use

Jika port 19000 sudah digunakan:

```bash
# Kill process di port 19000
kill -9 $(lsof -ti:19000)

# Atau start di port berbeda
expo start --port 19001
```

### Metro Bundler Issues

```bash
# Clear watchman cache
watchman watch-del-all

# Clear Metro cache
rm -rf $TMPDIR/metro-*

# Clear npm cache
npm cache clean --force
```

### iOS Pod Install Errors

```bash
cd ios
pod deintegrate
pod cache clean --all
pod install
cd ..
```

### Android Gradle Issues

```bash
cd android
./gradlew clean
cd ..
```

### Module Not Found Errors

```bash
# Remove node_modules dan reinstall
rm -rf node_modules
npm install

# Clear cache
npm start -- --reset-cache
```

## Development Best Practices

### Code Organization

- **Components**: Reusable UI components di folder `components/`
- **Screens**: Screen components di folder `screens/`
- **API**: API calls di folder `api/`
- **Store**: State management di folder `store/`
- **Resources**: Constants dan utilities di folder `resources/`

### State Management

- Use Zustand stores untuk global state
- Use React Query untuk server state
- Use local state (useState) untuk UI state

### API Calls

- Centralize API calls di folder `api/`
- Use axios interceptors untuk authentication
- Handle errors consistently

### Performance

- Use FlashList untuk long lists
- Optimize images (compress, lazy load)
- Avoid unnecessary re-renders
- Use React.memo untuk expensive components

### Testing on Real Devices

- Test di berbagai device sizes
- Test di iOS dan Android
- Test dengan connection yang lambat
- Test offline functionality

## Next Steps

Setelah development setup selesai:

1. **[Configuration](./configuration)** - Configure app settings
2. Test basic functionality
3. Implement new features
4. Prepare untuk build production

---

**Selanjutnya**: [Configuration](./configuration)
