
# 📱 Capacitor Android WebView App (APK Build & Signing Guide)

This guide shows you how to package your web app (`https://google.com/`) into an Android APK using Capacitor.  

---

## ⚡ Step 1. Install Capacitor Packages

```bash
npm install @capacitor/core @capacitor/cli @capacitor/android
```

## Optional - Install Splash Screen

```bash
npm install @capacitor/splash-screen
npx cap sync (if installed)
```

Add Splash image inside web assets

```bash
assets/splash.png
```

---

## ⚡ Step 2. Initialize Capacitor

```bash
npx cap init
```

When asked:  
- **App Name** → `google`  
- **App ID** → `com.google`  

Create an empty folder so Capacitor won’t complain:

```bash
mkdir dist
```

---

## ⚡ Step 3. Configure Remote WebView

Create `capacitor.config.json` in your project root:

```json
{
  "appId": "com.google",
  "appName": "google",
  "webDir": "dist",
  "server": {
    "url": "https://google.com/",
    "cleartext": true
  }
}
```

for Splash Screen
```json
{
  "appId": "com.google",
  "appName": "google",
  "webDir": "dist",
  "server": {
    "url": "https://google.com/",
    "cleartext": true
  },
  "plugins": {
    "SplashScreen": {
      "launchShowDuration": 2000,
      "launchAutoHide": true,
      "backgroundColor": "#ffffff",
      "androidScaleType": "CENTER_INSIDE",
      "showSpinner": false,
      "splashFullScreen": true,
      "splashImmersive": true
    }
  }
}

```

---

## Optional Generate Splash

```bash
npx @capacitor/assets generate --icon assets/icon.png --splash assets/splash.png
```

## ⚡ Step 4. Add Android Platform

```bash
npx cap add android
```

---

## ⚡ Step 5. Sync Project

```bash
npx cap sync android
```

---

## ⚡ Step 6. Open in Android Studio

```bash
npx cap open android
```

---

# 🔐 Sign & Generate Release APK

## Step 1. Create a Keystore (only once)

```bash
keytool -genkey -v -keystore release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias release
```

- `release-key.jks` → your keystore file (keep it safe).  
- `alias` → nickname for the key (here we used `release`).  

You will be asked for a password, name, organization, etc.  

---

## Step 2. Configure Gradle for Signing

Edit **`android/app/build.gradle`**.  
Inside the `android { ... }` block, add:

```gradle
android {
    namespace "com.google"
    compileSdkVersion 34

    defaultConfig {
        applicationId "com.google"
        minSdkVersion 23
        targetSdkVersion 34
        versionCode 1
        versionName "1.0"
    }

    signingConfigs {
        release {
            storeFile file("release-key.jks")
            storePassword "your-keystore-password"
            keyAlias "release"
            keyPassword "your-key-password"
        }
    }

    buildTypes {
        release {
            minifyEnabled false
            shrinkResources false
            signingConfig signingConfigs.release
        }
    }
}
```

Replace the passwords with the ones you entered when creating the keystore.

---

## Step 3. Build Signed APK

### 🔹 Option A: From Android Studio  
- **Build > Generate Signed Bundle / APK**  
- Choose **APK**  
- Select **release** build  
- Enter your keystore details  
- Click **Finish**  

### 🔹 Option B: From Command Line  

```bash
# On Linux/Mac
./gradlew assembleRelease

# On Windows (CMD/PowerShell)
gradlew assembleRelease
```

---

## Step 4. Find Your Signed APK

Your release APK will be generated here:

```
android/app/build/outputs/apk/release/app-release.apk
```

---

# 🎨 Change App Icon

### Option 1: Replace Manually  
Replace the PNG files in:  
```
android/app/src/main/res/mipmap-*/
```

### Option 2: Use Android Studio Asset Studio (Recommended)  

1. Open your project in Android Studio:  
   ```bash
   npx cap open android
   ```
2. Right-click **`res`** → **New → Image Asset**  
3. Choose your PNG file as the **launcher icon**  
4. Android Studio will generate all required sizes  
5. Run the app 🎉  

---

✅ Now you have a signed APK with your own app icon and WebView pointing to your website.  
