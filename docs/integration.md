CookiePrime Android SDK — Integration Guide v2.0
Full integration in under 10 minutes. Proven with Tic Tac Toe (2026).
________________________________________
📋 Table of Contents
1.	Prerequisites
2.	Quick Start
3.	Step 1: Add the AAR
4.	Step 2: Configure Gradle
5.	Step 3: Create Application Class
6.	Step 4: Update AndroidManifest.xml
7.	Step 5: Show the Consent Dialog
8.	Step 6: Check Consent in Your Code
9.	Verification
10.	Troubleshooting
________________________________________
Prerequisites
Requirement	Version
Android Studio	Hedgehog (2023.1.1) or later
Gradle	8.5 or later
Android Gradle Plugin	8.3.0 or later
minSdk	21 (Android 5.0)
targetSdk	34 (Android 14)
Java	17 or 21
Kotlin	1.9.22 or later
________________________________________
Quick Start
3 steps to integrate:
kotlin
// Step 1: Add the AAR to app/libs/
// Step 2: Initialize in Application class
CookiePrime.init(this, "YOUR_LICENSE_KEY");
// Step 3: Show consent dialog in your main Activity
CookiePrime.showConsentDialog(this);
________________________________________
Step 1: Add the AAR
1.1 Create the libs folder
bash
mkdir -p app/libs
1.2 Copy the AAR
Place the cookieprime-android-sdk-release.aar file into app/libs/
text
YourApp/
└── app/
    └── libs/
        └── cookieprime-android-sdk-release.aar   ← Copy here
________________________________________
Step 2: Configure Gradle
2.1 Update app/build.gradle (or build.gradle.kts)
For Groovy (build.gradle):
gradle
plugins {
    id 'com.android.application'
    id 'kotlin-android'      // Required
    id 'kotlin-kapt'         // Required for Room
}

android {
    namespace 'com.yourcompany.yourapp'
    compileSdk 34

    defaultConfig {
        applicationId "com.yourcompany.yourapp"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_17
        targetCompatibility JavaVersion.VERSION_17
    }

    kotlinOptions {
        jvmTarget = '17'
    }
}

dependencies {
    // ===== COOKIEPRIME SDK - ONE LINE =====
    implementation files('libs/cookieprime-android-sdk-release.aar')

    // ===== OPTIONAL: Add if you have trackers to detect =====
    implementation platform('com.google.firebase:firebase-bom:32.7.0')
    implementation 'com.google.firebase:firebase-analytics-ktx'
    implementation 'com.facebook.android:facebook-android-sdk:16.3.0'

    // ===== YOUR APP'S OTHER DEPENDENCIES =====
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
For Kotlin DSL (build.gradle.kts):
kotlin
plugins {
    id("com.android.application")
    id("org.jetbrains.kotlin.android")
    id("kotlin-kapt")
}

android {
    namespace = "com.yourcompany.yourapp"
    compileSdk = 34

    defaultConfig {
        applicationId = "com.yourcompany.yourapp"
        minSdk = 21
        targetSdk = 34
        versionCode = 1
        versionName = "1.0"
    }

    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_17
        targetCompatibility = JavaVersion.VERSION_17
    }

    kotlinOptions {
        jvmTarget = "17"
    }
}

dependencies {
    implementation(files("libs/cookieprime-android-sdk-release.aar"))
    
    // Your app dependencies
    implementation("androidx.appcompat:appcompat:1.6.1")
}
2.2 Update gradle.properties
properties
# Required for CookiePrime
android.useAndroidX=true
android.enableJetifier=true
kotlin.jvm.target.validation.mode=warning
android.suppressUnsupportedCompileSdk=34

# Recommended for performance
org.gradle.jvmargs=-Xmx2048m -XX:MaxMetaspaceSize=512m
org.gradle.parallel=true
________________________________________
Step 3: Create Application Class
3.1 Java version
Create app/src/main/java/com/yourcompany/yourapp/MyApplication.java:
java
package com.yourcompany.yourapp;

import android.app.Application;
import android.util.Log;

import com.cookieprime.CookiePrime;

public class MyApplication extends Application {
    private static final String TAG = "CookiePrimeApp";

    @Override
    public void onCreate() {
        super.onCreate();

        Log.d(TAG, "🚀 === APP STARTING ===");

        // ===== COOKIEPRIME INITIALIZATION - ONE LINE =====
        // Use "TRIAL-12345678" for 30-day free trial
        CookiePrime.init(this, "TRIAL-12345678");

        // Or with debug mode enabled:
        // CookiePrime.init(this, "TRIAL-12345678", config -> {
        //     config.setEnableDebug(true);
        //     return null;
        // });

        Log.d(TAG, "✅ === APP READY ===");
    }
}
3.2 Kotlin version
Create app/src/main/java/com/yourcompany/yourapp/MyApplication.kt:
kotlin
package com.yourcompany.yourapp

