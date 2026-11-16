# Android build and release

The Android app is a small WebView shell for the existing Streamlit service at
`https://bangla-word-analysis-htzr6coxs5ocscagkho6k2.streamlit.app/`. It does not
change or copy the original app's server-side code. It needs an internet connection.

## Security behavior

- The shell accepts only the exact HTTPS Streamlit host as in-app content.
- Cleartext HTTP and mixed content are disabled; certificate errors are rejected.
- Android WebView Safe Browsing is enabled on supported Android versions.
- Links to other HTTPS sites open in the phone's browser.
- The app does not ask for contacts, location, camera, microphone, or storage access.

These settings reduce app-side risks. They cannot suppress Android's unknown-source
or Play Protect warnings for a sideloaded APK, and they do not guarantee a store
approval. Users should install the Play release from Google Play. A new Play app is
published as a signed Android App Bundle (AAB); Play then generates device APKs.

## Release signing

Keep the release keystore and its passwords private and make a protected backup.
Never commit them to this repository. The same signing identity must be used for
updates distributed outside Google Play.

To sign GitHub Actions builds, add these repository Actions secrets:

- `ANDROID_KEYSTORE_BASE64`: base64-encoded release keystore file
- `ANDROID_KEYSTORE_PASSWORD`: keystore password
- `ANDROID_KEY_ALIAS`: key alias
- `ANDROID_KEY_PASSWORD`: key password

Then run the **Android build** workflow. It produces a signed release APK and AAB.
Without all four secrets, the workflow produces an unsigned release APK/AAB for
build review only; those files are not ready to distribute or upload to a store.

Choose and register the Android package name before the first store upload. This
project currently uses `com.nilanjanc.banglawordanalysis`. Package names are
permanent in Play Console.

## Current Play build settings

The project targets Android 16 (API 36), the API level required for new Play apps
starting August 31, 2026. Review current Play Console requirements and complete
the developer account, app content, privacy, and testing declarations before
publishing.
