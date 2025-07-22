# Deployment Guide

This guide covers the complete deployment process for the Food Delivery App across different platforms and environments.

## Table of Contents

- [Pre-deployment Checklist](#pre-deployment-checklist)
- [Android Deployment](#android-deployment)
- [iOS Deployment](#ios-deployment)
- [Web Deployment](#web-deployment)
- [Environment Configuration](#environment-configuration)
- [CI/CD Pipeline](#cicd-pipeline)
- [Release Management](#release-management)
- [Monitoring and Analytics](#monitoring-and-analytics)
- [Troubleshooting](#troubleshooting)

## Pre-deployment Checklist

### Code Quality Verification

```bash
# Run all quality checks before deployment
flutter analyze
flutter test
flutter format --set-exit-if-changed .

# Check for dependency issues
flutter pub deps
flutter doctor -v
```

### Performance Testing

```bash
# Profile the app for performance issues
flutter run --profile
flutter run --trace-startup

# Check app size
flutter build apk --analyze-size
flutter build ios --analyze-size
```

### Security Audit

- [ ] Review all dependencies for security vulnerabilities
- [ ] Ensure no sensitive data is hardcoded
- [ ] Verify proper asset configurations
- [ ] Check for debug code in release builds

### Asset Verification

- [ ] All images are optimized for mobile
- [ ] Asset paths are correctly configured in `pubspec.yaml`
- [ ] No unused assets included in build
- [ ] Image sizes are appropriate for target devices

## Android Deployment

### 1. Environment Setup

```bash
# Verify Android setup
flutter doctor
sdkmanager --licenses

# Install required tools
flutter config --android-sdk /path/to/android/sdk
flutter config --android-studio-dir /path/to/android/studio
```

### 2. App Signing Configuration

#### Generate Keystore

```bash
# Create a new keystore (one-time setup)
keytool -genkey -v -keystore android/app/upload-keystore.jks \
  -keyalg RSA -keysize 2048 -validity 10000 \
  -alias upload
```

#### Configure Signing in `android/key.properties`

```properties
storePassword=myStorePassword
keyPassword=myKeyPassword
keyAlias=upload
storeFile=upload-keystore.jks
```

#### Update `android/app/build.gradle`

```gradle
def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}

android {
    ...
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

### 3. Build Configuration

#### Update `android/app/build.gradle`

```gradle
android {
    compileSdkVersion 33
    buildToolsVersion "33.0.0"

    defaultConfig {
        applicationId "com.example.food"
        minSdkVersion 21
        targetSdkVersion 33
        versionCode flutterVersionCode.toInteger()
        versionName flutterVersionName
    }

    buildTypes {
        release {
            minifyEnabled true
            shrinkResources true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}
```

### 4. Building Release APK/AAB

```bash
# Build release APK
flutter build apk --release

# Build release AAB (recommended for Play Store)
flutter build appbundle --release

# Verify the build
cd build/app/outputs/apk/release/
ls -la app-release.apk
```

### 5. Google Play Store Deployment

#### Play Console Setup

1. **Create Play Console Account**
   - Sign up at [Google Play Console](https://play.google.com/console)
   - Pay one-time $25 registration fee

2. **Create New App**
   - Choose app name: "Food Delivery App"
   - Select default language and app type
   - Declare if app is game or app

3. **App Content Policies**
   - Fill out content rating questionnaire
   - Add privacy policy URL
   - Set target audience age groups

#### Upload and Release

```bash
# Upload using Play Console Web Interface
# 1. Go to Play Console → Your App → Release → Production
# 2. Upload app-release.aab
# 3. Fill release notes
# 4. Submit for review

# Alternative: Use fastlane for automated deployment
# gem install fastlane
# fastlane supply
```

#### Release Notes Template

```
Version 1.0.0 - Initial Release

New Features:
• Beautiful food delivery app interface
• Browse favorite and popular dishes
• View food ratings and prices
• Smooth scrolling experience
• Responsive design for all devices

Improvements:
• Optimized performance for smooth operation
• High-quality food imagery
• Intuitive user interface

Bug Fixes:
• Initial release - no bug fixes
```

## iOS Deployment

### 1. Environment Setup (macOS Only)

```bash
# Install Xcode from App Store
# Install Xcode command line tools
sudo xcode-select --install

# Install CocoaPods
sudo gem install cocoapods

# Verify iOS setup
flutter doctor
```

### 2. iOS Project Configuration

#### Update `ios/Runner/Info.plist`

```xml
<key>CFBundleName</key>
<string>Food Delivery</string>
<key>CFBundleDisplayName</key>
<string>Food Delivery</string>
<key>CFBundleIdentifier</key>
<string>com.example.food</string>
<key>CFBundleVersion</key>
<string>1</string>
<key>CFBundleShortVersionString</key>
<string>1.0.0</string>
```

### 3. Apple Developer Account Setup

#### Prerequisites

1. **Apple Developer Account**
   - Enroll at [developer.apple.com](https://developer.apple.com)
   - Annual fee: $99 USD

2. **Certificates and Provisioning**
   ```bash
   # Open Xcode project
   open ios/Runner.xcworkspace
   
   # In Xcode:
   # 1. Select Runner project
   # 2. Go to Signing & Capabilities
   # 3. Enable "Automatically manage signing"
   # 4. Select your development team
   ```

### 4. Building for iOS

```bash
# Build iOS release
flutter build ios --release

# Archive and upload via Xcode
open ios/Runner.xcworkspace

# In Xcode:
# 1. Product → Archive
# 2. Distribute App → App Store Connect
# 3. Upload to App Store Connect
```

### 5. App Store Connect Deployment

#### App Store Connect Setup

1. **Create App Record**
   - Go to [App Store Connect](https://appstoreconnect.apple.com)
   - Create new app with bundle ID from Xcode

2. **App Information**
   - App name: "Food Delivery App"
   - Primary language: English
   - Category: Food & Drink
   - Content rights declaration

3. **App Store Review Information**
   - Contact information
   - Demo account credentials (if needed)
   - Review notes

#### Screenshots and Metadata

```bash
# Required screenshot sizes:
# iPhone 6.7": 1290×2796 pixels
# iPhone 6.5": 1242×2688 pixels  
# iPhone 5.5": 1242×2208 pixels
# iPad Pro 12.9": 2048×2732 pixels
# iPad Pro 11": 1668×2388 pixels

# App preview videos (optional):
# Up to 3 videos per device size
# Duration: 15-30 seconds
# Format: .mov, .mp4, or .m4v
```

#### Submit for Review

1. **Build Upload**
   - Upload build through Xcode or Application Loader
   - Wait for processing (can take hours)

2. **Release Information**
   - Version number: 1.0.0
   - What's new in this version
   - App description and keywords

3. **Submit**
   - Submit for review
   - Review process: 24-48 hours typically
   - Watch for status updates

## Web Deployment

### 1. Build for Web

```bash
# Enable web support
flutter config --enable-web

# Create web build
flutter build web --release

# Output location: build/web/
```

### 2. Firebase Hosting Deployment

#### Setup Firebase

```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login to Firebase
firebase login

# Initialize project
firebase init hosting

# Configure firebase.json
```

#### Firebase Configuration

```json
{
  "hosting": {
    "public": "build/web",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

#### Deploy

```bash
# Build and deploy
flutter build web --release
firebase deploy

# Custom domain (optional)
firebase hosting:channel:deploy live --expires 30d
```

### 3. Alternative Web Hosting

#### GitHub Pages

```yaml
# .github/workflows/web-deploy.yml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    
    - uses: subosito/flutter-action@v1
      with:
        flutter-version: '3.0.0'
    
    - run: flutter config --enable-web
    - run: flutter pub get
    - run: flutter build web --release
    
    - name: Deploy to GitHub Pages
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./build/web
```

#### Netlify

```bash
# Install Netlify CLI
npm install -g netlify-cli

# Build and deploy
flutter build web --release
netlify deploy --prod --dir=build/web
```

## Environment Configuration

### 1. Environment Variables

#### Create `lib/config/environment.dart`

```dart
class Environment {
  static const String apiBaseUrl = String.fromEnvironment(
    'API_BASE_URL',
    defaultValue: 'https://api.fooddelivery.com',
  );
  
  static const bool isProduction = bool.fromEnvironment(
    'PRODUCTION',
    defaultValue: false,
  );
  
  static const String appVersion = String.fromEnvironment(
    'APP_VERSION',
    defaultValue: '1.0.0',
  );
}
```

#### Build with Environment Variables

```bash
# Development build
flutter build apk --dart-define=API_BASE_URL=https://dev-api.fooddelivery.com

# Production build
flutter build apk --dart-define=API_BASE_URL=https://api.fooddelivery.com --dart-define=PRODUCTION=true
```

### 2. Configuration Files

#### Create `config/app_config.json`

```json
{
  "development": {
    "apiUrl": "https://dev-api.fooddelivery.com",
    "enableLogging": true,
    "analyticsEnabled": false
  },
  "staging": {
    "apiUrl": "https://staging-api.fooddelivery.com",
    "enableLogging": true,
    "analyticsEnabled": true
  },
  "production": {
    "apiUrl": "https://api.fooddelivery.com",
    "enableLogging": false,
    "analyticsEnabled": true
  }
}
```

## CI/CD Pipeline

### 1. GitHub Actions Workflow

#### `.github/workflows/ci-cd.yml`

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Flutter
      uses: subosito/flutter-action@v2
      with:
        flutter-version: '3.7.0'
        channel: 'stable'
    
    - name: Install dependencies
      run: flutter pub get
    
    - name: Run tests
      run: flutter test
    
    - name: Analyze code
      run: flutter analyze
    
    - name: Check formatting
      run: flutter format --set-exit-if-changed .

  build-android:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Flutter
      uses: subosito/flutter-action@v2
      with:
        flutter-version: '3.7.0'
    
    - name: Setup Android keystore
      run: |
        echo "${{ secrets.ANDROID_KEYSTORE }}" | base64 --decode > android/app/key.jks
        echo "storeFile=key.jks" >> android/key.properties
        echo "keyAlias=${{ secrets.ANDROID_KEY_ALIAS }}" >> android/key.properties
        echo "storePassword=${{ secrets.ANDROID_STORE_PASSWORD }}" >> android/key.properties
        echo "keyPassword=${{ secrets.ANDROID_KEY_PASSWORD }}" >> android/key.properties
    
    - name: Build APK
      run: flutter build apk --release
    
    - name: Upload APK
      uses: actions/upload-artifact@v3
      with:
        name: app-release.apk
        path: build/app/outputs/apk/release/app-release.apk

  build-ios:
    needs: test
    runs-on: macos-latest
    if: github.ref == 'refs/heads/main'
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Flutter
      uses: subosito/flutter-action@v2
      with:
        flutter-version: '3.7.0'
    
    - name: Install CocoaPods
      run: sudo gem install cocoapods
    
    - name: Build iOS
      run: flutter build ios --release --no-codesign
```

### 2. Fastlane Integration

#### `android/fastlane/Fastfile`

```ruby
default_platform(:android)

platform :android do
  desc "Deploy to Google Play Store"
  lane :deploy do
    gradle(task: "clean")
    gradle(
      task: "bundle",
      build_type: "Release"
    )
    upload_to_play_store(
      track: "production",
      aab: "../build/app/outputs/bundle/release/app-release.aab"
    )
  end
end
```

#### `ios/fastlane/Fastfile`

```ruby
default_platform(:ios)

platform :ios do
  desc "Deploy to App Store"
  lane :deploy do
    build_app(
      workspace: "Runner.xcworkspace",
      scheme: "Runner",
      export_method: "app-store"
    )
    upload_to_app_store(
      force: true,
      submit_for_review: true
    )
  end
end
```

## Release Management

### 1. Versioning Strategy

#### Semantic Versioning

```yaml
# pubspec.yaml
version: 1.0.0+1
# Format: MAJOR.MINOR.PATCH+BUILD_NUMBER

# Version bump examples:
# 1.0.0+1 → 1.0.1+2 (patch)
# 1.0.1+2 → 1.1.0+3 (minor)
# 1.1.0+3 → 2.0.0+4 (major)
```

#### Automated Version Bumping

```bash
# Install version bumping tool
dart pub global activate cider

# Bump version
cider bump patch  # 1.0.0 → 1.0.1
cider bump minor  # 1.0.1 → 1.1.0
cider bump major  # 1.1.0 → 2.0.0
```

### 2. Release Checklist

#### Pre-release

- [ ] Update version number in `pubspec.yaml`
- [ ] Update `CHANGELOG.md`
- [ ] Run full test suite
- [ ] Perform manual testing on devices
- [ ] Update app store listings
- [ ] Prepare release notes

#### Release

- [ ] Create git tag for version
- [ ] Build release artifacts
- [ ] Upload to app stores
- [ ] Monitor crash reports
- [ ] Update documentation

#### Post-release

- [ ] Monitor user feedback
- [ ] Track performance metrics
- [ ] Plan next release cycle
- [ ] Address critical issues

### 3. Rollback Strategy

```bash
# Git rollback
git tag -d v1.0.1
git push origin :refs/tags/v1.0.1
git revert <commit-hash>

# App store rollback
# Use Play Console or App Store Connect to rollback to previous version
```

## Monitoring and Analytics

### 1. Crash Reporting

#### Firebase Crashlytics

```yaml
# pubspec.yaml
dependencies:
  firebase_crashlytics: ^3.0.0
  firebase_core: ^2.0.0
```

```dart
// lib/main.dart
import 'package:firebase_crashlytics/firebase_crashlytics.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  
  FlutterError.onError = FirebaseCrashlytics.instance.recordFlutterError;
  
  runApp(MyApp());
}
```

### 2. Performance Monitoring

```dart
// Add performance monitoring
import 'package:firebase_performance/firebase_performance.dart';

class PerformanceService {
  static Future<void> trackScreenView(String screenName) async {
    final trace = FirebasePerformance.instance.newTrace('screen_$screenName');
    await trace.start();
    // ... screen load logic
    await trace.stop();
  }
}
```

### 3. User Analytics

```dart
// Analytics implementation
import 'package:firebase_analytics/firebase_analytics.dart';

class AnalyticsService {
  static final FirebaseAnalytics _analytics = FirebaseAnalytics.instance;
  
  static Future<void> logFoodView(String foodName) async {
    await _analytics.logEvent(
      name: 'food_view',
      parameters: {'food_name': foodName},
    );
  }
}
```

## Troubleshooting

### Common Build Issues

#### Android Build Failures

```bash
# Fix Gradle issues
cd android
./gradlew clean
cd ..
flutter clean
flutter pub get

# Fix SDK issues
flutter doctor --android-licenses
```

#### iOS Build Failures

```bash
# Fix CocoaPods issues
cd ios
pod clean
pod install
cd ..
flutter clean
flutter pub get

# Fix signing issues
open ios/Runner.xcworkspace
# Check signing settings in Xcode
```

#### Web Build Issues

```bash
# Fix web build issues
flutter config --enable-web
flutter clean
flutter pub get
flutter build web --release --verbose
```

### Deployment Issues

#### App Store Rejection

Common rejection reasons:
- Missing privacy policy
- Incomplete app information
- Guideline violations
- Technical issues

#### Play Store Issues

- Check app bundle format
- Verify signing configuration
- Review content policies
- Test on various devices

### Performance Issues

```bash
# Profile app performance
flutter run --profile
flutter run --trace-startup

# Analyze bundle size
flutter build apk --analyze-size
flutter build ios --analyze-size
```

### Monitoring and Alerting

#### Set Up Alerts

```yaml
# Example alerting configuration
alerts:
  - name: "High Crash Rate"
    condition: "crash_rate > 1%"
    notification: "email, slack"
  
  - name: "App Store Rating Drop"
    condition: "rating < 4.0"
    notification: "email"
```

---

For additional deployment support, consult the [Flutter deployment documentation](https://flutter.dev/docs/deployment) and platform-specific guidelines.