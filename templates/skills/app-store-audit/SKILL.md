---
name: "app-store-audit"
description: "Check [APP_NAME] against Google Play and Apple App Store rejection rules with the App Store Compliance Playbook. Use before a release or a store submission, after a store rejection, or when a developer asks about store policy, the Play Data Safety form, permissions, privacy manifests, or account deletion rules."
---

<!-- templates v1.1.0: skills/app-store-audit/SKILL.md -->

# App Store Audit

Runs the [App Store Compliance Playbook](https://github.com/mjmirza/app-store-compliance) guard on [APP_NAME] and turns its findings into a fix list. The audit only warns. It never stops a release: the developer decides whether to continue.

The playbook is a git submodule at `.agents/vendor/app-store-compliance`. Dependabot opens a pull request when it changes upstream. Do not edit files inside it.

## Steps

1. **Get the playbook.** From the repository root:
   ```
   git submodule update --init --depth 1 .agents/vendor/app-store-compliance
   ```
   A fresh clone or a cloud session does not include submodules, so always run this. If it fails (no network), tell the developer and stop.
2. **Run the guard.** From the repository root:
   ```
   bash .agents/vendor/app-store-compliance/agent-os/hooks/app-store-compliance-guard.sh "$PWD"
   ```
   [NATIVE_FOLDERS_NOTE: say what the guard can see in this repository. For example, for a managed Expo app: "`android/` and `ios/` are not committed. Without them (for example in a cloud session) the guard reads only `app.config.ts`, and the native code shows as NOT CHECKED. Say so in the report. On the build machine, after `npx expo prebuild`, the native code is scanned too." For a native app with committed project folders, delete this note.]
3. **Read the verdict** (the last lines of the output):
   - `BLOCKED.` Critical rejection risks. Show the developer each `[CRITICAL]` line with its file, line, and the `fix.` line under it. Say clearly that these are likely to cause a store rejection, then ask whether to fix them now or continue. Do not set `APP_STORE_GUARD_OK` and do not hide findings.
   - `CLEAR.` No critical risk in the code. This is not an approval: list the manual checks in step 5 that are still open.
   - `Rule pack STALE.` The rules are more than 60 days old. Tell the developer to merge the open Dependabot pull request for the playbook, or to update the submodule.
   - `NOT CHECKED.` No project was found or the temp folder is not writable. Report the reason.
4. **Add context.** Read the matching guideline files in `.agents/vendor/app-store-compliance/references/guidelines/by-app-type/` ([APP_TYPE_GUIDELINES, for example `universal-every-app.md` and `games.md`]) and the rule files in `references/rules/` ([RULE_FILES, for example `android.md`, `privacy.md`, `safety.md`]) that relate to the findings. Base each fix on the cited guideline, never on a guess.
5. **Manual checks.** Go through `.agents/vendor/app-store-compliance/docs/PRE-SUBMISSION-CHECKLIST.md` with the developer for what a scan cannot see: developer account and agreements, store listing, Data Safety form, demo account, privacy policy link.
6. **Report.** Give a short list ranked by severity: critical, then warnings, then open manual checks. Each item has the file and line, the guideline, and the fix.

## Notes

- [DISTRIBUTION_NOTE: for example "The app ships an APK on GitHub Releases today. Rules that apply only to store listings (Data Safety form, payments, listing metadata) are advice until the Play submission starts." Delete when the app is already in a store.]
- Project-specific store rules belong in this file, not in the submodule.
