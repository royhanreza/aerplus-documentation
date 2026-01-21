---
sidebar_position: 5
---

# Configuration

Panduan konfigurasi Aerplus Kasir untuk development dan production.

## App Configuration (app.json)

File `app.json` adalah konfigurasi utama untuk Expo app.

### Basic Settings

```json
{
  "expo": {
    "name": "Aerplus Kasir 2",
    "slug": "aerplus-pos-2",
    "version": "1.5.2",
    "orientation": "portrait"
  }
}
```

**Penjelasan**:
- `name`: Nama aplikasi yang tampil di device
- `slug`: Unique identifier untuk app (lowercase, no spaces)
- `version`: Semantic version (major.minor.patch)
- `orientation`: Lock orientation (portrait/landscape/default)

### App Icons

```json
{
  "icon": "./assets/final/icon1024.png",
  "ios": {
    "icon": "./assets/final/icon1024.png"
  },
  "android": {
    "icon": "./assets/final/icon512.png",
    "adaptiveIcon": {
      "foregroundImage": "./assets/final/adaptive-icon.png",
      "backgroundColor": "#5AB9AE"
    }
  }
}
```

**Requirements**:
- iOS icon: 1024x1024 PNG
- Android icon: 512x512 PNG
- Adaptive icon: 512x512 PNG (foreground)

### Splash Screen

```json
{
  "splash": {
    "image": "./assets/final/splash.png",
    "resizeMode": "contain",
    "backgroundColor": "#5AB9AE"
  }
}
```

**Splash Screen Tips**:
- Use simple logo atau branding
- Keep background solid color
- `resizeMode`: "contain" atau "cover"

### iOS Configuration

```json
{
  "ios": {
    "supportsTablet": true,
    "bundleIdentifier": "com.magentamediatama.aerpluspos2",
    "buildNumber": "1.5.2",
    "infoPlist": {
      "NSPhotoLibraryUsageDescription": "Allow $(PRODUCT_NAME) to access your photos.",
      "NSPhotoLibraryAddUsageDescription": "Allow $(PRODUCT_NAME) to save photos.",
      "NSCameraUsageDescription": "Allow $(PRODUCT_NAME) to access camera."
    }
  }
}
```

**Penjelasan**:
- `supportsTablet`: Support iPad
- `bundleIdentifier`: Unique app ID (reverse domain)
- `buildNumber`: Build version (increment untuk setiap build)
- `infoPlist`: Permission descriptions (ditampilkan saat request permission)

### Android Configuration

```json
{
  "android": {
    "package": "com.magentamediatama.aerpluspos2",
    "versionCode": 23,
    "permissions": [
      "android.permission.CAMERA",
      "android.permission.READ_EXTERNAL_STORAGE",
      "android.permission.WRITE_EXTERNAL_STORAGE"
    ],
    "googleServicesFile": "./google-services.json"
  }
}
```

**Penjelasan**:
- `package`: Unique app ID (reverse domain)
- `versionCode`: Integer version (increment untuk setiap release)
- `permissions`: Android permissions
- `googleServicesFile`: Firebase configuration

### Build Properties

```json
{
  "plugins": [
    [
      "expo-build-properties",
      {
        "android": {
          "compileSdkVersion": 34,
          "targetSdkVersion": 34,
          "buildToolsVersion": "34.0.0"
        },
        "ios": {
          "deploymentTarget": "13.4"
        }
      }
    ]
  ]
}
```

### Notifications Configuration

```json
{
  "plugins": [
    [
      "expo-notifications",
      {
        "sounds": ["./assets/sound/order_notification.wav"]
      }
    ]
  ]
}
```

Custom notification sounds:
- Format: WAV atau MP3
- Letakkan di `assets/sound/`

### Barcode Scanner

```json
{
  "plugins": [
    [
      "expo-barcode-scanner",
      {
        "cameraPermission": "Allow $(PRODUCT_NAME) to access camera."
      }
    ]
  ]
}
```

## API Configuration

### Base API URL

Edit `resources/utils.js`:

```javascript
// Development
export const BASE_API_URL = "http://localhost:8000/api";

// Staging
export const BASE_API_URL = "https://staging-api.aerplus.com/api";

// Production
export const BASE_API_URL = "https://api.aerplus.com/api";
```

### API Timeout

Configure axios timeout di API files:

```javascript
import axios from 'axios';

const instance = axios.create({
  baseURL: BASE_API_URL,
  timeout: 30000, // 30 seconds
  headers: {
    'Content-Type': 'application/json',
  }
});
```

## EAS Build Configuration (eas.json)

### Build Profiles

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
    "production": {
      "ios": {
        "resourceClass": "m1-medium"
      }
    }
  }
}
```

**Build Profiles**:

1. **development**: Development builds dengan debugging enabled
2. **preview**: Internal testing (APK untuk Android)
3. **production**: Production builds untuk app stores

## Environment Variables

### Using expo-constants

```javascript
import Constants from 'expo-constants';

