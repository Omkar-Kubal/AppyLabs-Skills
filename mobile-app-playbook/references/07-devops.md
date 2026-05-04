# Section 7: Deployment & DevOps

## 7.1 CI/CD Pipeline (GitHub Actions)

### Pipeline Stages
```
Push to branch
    └── [CI: Pull Request]
            ├── Lint check
            ├── Unit tests
            └── Build debug APK

Merge to develop
    └── [CI: Develop]
            ├── Unit + Integration tests
            ├── Build staging AAB
            └── Deploy to Firebase App Distribution (internal testers)

Merge to main
    └── [CD: Production]
            ├── All tests
            ├── Build release AAB (signed)
            ├── Upload to Play Console (internal track)
            └── Notify team (Slack/email)
```

### GitHub Actions Workflow
```yaml
# .github/workflows/ci.yml
name: CI

on:
  pull_request:
    branches: [main, develop]
  push:
    branches: [main, develop]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'
      - uses: gradle/gradle-build-action@v3

      - name: Run lint
        run: ./gradlew lintDebug

      - name: Run unit tests
        run: ./gradlew testDebugUnitTest

      - name: Build debug APK
        run: ./gradlew assembleDebug

  release:
    if: github.ref == 'refs/heads/main'
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Decode keystore
        env:
          KEYSTORE_BASE64: ${{ secrets.KEYSTORE_BASE64 }}
        run: echo $KEYSTORE_BASE64 | base64 -d > release.jks

      - name: Build release AAB
        env:
          STORE_PASSWORD: ${{ secrets.STORE_PASSWORD }}
          KEY_ALIAS: ${{ secrets.KEY_ALIAS }}
          KEY_PASSWORD: ${{ secrets.KEY_PASSWORD }}
        run: ./gradlew bundleRelease

      - name: Upload to Play Store
        uses: r0adkll/upload-google-play@v1
        with:
          serviceAccountJsonPlainText: ${{ secrets.PLAY_SERVICE_ACCOUNT_JSON }}
          packageName: com.company.appname
          releaseFiles: app/build/outputs/bundle/release/*.aab
          track: internal
```

### GitHub Secrets to Configure
```
KEYSTORE_BASE64           — Base64 encoded .jks file
STORE_PASSWORD            — Keystore password
KEY_ALIAS                 — Key alias
KEY_PASSWORD              — Key password
PLAY_SERVICE_ACCOUNT_JSON — Play Console service account JSON
FIREBASE_APP_ID           — For App Distribution
FIREBASE_TOKEN            — Firebase CI token
SLACK_WEBHOOK_URL         — Build notifications
```

---

## 7.2 Build Generation

### APK vs AAB
| Format | Use Case |
|--------|---------|
| APK | Direct install, Firebase App Distribution, testing |
| AAB (Android App Bundle) | Play Store submission (required since 2021) |

### Build Variants
```kotlin
// build.gradle.kts
buildTypes {
    debug {
        applicationIdSuffix = ".debug"     // Installs alongside release
        versionNameSuffix = "-debug"
        isDebuggable = true
        isMinifyEnabled = false
    }
    release {
        isMinifyEnabled = true
        isShrinkResources = true
        proguardFiles(getDefaultProguardFile("proguard-android-optimize.txt"), "proguard-rules.pro")
        signingConfig = signingConfigs.getByName("release")
    }
}

flavorDimensions += "environment"
productFlavors {
    create("staging") {
        applicationIdSuffix = ".staging"
        buildConfigField("String", "BASE_URL", "\"https://staging-api.company.com\"")
    }
    create("production") {
        buildConfigField("String", "BASE_URL", "\"https://api.company.com\"")
    }
}
```

---

## 7.3 Environment Management

### Environment Configuration
```
debug + staging   → Development environment
  BASE_URL = staging API
  Firebase project = staging project
  Logging = verbose
  Crashlytics = disabled

release + staging → QA/UAT environment
  BASE_URL = staging API
  Firebase project = staging project
  Logging = minimal
  Crashlytics = enabled → staging dashboard

release + production → App Store release
  BASE_URL = production API
  Firebase project = production project
  Logging = errors only
  Crashlytics = enabled → production dashboard
```

### local.properties (never commit)
```properties
# Signing
store.file=../release.jks
store.password=your_store_password
key.alias=your_key_alias
key.password=your_key_password

# API Keys (keep out of codebase)
maps.api.key=AIza...
```

---

## 7.4 Versioning Strategy

### Semantic Versioning for Mobile
```
versionName: MAJOR.MINOR.PATCH
  MAJOR → Breaking redesign or complete rewrite
  MINOR → New feature released
  PATCH → Bug fix, hotfix

versionCode: Auto-incremented integer (never decrements)
  Strategy: (MAJOR × 10000) + (MINOR × 100) + PATCH
  Example: 1.3.2 → 10302

CI auto-increment:
  versionCode = github.run_number (always increases)
```

### Branch Strategy
```
main           → Production releases only
develop        → Integration branch (always deployable)
feature/*      → New features (branch from develop)
fix/*          → Bug fixes (branch from develop or main for hotfix)
release/x.x.x  → Release prep (freeze features, only bug fixes)
```
