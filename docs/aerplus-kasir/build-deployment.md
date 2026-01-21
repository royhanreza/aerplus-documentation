---
sidebar_position: 6
---

# Build & Deployment

Panduan lengkap untuk build dan deploy Aerplus Kasir ke App Store dan Google Play Store.

## Prerequisites

### Accounts

**iOS**:
- Apple Developer Account ($99/tahun)
- Enrolled in Apple Developer Program

**Android**:
- Google Play Console account ($25 one-time)

### Tools

- EAS CLI installed: `npm install -g eas-cli`
- Logged in to Expo: `eas login`

## EAS Build Overview

EAS (Expo Application Services) menyediakan cloud build service untuk iOS dan Android.

### Build Profiles

Di `eas.json`:

```json
{
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal"
    },
    "preview": {
      "distribution": "internal",
      "android": {
        "buildType": "apk"
      }
    },
    "production": {}
  }
}
```

## iOS Build & Deployment

### Step 1: Configure iOS Credentials

```bash
eas credentials
```

Follow prompts untuk:
- Generate atau upload signing certificate
- Generate atau upload provisioning profile
- Configure push notification keys (jika diperlukan)

### Step 2: Update Version

Edit `app.json`:

```json
{
  "expo": {
    "version": "1.5.3",
    "ios": {
      "buildNumber": "1.5.3"
    }
  }
}
```

**Version Strategy**:
- `version`: User-facing version (semantic versioning)
- `buildNumber`: Internal build number (increment setiap build)

### Step 3: Build for iOS

**Preview Build (Internal Testing)**:

```bash
eas build --platform ios --profile preview
```

**Production Build (App Store)**:

```bash
eas build --platform ios --profile production
```

Build akan:
1. Upload code ke EAS servers
2. Install dependencies
3. Run native build di macOS machine
4. Generate IPA file
5. Provide download link

### Step 4: Download & Test Build

```bash
# Download IPA
eas build:download --platform ios --latest

# Install di device untuk testing
# Use Apple Configurator atau TestFlight
```

### Step 5: Submit to App Store

**Option 1: Via EAS Submit**

```bash
eas submit --platform ios
```

Follow prompts untuk:
- Select build
- Enter Apple ID credentials
- Configure submission settings

**Option 2: Manual Upload via Xcode**

1. Download IPA dari EAS
2. Open Xcode
3. Window → Organizer
4. Upload IPA
5. Submit untuk review di App Store Connect

### Step 6: App Store Connect Configuration

