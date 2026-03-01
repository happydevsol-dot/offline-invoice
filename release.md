# Android Release Build

Use a dedicated upload keystore for Play Console builds. Do not commit keystores or passwords to this repository.

## 1. Generate an upload keystore

Example:

```bash
keytool -genkeypair \
  -v \
  -storetype PKCS12 \
  -keystore ~/keystores/offline-invoice-upload.keystore \
  -alias offlineinvoiceupload \
  -keyalg RSA \
  -keysize 2048 \
  -validity 9125
```

This command prompts for the keystore password, key password, and certificate details.

## 2. Configure Gradle properties

Recommended location:

- `~/.gradle/gradle.properties`

Example entries:

```properties
OFFLINE_INVOICE_UPLOAD_STORE_FILE=~/keystores/offline-invoice-upload.keystore
OFFLINE_INVOICE_UPLOAD_STORE_PASSWORD=replace-with-keystore-password
OFFLINE_INVOICE_UPLOAD_KEY_ALIAS=offlineinvoiceupload
OFFLINE_INVOICE_UPLOAD_KEY_PASSWORD=replace-with-key-password
```

The Android build also accepts the same keys as environment variables. `~/.gradle/gradle.properties` is the preferred local setup because Gradle will load the values automatically for `bundleRelease`.

## 3. Build the Play upload bundle

```bash
cd android
./gradlew clean
./gradlew :app:bundleRelease
```

Bundle output:

- `android/app/build/outputs/bundle/release/app-release.aab`

If signing values are missing, the release build fails intentionally with a clear error.
