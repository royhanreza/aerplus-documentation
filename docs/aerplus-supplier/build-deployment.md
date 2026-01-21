---
sidebar_position: 6
---

# Build & Deployment

Panduan lengkap untuk build dan deploy Aerplus Supplier ke Google Play Store dan Apple App Store.

## Prerequisites

### Accounts & Tools

**Android**:
- Google Play Console account ($25 one-time fee)
- Android Keystore untuk app signing

**iOS**:
- Apple Developer Account ($99/year)
- Mac computer dengan Xcode
- Apple Developer certificates

**Both**:
- Flutter SDK installed dan configured
- Project fully tested

## Android Build & Deployment

### Step 1: Create Keystore

Generate keystore untuk app signing:

```bash
keytool -genkey -v -keystore ~/aerplus-supplier.jks \
  -keyalg RSA -keysize 2048 -validity 10000 \
  -alias aerplus-supplier
```

Enter information:
- Keystore password (save securely!)
- Key password (save securely!)
- Your name
- Organization
- City, State, Country

**Important**: 
- Save keystore file dengan aman!
- Backup keystore dan passwords!
- Kehilangan keystore = cannot update app!

### Step 2: Configure App Signing

Create `android/key.properties`:

```properties
storePassword=your_store_password
keyPassword=your_key_password
keyAlias=aerplus-supplier
storeFile=/Users/your_name/aerplus-supplier.jks
```

Add to `.gitignore`:

```gitignore
# Keystore
**/android/key.properties
*.jks
*.keystore
```

Update `android/app/build.gradle`:

```gradle
def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}

android {
    // ... existing config
    
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
            minifyEnabled true
            shrinkResources true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}
```

### Step 3: Update Version

Edit `pubspec.yaml`:

```yaml
version: 1.0.0+1
```

Format: `version+buildNumber`
- `version`: User-facing version (1.0.0)
- `buildNumber`: Internal build number (1, 2, 3, ...)
- Increment buildNumber untuk setiap release

### Step 4: Build APK (Testing)

For testing dan internal distribution:

```bash
# Build APK
flutter build apk --release

# Build APK with split per ABI (smaller file sizes)
flutter build apk --split-per-abi
```

Output: `build/app/outputs/flutter-apk/app-release.apk`

Install APK:
```bash
adb install build/app/outputs/flutter-apk/app-release.apk
```

### Step 5: Build App Bundle (Production)

For Google Play Store (recommended):

```bash
flutter build appbundle --release
```

Output: `build/app/outputs/bundle/release/app-release.aab`

**Why App Bundle?**
- Smaller download size (Google generates APKs per device)
- Required untuk new apps di Play Store
- Better optimization

### Step 6: Test Release Build

Before uploading:

```bash
# Install release APK
flutter install --release

# Or test bundle (requires bundletool)
# Download bundletool from GitHub
java -jar bundletool.jar build-apks \
  --bundle=build/app/outputs/bundle/release/app-release.aab \
  --output=app.apks

java -jar bundletool.jar install-apks --apks=app.apks
```

Test checklist:
- [ ] App launches successfully
- [ ] Login works
- [ ] API calls work
- [ ] Image upload works
- [ ] Navigation works
- [ ] No crashes
- [ ] Performance is good

### Step 7: Upload to Google Play Console