import android.app.Application
import android.util.Log
import com.cookieprime.CookiePrime

class MyApplication : Application() {
    companion object {
        private const val TAG = "CookiePrimeApp"
    }

    override fun onCreate() {
        super.onCreate()

        Log.d(TAG, "🚀 === APP STARTING ===")

        // ===== COOKIEPRIME INITIALIZATION - ONE LINE =====
        CookiePrime.init(this, "TRIAL-12345678")

        Log.d(TAG, "✅ === APP READY ===")
    }
}
3.3 License Key Options
License Key	Type	Description
TRIAL-12345678	Trial	30 days free, full features
CP-XXXXX-XXXXX-XXXXX	Commercial	Full commercial license
DEV-XXXXXX	Development	For testing only
________________________________________
Step 4: Update AndroidManifest.xml
4.1 Full AndroidManifest.xml
xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.yourcompany.yourapp">

    <!-- ===== REQUIRED PERMISSIONS ===== -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:name=".MyApplication"          ← MUST match your Application class
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme">

        <!-- Your Main Activity -->
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <!-- ===== COOKIEPRIME CONSENT ACTIVITY ===== -->
        <!-- IMPORTANT: Must be declared exactly as shown -->
        <activity
            android:name="com.cookieprime.consent.ui.ConsentActivity"
            android:exported="false"
            android:theme="@style/AppTheme"
            tools:replace="android:theme" />

    </application>

</manifest>
4.2 Important Notes
Element	Requirement
android:name=".MyApplication"	Must match your Application class name
ConsentActivity	Must be declared exactly as shown
tools:replace="android:theme"	Required to avoid theme conflicts
INTERNET permission	Required for license validation
________________________________________
Step 5: Show the Consent Dialog
5.1 Java Version
In your MainActivity.java:
java
package com.yourcompany.yourapp;

import android.os.Bundle;
import android.os.Handler;
import android.util.Log;
import androidx.appcompat.app.AppCompatActivity;

import com.cookieprime.CookiePrime;

public class MainActivity extends AppCompatActivity {
    private static final String TAG = "CookiePrimeApp";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Log.d(TAG, "🎮 === MainActivity STARTED ===");

        // ===== SHOW CONSENT DIALOG =====
        // Wait 500ms to let the app fully load
        new Handler().postDelayed(() -> {
            try {
                CookiePrime.showConsentDialog(this);
                Log.d(TAG, "Consent dialog triggered");
            } catch (Exception e) {
                Log.e(TAG, "Failed to show consent", e);
            }
        }, 500);
    }
}
5.2 Kotlin Version
In your MainActivity.kt:
kotlin
package com.yourcompany.yourapp

import android.os.Bundle
import android.os.Handler
import android.util.Log
import androidx.appcompat.app.AppCompatActivity
import com.cookieprime.CookiePrime

class MainActivity : AppCompatActivity() {
    companion object {
        private const val TAG = "CookiePrimeApp"
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        Log.d(TAG, "🎮 === MainActivity STARTED ===")

        // ===== SHOW CONSENT DIALOG =====
        Handler().postDelayed({
            try {
                CookiePrime.showConsentDialog(this)
                Log.d(TAG, "Consent dialog triggered")
            } catch (e: Exception) {
                Log.e(TAG, "Failed to show consent", e)
            }
        }, 500)
    }
}
5.3 Alternative: Show Consent on Button Click
java
// Java
findViewById(R.id.button_show_consent).setOnClickListener(v -> {
    CookiePrime.showConsentDialog(this);
});
kotlin
// Kotlin
findViewById<Button>(R.id.button_show_consent).setOnClickListener {
    CookiePrime.showConsentDialog(this)
}
________________________________________
Step 6: Check Consent in Your Code
6.1 Java
java
import com.cookieprime.CookiePrime;

// Check if analytics consent is given
if (CookiePrime.isConsentGiven("analytics")) {
    // Initialize Firebase Analytics
    FirebaseAnalytics.getInstance(context);
}

// Check if advertising consent is given
if (CookiePrime.isConsentGiven("advertising")) {
    // Show personalized ads
    MobileAds.initialize(context);
}

// Check if essential (always true)
if (CookiePrime.isConsentGiven("essential")) {
    // Essential functionality - always allowed
}

// Check multiple categories
if (CookiePrime.isConsentGiven("analytics") && 
    CookiePrime.isConsentGiven("marketing")) {
    // Both analytics and marketing allowed
}
6.2 Kotlin
kotlin
import com.cookieprime.CookiePrime

