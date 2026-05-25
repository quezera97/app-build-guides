# 📱 Capacitor Android WebView App (Full APK Build + Splash + Signing Guide)

This guide shows how to wrap a live website into an Android APK using Capacitor.

Example website:
https://your-site.com

---

## ⚡ 1. Install Capacitor

npm install @capacitor/core @capacitor/cli @capacitor/android

(Optional for icons + splash automation)

npm install @capacitor/assets

---

## ⚡ 2. Initialize Capacitor

npx cap init

Fill in:
- App Name → MyApp  
- App ID → com.myapp.name  
- Web Dir → dist  

Create folder if using remote website:

mkdir dist

---

## ⚡ 3. Capacitor Config (Remote WebView)

Create capacitor.config.json:

{
  "appId": "com.myapp.name",
  "appName": "MyApp",
  "webDir": "dist",
  "server": {
    "url": "https://your-site.com",
    "cleartext": true
  },
  "android": {
    "allowMixedContent": true
  },
  "plugins": {
    "SplashScreen": {
      "launchShowDuration": 2000,
      "launchAutoHide": true,
      "backgroundColor": "#ffffff",
      "showSpinner": false
    }
  }
}

---

## ⚡ 4. Add Android Platform

npx cap add android

---

## ⚡ 5. Sync Project

npx cap sync android

---

## 🎨 6. App Icons & Splash (Optional but recommended)

Create assets folder:

mkdir assets

Add:
- icon.png (1024x1024)
- splash.png (2000x2000)

Generate assets:

npx @capacitor/assets generate --android

---

## ⚡ 7. Open Android Studio

npx cap open android

---

## 📱 8. Splash Screen Setup (Manual – BEST CONTROL)

Add splash image:

android/app/src/main/res/drawable/splash.png

Edit launch_background.xml:

<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">

    <item android:drawable="@android:color/white" />

    <item>
        <bitmap
            android:gravity="center"
            android:src="@drawable/splash" />
    </item>

</layer-list>

---

## ⚡ Android 12+ Splash Screen

Edit themes.xml:

<style name="AppTheme.NoActionBarLaunch" parent="Theme.SplashScreen">

    <item name="windowSplashScreenBackground">#FFFFFF</item>

    <item name="windowSplashScreenAnimatedIcon">
        @drawable/splash
    </item>

    <item name="postSplashScreenTheme">
        @style/AppTheme
    </item>

</style>

---

## 🔐 9. Create Keystore (ONE TIME ONLY)

keytool -genkey -v -keystore release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias release

Keep this file SAFE forever.

---

## ⚙️ 10. Configure Signing (android/app/build.gradle)

android {

    signingConfigs {
        release {
            storeFile file("release-key.jks")
            storePassword "your-store-password"
            keyAlias "release"
            keyPassword "your-key-password"
        }
    }

    buildTypes {
        release {
            signingConfig signingConfigs.release
            minifyEnabled false
            shrinkResources false
        }
    }
}

---

## 📦 11. Build Release APK

Option A: Android Studio
- Build → Generate Signed Bundle / APK  
- Choose APK  
- Select release  

Option B: Terminal

./gradlew assembleRelease

APK output:
android/app/build/outputs/apk/release/app-release.apk

---

## ✅ DONE
You now have:
- WebView Android app
- Remote website loading
- Custom icon + splash screen
- Signed APK build