1. **Login** ke [Google Play Console](https://play.google.com/console)

2. **Create App** (first time only):
   - Click "Create app"
   - Enter app name: "Aerplus Supplier"
   - Select default language
   - Choose app/game type
   - Select free/paid

3. **Setup Store Listing**:
   - App name
   - Short description (80 characters)
   - Full description (4000 characters)
   - App icon (512x512 PNG)
   - Feature graphic (1024x500 PNG)
   - Screenshots (minimum 2):
     - Phone: 1080x1920 atau 1080x2340
     - 7" tablet (optional)
     - 10" tablet (optional)

4. **Content Rating**:
   - Complete questionnaire
   - Get content rating

5. **Target Audience**:
   - Select target age groups
   - Complete questionnaire

6. **Privacy Policy**:
   - Provide privacy policy URL

7. **App Access**:
   - All functionality available atau restricted
   - If restricted, provide test credentials

8. **Upload Build**:
   - Production → Create new release
   - Upload AAB file
   - Add release notes
   - Choose release type:
     - Managed publishing (manual release)
     - Immediate release
   - Save and review

9. **Rollout**:
   - Review release
   - Start rollout to production
   - Options:
     - Full rollout: 100% users
     - Staged rollout: 1%, 5%, 10%, 20%, 50%, 100%

### Step 8: Internal Testing (Optional)

Before production:

1. **Create Internal Testing Track**:
   - Testing → Internal testing
   - Create new release
   - Upload AAB

2. **Add Testers**:
   - Add email addresses
   - Or create email list

3. **Share Link**:
   - Copy testing link
   - Share dengan testers

## iOS Build & Deployment

### Step 1: Configure Xcode Project

Open `ios/Runner.xcworkspace` di Xcode:

1. **Select Runner** di project navigator
2. **General Tab**:
   - Display Name: "Aerplus Supplier"
   - Bundle Identifier: `com.magentamediatama.aerplusSupplier`
   - Version: 1.0.0 (from pubspec.yaml)
   - Build: 1 (from pubspec.yaml)

3. **Signing & Capabilities**:
   - Select your Team
   - Automatic signing (recommended)
   - Or manual signing dengan certificates

### Step 2: Configure Deployment Info

1. **iOS Deployment Target**: 12.0
2. **Supported Destinations**: iPhone, iPad (if supported)
3. **Device Orientation**: Portrait, Landscape (as needed)

### Step 3: Update Version

Same as Android - edit `pubspec.yaml`:

```yaml
version: 1.0.0+1
```

### Step 4: Build iOS App

**Option 1: Build for Simulator** (testing only)

```bash
flutter build ios --simulator
```

**Option 2: Build for Device** (requires signing)

```bash
flutter build ios --release
```

**Option 3: Build IPA** (for distribution)

```bash
flutter build ipa
```

Output: `build/ios/ipa/*.ipa`

### Step 5: Archive in Xcode

1. **Open Xcode**:
   ```bash
   open ios/Runner.xcworkspace
   ```

2. **Select Scheme**:
   - Product → Scheme → Runner
   - Product → Destination → Any iOS Device

3. **Archive**:
   - Product → Archive
   - Wait for build to complete

4. **Organizer** akan terbuka automatically

### Step 6: Upload to App Store Connect

From Xcode Organizer:

1. **Distribute App**
2. **App Store Connect**
3. **Upload**
4. **Select signing**:
   - Automatically manage signing (recommended)
   - Or manually manage signing
5. **Upload**

Wait for processing (biasanya 5-15 menit).

### Step 7: Configure App Store Connect

1. **Login** ke [App Store Connect](https://appstoreconnect.apple.com)

2. **Create App** (first time only):
   - My Apps → + → New App
   - Platform: iOS
   - Name: Aerplus Supplier
   - Primary Language: English atau Indonesian
   - Bundle ID: Select dari dropdown
   - SKU: Unique identifier (e.g., aerplus-supplier-001)

3. **App Information**:
   - Category: Business atau Productivity
   - Content Rights: Appropriate selection

4. **Pricing & Availability**:
   - Price: Free atau paid
   - Availability: All countries atau specific

5. **App Privacy**:
   - Privacy Policy URL
   - Data collection questionnaire

6. **Prepare for Submission**:
   - Screenshots (required sizes):
     - 6.7" Display (iPhone 14 Pro Max): 1290x2796
     - 6.5" Display (iPhone 11 Pro Max): 1242x2688
     - 5.5" Display (iPhone 8 Plus): 1242x2208
     - iPad Pro (3rd gen) 12.9": 2048x2732
   - App Description
   - Keywords (for search)
   - Support URL
   - Marketing URL (optional)
   - Promotional text (optional)

7. **Build**:
   - Select uploaded build
   - Export Compliance: Provide info

8. **App Review Information**:
   - Contact information
   - Demo account (if required)
   - Notes for reviewer

9. **Version Release**:
   - Automatic release
   - Manual release

10. **Submit for Review**:
    - Review all info
    - Submit

### Step 8: TestFlight (Beta Testing)

Before production release:

1. **TestFlight Tab** di App Store Connect
2. **Internal Testing**:
   - Add testers (up to 100)
   - Automatic distribution
3. **External Testing** (optional):
   - Create group
   - Add testers (up to 10,000)
   - Requires beta review

Testers:
- Install TestFlight app dari App Store
- Receive invitation email
- Install beta build

## Build Commands Reference

### Android

```bash
# Debug build
flutter build apk --debug

# Release APK
flutter build apk --release

# Release APK split per ABI
flutter build apk --split-per-abi

# Release App Bundle
flutter build appbundle --release

# Build with obfuscation
flutter build appbundle --obfuscate --split-debug-info=build/app/outputs/symbols
```

### iOS

```bash
# Debug build
flutter build ios --debug

# Release build
flutter build ios --release

# Build IPA
flutter build ipa

# Build with obfuscation
flutter build ipa --obfuscate --split-debug-info=build/ios/outputs/symbols

# Clean build
flutter clean
flutter pub get
flutter build ipa
```

## Code Obfuscation (Recommended)

Obfuscate code untuk security:

```bash
# Android
flutter build appbundle --obfuscate --split-debug-info=build/app/outputs/symbols

# iOS
flutter build ipa --obfuscate --split-debug-info=build/ios/outputs/symbols
```

Save symbols untuk debugging crashes later.

## Versioning Strategy

### Semantic Versioning

Format: `MAJOR.MINOR.PATCH+BUILD`

Example: `1.2.3+15`
- `1`: Major version (breaking changes)
- `2`: Minor version (new features, backward compatible)
- `3`: Patch version (bug fixes)
- `15`: Build number (internal, always increment)

### Version Updates

**Bug Fixes**: `1.0.0+1` → `1.0.1+2`

**New Features**: `1.0.1+2` → `1.1.0+3`

**Breaking Changes**: `1.1.0+3` → `2.0.0+4`

## Release Checklist

Sebelum submit:

### Code & Testing
- [ ] All features tested
- [ ] Tested di multiple devices
- [ ] Tested di iOS dan Android
- [ ] No debug code or console.log
- [ ] Error handling implemented
- [ ] Performance tested
- [ ] Memory leaks checked
- [ ] API endpoints configured to production

### Configuration
- [ ] Version updated di pubspec.yaml
- [ ] App icon updated
- [ ] Splash screen configured
- [ ] Permissions configured dan justified
- [ ] API base URL set to production
- [ ] Debug mode disabled
- [ ] Keystore/certificates configured

### Store Listing
- [ ] App name finalized
- [ ] Description written
- [ ] Screenshots prepared (all sizes)
- [ ] App icon (512x512 untuk Android)
- [ ] Feature graphic (1024x500 untuk Android)
- [ ] Keywords optimized
- [ ] Privacy policy URL
- [ ] Support email/URL
- [ ] Content rating completed
- [ ] Test credentials (if required)

### Build
- [ ] Release build tested
- [ ] App bundle/IPA built successfully
- [ ] Signed dengan proper credentials
- [ ] File size reasonable (&lt;100MB ideal)

## Post-Release

### Monitoring

1. **Crash Reports**:
   - Google Play Console → Quality
   - App Store Connect → TestFlight/App Analytics

2. **User Reviews**:
   - Monitor dan respond to reviews
   - Track ratings

3. **Analytics**:
   - Installs
   - Active users
   - Retention

### Updates

When ready untuk update:

1. Implement changes
2. Test thoroughly
3. Increment version dan build number
4. Build new release
5. Upload to stores
6. Submit for review

**Update Frequency**: Recommended minimal 1-2 months untuk stability.

## Troubleshooting

### Android Issues

**Build Fails**:
```bash
flutter clean
flutter pub get
flutter build appbundle
```

**Signing Issues**:
- Verify key.properties path
- Check keystore passwords
- Ensure keystore file exists

**Upload Rejected**:
- Check app bundle integrity
- Verify package name matches
- Ensure version code is incremented

### iOS Issues

**Code Signing**:
- Check certificates di Xcode
- Verify provisioning profiles
- Ensure bundle ID matches

**Archive Fails**:
```bash
cd ios
pod deintegrate
pod install
cd ..
flutter clean
flutter pub get
```

**Upload Fails**:
- Check Xcode version compatibility
- Verify Apple Developer account status
- Ensure proper entitlements

## Store Approval Tips

### Common Rejection Reasons

**Android**:
- Missing privacy policy
- Incorrect content rating
- Crashes on launch
- Misleading descriptions

**iOS**:
- App doesn't match description
- Crashes or bugs
- Missing required metadata
- Privacy issues
- Design doesn't meet guidelines

### Best Practices

1. **Provide clear description** of app functionality
2. **Test extensively** before submission
3. **Provide demo credentials** jika diperlukan
4. **Follow platform guidelines**:
   - [Android](https://developer.android.com/distribute/best-practices)
   - [iOS](https://developer.apple.com/app-store/review/guidelines/)
5. **Respond promptly** to reviewer questions

---

**Congratulations!** Aerplus Supplier sekarang sudah live di Google Play Store dan Apple App Store.
