# API Reference

## Class: `CookiePrime`
The main entry point for the CookiePrime consent engine.

### `init(Application context, String licenseKey)`
Initializes the SDK, scans for dependencies, and automatically configures intercepted trackers based on saved consent.
- **`context`**: The Android Application context.
- **`licenseKey`**: Your commercial license key, or `"TRIAL-12345678"` for a 30-day free trial.

### `showConsentDialog(Activity activity)`
Triggers the UI to display the consent banner to the user.
- **`activity`**: The current foreground Activity.

### `boolean isConsentGiven(String category)`
Checks if the user has opted-in to a specific tracking category. Useful for manually initializing custom SDKs.
- **`category`**: The tracking category (e.g., `"analytics"`, `"advertising"`, `"essential"`).
- **Returns**: `true` if consent is given or if the category is `"essential"`; `false` otherwise.
