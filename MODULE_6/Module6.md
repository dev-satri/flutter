# Module 6: Deployment and Distribution

## 1. Preparing Apps for Deployment

### 1.1 Creating Build Configurations (Debug, Release)
In Flutter, you can create different build configurations for your application to manage how it behaves in different environments.

- **Debug Build**:
  The debug build is used during development. It includes debugging information, and hot reload features are enabled.

- **Release Build**:
  The release build is optimized for performance and is meant for deployment. It does not include debugging information.

#### Creating a Release Build:
To create a release build for Android, run the following command in your terminal:

```
flutter build apk --release
```

For iOS, you can create a release build with:

```
flutter build ios --release
```

### 1.2 App Icon and Splash Screen Customization
Customizing your app icon and splash screen is essential for branding.

- **App Icon**:
  You can use the `flutter_launcher_icons` package to easily set your app icon. First, add it to your `pubspec.yaml`:

  ```
  dev_dependencies:
    flutter_launcher_icons: ^0.10.0
  ```

  Then, configure it:

  ```
  flutter_icons:
    android: true
    ios: true
    image_path: "assets/icon/app_icon.png"
  ```

  Run the following command to generate the app icon:

  ```
  flutter pub run flutter_launcher_icons:main
  ```

- **Splash Screen**:
  To customize the splash screen, you can use the `flutter_native_splash` package. Add it to your `pubspec.yaml`:

  ```
  dev_dependencies:
    flutter_native_splash: ^2.0.0
  ```

  Configure it as follows:

  ```
  flutter_native_splash:
    color: "#ffffff"
    image: assets/images/splash_image.png
  ```

  Run the command to generate the splash screen:

  ```
  flutter pub run flutter_native_splash:create
  ```

### 1.3 Managing App Permissions and Configurations
Managing app permissions is crucial for accessing device features.

- **Android Permissions**:
  Add the required permissions in `AndroidManifest.xml` located in `android/app/src/main/AndroidManifest.xml`:

  ```
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
  ```

- **iOS Permissions**:
  Add permissions in `Info.plist` located in `ios/Runner/Info.plist`:

  ```
  <key>NSLocationWhenInUseUsageDescription</key>
  <string>This app requires access to your location.</string>
  ```

---

## 2. Deploying Apps to the Play Store and App Store

### 2.1 Signing and Building APKs and App Bundles for Android
To deploy your Flutter app to the Play Store, you need to sign your APK or App Bundle.

- **Creating a Keystore**:
  Use the following command to create a new keystore:

  ```
  keytool -genkey -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-key-alias
  ```

  Follow the prompts to set up your keystore.

- **Configure `build.gradle`**:
  In `android/app/build.gradle`, configure signingConfigs:

  ```
  android {
      ...
      signingConfigs {
          release {
              keyAlias 'my-key-alias'
              keyPassword 'your_key_password'
              storeFile file('path/to/my-release-key.jks')
              storePassword 'your_store_password'
          }
      }
      buildTypes {
          release {
              signingConfig signingConfigs.release
          }
      }
  }
  ```

- **Build the APK**:
  To build the signed APK, run:

  ```
  flutter build apk --release
  ```

- **Build the App Bundle**:
  To build the signed App Bundle, run:

  ```
  flutter build appbundle --release
  ```

### 2.2 Preparing iOS Builds for the App Store
For iOS deployment, you must have a valid Apple Developer account and Xcode installed.

- **Update `Info.plist`**:
  Ensure you update the `Info.plist` file for necessary permissions and app information.

- **Create an Archive**:
  Open your project in Xcode, select the target, and go to `Product > Archive`. This creates a build you can upload to the App Store.

### 2.3 Handling App Store Submission Process
- **Prepare for Submission**:
  1. Sign in to App Store Connect.
  2. Create a new app listing and fill in the required metadata (app name, description, keywords, etc.).

- **Upload Your Build**:
  Use Xcode or Transporter to upload your build to App Store Connect.

- **Submit for Review**:
  After your build is processed, submit it for review. Ensure you comply with App Store guidelines to avoid rejection.

---

## 3. CI/CD for Flutter Apps

### 3.1 Setting Up CI/CD with GitHub Actions
GitHub Actions allows you to automate your workflow.

- **Creating a Workflow**:
  Create a `.github/workflows/flutter.yml` file in your project:

  ```
  name: Flutter CI

  on:
    push:
      branches: [ main ]
    pull_request:
      branches: [ main ]

  jobs:
    build:
      runs-on: ubuntu-latest

      steps:
      - name: Check out code
        uses: actions/checkout@v2
      - name: Set up JDK
        uses: actions/setup-java@v1
        with:
          java-version: '11'
      - name: Install Flutter
        run: |
          git clone https://github.com/flutter/flutter.git -b stable --depth 1
          echo "${{ github.workspace }}/flutter/bin" >> $GITHUB_PATH
      - name: Run tests
        run: |
          flutter pub get
          flutter test
  ```

### 3.2 Automated Testing and Deployment
- **Automated Testing**:
  You can add testing steps to your GitHub Actions workflow to ensure your app is working correctly.

```
- name: Run tests
  run: |
    flutter pub get
    flutter test
```

- **Deployment**:
  After successful testing, you can add steps to deploy your app to the Play Store or App Store using CI/CD tools.

---

## Assignment Questions

1. **Build Configurations**:
   - Create a Flutter app and set up both debug and release configurations. Document the steps.

2. **App Icon and Splash Screen**:
   - Customize your app's icon and splash screen. Provide screenshots of your app with the new customizations.

3. **App Permissions**:
   - Implement functionality that requires permissions (e.g., accessing the camera or location) in your app. Test and document how the app behaves with and without permissions.

4. **APK and App Bundle**:
   - Build and sign an APK and App Bundle for your Flutter app. Provide the steps and any issues faced during the process.

5. **iOS Deployment**:
   - Prepare an iOS build for the App Store. Document the submission process and any challenges encountered.

6. **CI/CD with GitHub Actions**:
   - Set up GitHub Actions for your Flutter project. Create a simple CI workflow that runs tests on each push to the main branch.

7. **Automated Testing**:
   - Write unit tests for a Flutter app feature and integrate those tests into your CI/CD pipeline.

8. **Deployment Process**:
   - Research the app submission process for either the Play Store or App Store and summarize the key steps required for a successful submission.
