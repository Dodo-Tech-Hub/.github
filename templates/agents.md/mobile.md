
<!-- templates v1.0.0: agents.md/mobile.md -->
## Mobile

### Build and run

```bash
[BUILD_COMMAND]      # for example ./gradlew assembleDebug or npx expo run:android
[TEST_COMMAND]       # for example ./gradlew test or npm test
[INSTALL_COMMAND]    # for example ./gradlew installDebug (needs a connected device)
```

- Needs: [SDK_REQUIREMENTS, for example the Android SDK via local.properties sdk.dir or ANDROID_HOME, Xcode for iOS].
- Builds: [BUILD_LOCATION, for example local only on the owner's machine, or a named CI service]. Do not assume a cloud build service, account, or environment variables that this file does not name.

### Devices

- Primary test device: [DEVICE, for example Google Pixel 8 Pro, Android SDK 36, connected over wireless adb].
- Supported platforms: [PLATFORMS, for example Android first, iOS later]. Specs cover all of them.

### Release

- Distribution: [CHANNEL, for example signed APK on GitHub Releases, Google Play, App Store].
- Release steps live in [RELEASE_DOC, for example docs/RELEASING.md].

### Platform constraints that shape the design

[PLATFORM_CONSTRAINTS: the limits a new agent must know before designing. Examples:]

- [targetSdk and minSdk, and why they are set that way.]
- [Permissions that need special access, ADB, or user steps in system settings.]
- [Background work limits: foreground services, battery optimization.]
- [Third-party API quotas and the terms that limit automation.]