1. Login ke [App Store Connect](https://appstoreconnect.apple.com)
2. Select app
3. Fill metadata:
   - App name
   - Description
   - Keywords
   - Screenshots (required sizes)
   - App icon
   - Age rating
   - Contact information
4. Submit untuk review

**Screenshot Requirements**:
- iPhone 6.7" display (required)
- iPhone 6.5" display (required)
- iPhone 5.5" display
- iPad Pro (3rd gen) 12.9" (jika support iPad)

### Step 7: TestFlight (Beta Testing)

TestFlight automatically available setelah upload:

1. Add internal testers (up to 100)
2. Add external testers (up to 10,000)
3. Share TestFlight link
4. Testers install TestFlight app
5. Install beta build

## Android Build & Deployment

### Step 1: Configure Android Credentials

```bash
eas credentials
```

EAS akan generate:
- Android Keystore
- Keystore password
- Key alias dan password

**Important**: Save keystore credentials dengan aman!

### Step 2: Update Version

Edit `app.json`:

```json
{
  "expo": {
    "version": "1.5.3",
    "android": {
      "versionCode": 24
    }
  }
}
```

**Version Strategy**:
- `version`: User-facing version (1.5.3)
- `versionCode`: Integer, increment setiap build (must be higher than previous)

### Step 3: Build for Android

**Preview Build (APK for Testing)**:

```bash
eas build --platform android --profile preview
```

APK dapat langsung diinstall di device tanpa Google Play.

**Production Build (AAB for Play Store)**:

```bash
eas build --platform android --profile production
```

AAB (Android App Bundle) adalah format yang required oleh Google Play.

### Step 4: Download & Test Build

```bash
# Download APK atau AAB
eas build:download --platform android --latest

# Install APK di device
adb install app.apk
```

### Step 5: Submit to Google Play Store

**Option 1: Via EAS Submit**

```bash
eas submit --platform android
```

**Option 2: Manual Upload**

1. Login ke [Google Play Console](https://play.google.com/console)
2. Select app
3. Production → Create new release
4. Upload AAB
5. Add release notes
6. Review dan roll out

### Step 6: Google Play Console Configuration

#### Store Listing

- App name
- Short description (80 chars)
- Full description (4000 chars)
- Screenshots (required):
  - Phone: 2-8 screenshots
  - 7" tablet: 1-8 screenshots (optional)
  - 10" tablet: 1-8 screenshots (optional)
- Feature graphic: 1024x500 PNG
- App icon: 512x512 PNG

#### Content Rating

Complete questionnaire untuk get content rating.

#### App Content

- Privacy policy URL (required)
- App access (free/paid)
- Ads declaration
- Target audience

#### Pricing & Distribution

- Countries
- Free atau paid
- Contains ads (yes/no)

### Step 7: Internal Testing

Before production release:

1. Create internal testing track
2. Add internal testers (email addresses)
3. Upload build
4. Share testing link
5. Testers join via link dan install

### Step 8: Production Release

Options:
- **Production**: Release ke semua users
- **Staged Rollout**: Release gradually (20%, 50%, 100%)

## Build Commands Reference

```bash
# Login ke EAS
eas login

# Initialize EAS project
eas init

# Configure credentials
eas credentials

# Build iOS preview
eas build --platform ios --profile preview

# Build iOS production
eas build --platform ios --profile production

# Build Android APK
eas build --platform android --profile preview

# Build Android AAB
eas build --platform android --profile production

# Build both platforms
eas build --platform all

# Download latest build
eas build:download --platform ios --latest
eas build:download --platform android --latest

# Submit to stores
eas submit --platform ios
eas submit --platform android

# View build list
eas build:list

# View build details
eas build:view [BUILD_ID]

# Cancel build
eas build:cancel [BUILD_ID]
```

## Over-The-Air (OTA) Updates

### Publish Update

```bash
# Publish update ke production
eas update --branch production --message "Bug fixes"

# Publish update ke staging
eas update --branch staging --message "New features"
```

### Configure Auto Updates

Di `app.json`:

```json
{
  "updates": {
    "enabled": true,
    "checkAutomatically": "ON_LOAD",
    "fallbackToCacheTimeout": 0
  }
}
```

**Update Strategies**:
- `ON_LOAD`: Check on app launch (recommended)
- `ON_ERROR_RECOVERY`: Check setelah crash
- `NEVER`: Manual updates only

### Update Best Practices

- Test updates thoroughly before publishing
- Use staging branch untuk testing
- Provide clear update messages
- Monitor update adoption rate
- Keep update size small

**Important**: OTA updates hanya untuk JavaScript code. Native code changes require new build.

## Versioning Strategy

### Semantic Versioning

Format: `MAJOR.MINOR.PATCH`

- **MAJOR**: Breaking changes, major features
- **MINOR**: New features, backward compatible
- **PATCH**: Bug fixes

Example: `1.5.3`
- `1`: Major version
- `5`: Minor version (5th feature update)
- `3`: Patch version (3rd bug fix)

### Build Numbers

**iOS buildNumber**:
- Can be same as version: `1.5.3`
- Or use incremental: `153`

**Android versionCode**:
- Must be integer
- Must increment setiap release
- Example: `24` → `25` → `26`

## Pre-Release Checklist

Sebelum submit ke stores:

### Code & Testing
- [ ] All features tested on real devices
- [ ] Tested di iOS dan Android
- [ ] No console.log di production code
- [ ] Error handling implemented
- [ ] Offline functionality tested
- [ ] Performance tested
- [ ] Memory leaks checked

### Configuration
- [ ] Production API URL configured
- [ ] App version updated
- [ ] Build number / versionCode incremented
- [ ] Environment variables checked
- [ ] Debug mode disabled

### Assets
- [ ] App icon updated (iOS: 1024x1024, Android: 512x512)
- [ ] Splash screen configured
- [ ] Screenshots prepared (all required sizes)
- [ ] Feature graphic created (Android)

### Legal & Privacy
- [ ] Privacy policy URL ready
- [ ] Terms of service ready
- [ ] Content rating completed
- [ ] Permissions justified

### Store Listing
- [ ] App name finalized
- [ ] Description written (short & full)
- [ ] Keywords optimized
- [ ] Release notes prepared
- [ ] Contact information updated

## Troubleshooting

### iOS Build Errors

**Provisioning Profile Issues**:
```bash
# Reset credentials
eas credentials
# Select "Remove all credentials"
# Then reconfigure
```

**Certificate Expired**:
```bash
eas credentials
# Select "Renew certificate"
```

### Android Build Errors

**Keystore Issues**:
- Ensure keystore is properly configured
- Check passwords are correct
- Don't lose keystore (cannot update app without it!)

**Gradle Build Failed**:
- Check `android` configuration di `app.json`
- Verify SDK versions are supported

### Submission Errors

**iOS Rejection Reasons**:
- Missing privacy policy
- Crashes on launch
- Missing required screenshots
- App doesn't match description
- Missing permissions descriptions

**Android Rejection Reasons**:
- Target SDK version too old
- Missing required metadata
- Privacy policy issues
- Content rating incomplete

## Post-Release

### Monitoring

- Monitor crash reports
- Check user reviews
- Track download stats
- Monitor update adoption

### Update Release

When ready untuk update:

1. Implement changes
2. Test thoroughly
3. Increment version
4. Build baru
5. Submit ke stores
6. Or publish OTA update (for JS changes only)

---

**Congratulations!** Aerplus Kasir sekarang sudah live di App Store dan Google Play Store.