// Check if analytics consent is given
if (CookiePrime.isConsentGiven("analytics")) {
    // Initialize Firebase Analytics
    FirebaseAnalytics.getInstance(context)
}

// Check if advertising consent is given
if (CookiePrime.isConsentGiven("advertising")) {
    // Show personalized ads
    MobileAds.initialize(context)
}

// Check if essential (always true)
if (CookiePrime.isConsentGiven("essential")) {
    // Essential functionality - always allowed
}
6.3 Available Categories
Category	Description	Always Allowed
essential	Required for app to function	✅ Always true
analytics	Analytics & Performance	❌ User choice
advertising	Personalized Advertising	❌ User choice
social	Social Features	❌ User choice
performance	Performance Monitoring	❌ User choice
location	Location Services	❌ User choice
________________________________________
Verification
Build and Install
bash
# 1. Clean build
gradlew clean

# 2. Build debug APK
gradlew assembleDebug

# 3. Install on device
gradlew installDebug
Monitor Logs
bash
# Monitor all CookiePrime logs
adb logcat -s "CookiePrime" -s "CookiePrimeScanner" -s "CookiePrimeBlock" -s "ConsentActivity" -s "CookiePrimeApp" -v color
Expected Output
text
D/CookiePrimeApp: 🚀 === APP STARTING ===
D/CookiePrime: 🚀 Starting CookiePrime initialization...
D/CookiePrimeScanner: === STARTING COMPREHENSIVE DEPENDENCY SCAN ===
D/CookiePrimeScanner: ✅ Detected X SDKs total
D/ConsentActivity: === ConsentActivity.onCreate() STARTED ===
D/CookiePrimeBlock: 🛡️ === CONFIGURING SDKs BASED ON CONSENT ===
D/CookiePrimeApp: ✅ === APP READY ===
Verify Consent Saved
bash
# Check consent file
adb shell run-as com.yourcompany.yourapp cat /data/data/com.yourcompany.yourapp/shared_prefs/cookieprime_consent_simple.xml
________________________________________
Troubleshooting
Build Errors
Error	Fix
kapt not found	Add id 'kotlin-kapt' to plugins
Namespace not specified	Add namespace 'com.yourcompany.yourapp' to android block
SDK location not found	Create local.properties with sdk.dir=C:\\...
Unsupported class file major version	Use Java 17 or update Gradle to 8.5+
Runtime Errors
Error	Fix
NoClassDefFoundError: OkHttpClient	Add OkHttp to your app build.gradle
ConsentActivity not found	Add ConsentActivity to AndroidManifest.xml
No banner showing	Call CookiePrime.showConsentDialog(this) in your Activity
License invalid    Use TRIAL-12345678 for testing


Important: CookiePrime SDK communicates with your server over HTTP. For Android 9+ (API 28+), you must add the following to your AndroidManifest.xml:
xml
<application
    android:usesCleartextTraffic="true"
    tools:targetApi="28">






	

Full Diagnostic Script
bash
#!/bin/bash
echo "=== COOKIEPRIME DIAGNOSTIC ==="

# 1. Check app is installed
adb shell pm list packages | grep com.yourcompany.yourapp
# 2. Check logs
adb logcat -d -s "CookiePrime" "ConsentActivity"
# 3. Check consent file
adb shell run-as com.yourcompany.yourapp cat /data/data/com.yourcompany.yourapp/shared_prefs/cookieprime_consent_simple.xml
# 4. Check permissions
adb shell dumpsys package com.yourcompany.yourapp | grep permission
________________________________________
📋 Complete File Structure
text
YourApp/
├── app/
│   ├── libs/
│   │   └── cookieprime-android-sdk-release.aar   ← Step 1
│   ├── src/
│   │   └── main/
│   │       ├── java/
│   │       │   └── com/
│   │       │       └── yourcompany/
│   │       │           └── yourapp/
│   │       │               ├── MyApplication.java   ← Step 3
│   │       │               └── MainActivity.java    ← Step 5
│   │       ├── res/
│   │       └── AndroidManifest.xml                 ← Step 4
│   └── build.gradle                                ← Step 2
├── build.gradle
├── settings.gradle
└── gradle.properties
________________________________________
✅ Integration Checklist
Step	Task	Status
1	Copy AAR to app/libs/	☐
2	Update app/build.gradle	☐
3	Create MyApplication.java	☐
4	Update AndroidManifest.xml	☐
5	Add consent dialog to MainActivity.java	☐
6	Build and test	☐
________________________________________
📞 Support
Purpose	Contact
Commercial Licensing	contact@cookieprime.com
Technical Support	support@cookieprime.com


