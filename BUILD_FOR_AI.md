# Build instructions (for an AI agent or developer)

This is a complete, native Android project (Kotlin, single Activity, CameraX + ML Kit offline QR scanning,
Storage Access Framework for choosing the save folder). Goal: produce an installable **debug APK**.

## Requirements

| Item | Version |
|---|---|
| JDK | 17 |
| Android SDK platform | 34 (`platforms;android-34`) |
| Android build-tools | 34.0.0 |
| Gradle | 8.7 (wrapper included: `./gradlew`) |
| Android Gradle Plugin / Kotlin | 8.5.2 / 1.9.24 (already pinned in `build.gradle.kts`) |

Internet is needed on the first build to download Gradle and the dependencies (Google Maven + Maven Central).

## Build

```bash
# from the project root (the folder containing settings.gradle.kts)
chmod +x ./gradlew
./gradlew assembleDebug
```

Windows: `gradlew.bat assembleDebug`

If `ANDROID_HOME` is not set, create `local.properties` in the project root:

```
sdk.dir=/path/to/android/sdk
```

Output: `app/build/outputs/apk/debug/app-debug.apk`

## If the build fails

- Fix only what is needed to compile (versions, missing SDK package, import errors). Do **not** change the app's behaviour.
- Typical fixes: install `platforms;android-34` and `build-tools;34.0.0` with `sdkmanager`, accept licenses with `sdkmanager --licenses`.
- Report the exact error text if it cannot be fixed.

## Do not

- Do not hand over a placeholder or unsigned-from-source file. Only deliver an APK that came out of a successful build.

## App behaviour (for reference)

1. User picks a "work folder" (phone storage or USB/OTG drive) once; the choice is remembered.
2. Camera scans the QR on a TT slip. QR text = folder name, e.g. `FG-3673120100-01`.
3. If that folder exists in the work folder with 2+ images -> green "already done".
4. Otherwise capture mode: take 2 photos -> the app creates the folder and saves `<name>_1.jpg`, `<name>_2.jpg`.