const API_URL = Constants.expoConfig?.extra?.apiUrl;
```

### Configure in app.json

```json
{
  "extra": {
    "apiUrl": "https://api.aerplus.com/api",
    "environment": "production"
  }
}
```

## Storage Configuration

### Secure Store (for tokens)

```javascript
import * as SecureStore from 'expo-secure-store';

// Save token
await SecureStore.setItemAsync('authToken', token);

// Get token
const token = await SecureStore.getItemAsync('authToken');

// Delete token
await SecureStore.deleteItemAsync('authToken');
```

## Push Notifications

### Firebase Configuration

1. Download `google-services.json` dari Firebase Console
2. Letakkan di root directory
3. Add reference di `app.json`:

```json
{
  "android": {
    "googleServicesFile": "./google-services.json"
  }
}
```

### Notification Permissions

Request permissions di App.js:

```javascript
import * as Notifications from 'expo-notifications';

async function registerForPushNotificationsAsync() {
  const { status: existingStatus } = await Notifications.getPermissionsAsync();
  let finalStatus = existingStatus;
  
  if (existingStatus !== 'granted') {
    const { status } = await Notifications.requestPermissionsAsync();
    finalStatus = status;
  }
  
  if (finalStatus !== 'granted') {
    alert('Failed to get push token for push notification!');
    return;
  }
  
  const token = (await Notifications.getExpoPushTokenAsync()).data;
  return token;
}
```

## Printing Configuration

### Bluetooth Printer Setup

```javascript
import * as Print from 'expo-print';

// Print receipt
await Print.printAsync({
  html: receiptHtml,
  // Printer selection akan muncul
});
```

### Receipt Template

Edit template di `resources/receipt-designs/v1.txt`:

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    body { font-family: monospace; font-size: 12px; }
    .header { text-align: center; font-weight: bold; }
    .line { border-top: 1px dashed #000; }
  </style>
</head>
<body>
  <div class="header">
    <h2>TOKO NAME</h2>
    <p>Address Line 1</p>
  </div>
  <!-- Receipt content -->
</body>
</html>
```

## Deep Linking (Optional)

### Configure Scheme

```json
{
  "scheme": "aerpluspos",
  "ios": {
    "associatedDomains": ["applinks:aerplus.com"]
  },
  "android": {
    "intentFilters": [
      {
        "action": "VIEW",
        "data": [
          {
            "scheme": "https",
            "host": "aerplus.com",
            "pathPrefix": "/pos"
          }
        ],
        "category": ["BROWSABLE", "DEFAULT"]
      }
    ]
  }
}
```

## Analytics (Optional)

### Firebase Analytics

```javascript
import * as Analytics from 'expo-firebase-analytics';

// Log event
await Analytics.logEvent('purchase', {
  item_id: 'P12345',
  item_name: 'Product Name',
  currency: 'IDR',
  value: 100000
});
```

## App Updates (OTA)

### Configure Updates

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
- `ON_LOAD`: Check on app launch
- `ON_ERROR_RECOVERY`: Check after error
- `NEVER`: Manual updates only

## Development vs Production

### Development Configuration

```javascript
const DEV_CONFIG = {
  API_URL: 'http://localhost:8000/api',
  DEBUG: true,
  LOG_LEVEL: 'verbose'
};
```

### Production Configuration

```javascript
const PROD_CONFIG = {
  API_URL: 'https://api.aerplus.com/api',
  DEBUG: false,
  LOG_LEVEL: 'error'
};
```

### Use Based on Environment

```javascript
import Constants from 'expo-constants';

const isDev = Constants.appOwnership === 'expo' || __DEV__;
const config = isDev ? DEV_CONFIG : PROD_CONFIG;
```

## Security Configuration

### API Token Security

```javascript
import * as SecureStore from 'expo-secure-store';

// Store sensitive data
await SecureStore.setItemAsync('apiKey', apiKey, {
  keychainAccessible: SecureStore.WHEN_UNLOCKED
});
```

### SSL Pinning (Advanced)

Untuk extra security, implement SSL pinning:

```javascript
// Memerlukan custom native module
// Recommended untuk production apps yang handle sensitive data
```

## Performance Configuration

### Enable Hermes

Hermes sudah enabled di `app.json`:

```json
{
  "jsEngine": "hermes"
}
```

### Optimize Images

```javascript
import { Image } from 'expo-image';

<Image
  source={{ uri: imageUrl }}
  cachePolicy="memory-disk"
  contentFit="cover"
  transition={200}
/>
```

## Checklist Konfigurasi

Sebelum production:

- [ ] Update `version` dan `versionCode`/`buildNumber`
- [ ] Set production API URL
- [ ] Configure app icons dan splash screen
- [ ] Setup Firebase (jika menggunakan notifications)
- [ ] Test permissions (camera, storage, notifications)
- [ ] Configure deep linking (jika diperlukan)
- [ ] Enable Hermes engine
- [ ] Setup OTA updates
- [ ] Test on real devices (iOS dan Android)

---

**Selanjutnya**: [Build & Deployment](./build-deployment)
