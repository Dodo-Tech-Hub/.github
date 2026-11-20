
<!-- templates v1.0.0: speckit-constitution/mobile.md -->
## Mobile Principles

### Mobile Security (OWASP MASVS)

Every feature MUST meet [OWASP MASVS](https://mas.owasp.org/MASVS/) v2 at the MAS-L1 testing profile. These rules are the ones that most often apply:

- **Storage (MASVS-STORAGE)**: store tokens and secrets only in the platform keystore (iOS Keychain, Android Keystore, or a wrapper over them). Never in plain preferences, files, or a database. Exclude sensitive data from cloud and device backups.
- **No secrets in the app (MASVS-CODE)**: the app binary is public. Never ship client secrets, private API keys, or credentials. Every build-time config value is public.
- **Sign-in (MASVS-AUTH)**: follow [RFC 8252](https://www.rfc-editor.org/rfc/rfc8252) (OAuth 2.0 for native apps): use the platform's native sign-in SDK or the system browser with PKCE ([RFC 7636](https://www.rfc-editor.org/rfc/rfc7636)), never an embedded WebView. Never collect passwords of third-party services. Revoke and delete tokens on sign-out.
- **Network (MASVS-NETWORK)**: keep iOS App Transport Security on and Android cleartext traffic off.
- **Platform (MASVS-PLATFORM)**: export only the components the app needs (activities, services, receivers, providers). Treat intents, deep links, and data from other apps as untrusted, and handle only expected routes.
- **Permissions**: request a device permission only when a spec requires it, at the moment it is needed, with an explanation the user can understand. Every spec lists, per feature: required permissions, special access (for example Accessibility or notification access), and any platform setting the user must change.

Rationale: a mobile app runs on a device the developer does not control, and its binary can be read by anyone.

### Platform Design and Accessibility

- Follow the platform design guidelines: [Material Design 3](https://m3.material.io) on Android and the [Apple Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/) on iOS.
- Meet [WCAG 2.2](https://www.w3.org/TR/WCAG22/) level AA where it applies to native apps. Every interactive element MUST have an accessibility label for TalkBack and VoiceOver.
- Touch targets are at least 48 by 48 dp on Android and 44 by 44 points on iOS.
- User-facing text uses plain language: short sentences, everyday words, no technical terms.
- Every screen spec names its loading, error, and empty states.
- Confirmation dialogs appear only for destructive or irreversible actions.

Rationale: users already know their platform's patterns. Following them makes the app easy to learn and usable by everyone.

### Adaptive Layout

- Choose layout from the current window width using the [Material 3 window size classes](https://developer.android.com/develop/ui/compose/layouts/adaptive/use-window-size-classes): Compact (below 600 dp), Medium (600 to 839 dp), Expanded (840 dp and above). Layout MUST NOT depend on device model or device type.
- Screens MUST work in portrait and landscape, at the largest system font size, and on a 320 dp wide window. No text or control may be drawn under cutouts, status bars, or system navigation areas.
- A configuration change (rotation, fold, resize, theme or language change) MUST NOT lose the current screen, user input, or running work.
- Every spec that adds or changes a screen states how it behaves in each size class and orientation, or states that the default single column applies.

Rationale: users run the app on many screen sizes, and foldables change size while the app is open.

### Platform Rules and Verification

- Follow the store and platform policies ([Google Play Developer Policy](https://play.google/developer-content-policy/), [App Store Review Guidelines](https://developer.apple.com/app-store/review/guidelines/)) and the terms of every third-party API the app uses, even when the app is distributed outside a store.
- Specs cover every supported platform and mark platform-specific behavior.
- A feature is done only after a manual check on the primary test device named in `AGENTS.md`, recorded in its `tasks.md`.

Rationale: emulators miss real-device behavior such as battery optimization, permissions, and vendor changes.
