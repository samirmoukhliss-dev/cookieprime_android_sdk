# 🍪 CookiePrime: On-Device Consent Enforcement for Android

**93ms consent trace • 27+ SDKs detected • 380KB AAR • 10-minute integration**

> **Copyright © 2026 CookiePrime. All Rights Reserved.**
> This SDK is proprietary software protected under international copyright law. Commercial use requires a separate license agreement.

## 📋 The Problem with Traditional CMPs
Most Consent Management Platforms (CMPs) attempt to block tracking at the network layer. This is inefficient because:
- **SDKs initialize before the network call.**
- **Data is collected immediately upon initialization.**
- **Network-level blocking cannot "un-collect" data once it is sent.**

## 💡 The Solution: Block at Initialization
CookiePrime intercepts tracking SDKs at runtime initialization.
1. App starts → `CookiePrime.init(this, "LICENSE_KEY")`
2. Automatic scan (93ms) → Detects known/unknown SDKs.
3. Consent dialog appears → User chooses preferences.
4. SDKs are ALLOWED or BLOCKED at initialization.

## 🔍 Detection Capabilities
| Known SDKs | Unknown SDKs |
|------------|--------------|
| Firebase Analytics | Pattern-matched packages |
| Facebook SDK | Suspicious class loading |
| Google AdMob | Unknown permissions |
| Unity Ads | Suspicious DEX entries |
| AppsFlyer | Suspicious class structures |

**Detection methods:** Class loading, DEX scanning, Pattern matching, Reflection.

### 📊 Centralized Compliance Auditing
* **Live Audit Trail:** Automatically syncs user consent cryptographic flags to your secure dashboard.
* **Privacy-First Logging:** Full IP anonymization ensures zero PII (Personally Identifiable Information) touches your databases.
* **Secure Access:** A protected, password-gated compliance portal for real-time reporting during legal or platform audits.
* Login here for audit: http://34.14.109.247:5000/login
* Username: partner@cookieprime.com

## 🔧 One-Line Integration

**Step 1: Add the AAR**

```kotlin
dependencies {
    implementation files('libs/cookieprime-android-sdk-release.aar')
}
```

**Step 2: Initialize (Application class)**
```
public class MyApp extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        CookiePrime.init(this, "YOUR_LICENSE_KEY");
    }
}
```

**Step 3: Check consent anywhere**
```
if (CookiePrime.isConsentGiven("analytics")) {
    // Analytics is safe to use
}
```



🚀 Technical Specs


Metric	Value
AAR Size	380KB
Consent Trace	93ms average
Integration	< 10 minutes
minSdk	21 (Android 5.0)




🛡️ Legal & Copyright


This SDK is proprietary software.

    Trial/Testing: Use license key TRIAL-12345678 for 30 days free.
    
    Personal/Non-commercial: Free (no license required)

    ### 📜 Commercial License Benefits
While local on-device blocking works out of the box with the evaluation key, commercial license holders unlock our complete cloud auditing infrastructure:
- ✅ **Hosted Consent Ledger:** Secure storage of global user decisions.
- ✅ **Compliance Dashboard:** Password-gated, live-updating visual reports.
- ✅ **Anonymized Analytics:** Track opt-in/opt-out trends without tracking real people.

    Commercial: Commercial license required

Contact: contact@cookieprime.com

