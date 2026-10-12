# Khata — Offline Android App

This version of Khata is designed to run fully offline after installation.

## Storage
All Khata data (people, transactions, interest settings, and app settings) is stored on the Android device using React Native AsyncStorage. No Express API, PostgreSQL database, or internet connection is required for normal app operation.

## Run locally
From the repository root:

```bash
pnpm install
pnpm --dir artifacts/khata start
```

## Build an APK
Install EAS CLI and log in to Expo:

```bash
npm install -g eas-cli
eas login
```

Then from `artifacts/khata`:

```bash
eas build:configure
eas build --platform android --profile preview
```

Set the Android preview profile's `buildType` to `apk` in `eas.json` if it is not already present.

## Offline behavior
- No API calls are used by the app.
- No PostgreSQL connection is used by the app.
- Data persists across app restarts.
- Removing/uninstalling the app can remove its local data, depending on Android behavior.
- Internet is only needed for development/cloud APK building, not for normal use of the installed app.

## Update notes (profile, fresh start, app lock, reminders)
- **First launch** asks for name, mobile number and email. Edit them later in Settings → pencil icon on the profile card.
- **Fresh start**: no demo khatas ship with the app. Older installs have the old demo people removed on first open.
- **App lock** (Settings → Data & Security → App lock): 4-digit PIN plus optional fingerprint. Locks every time the app is opened or returns from the background.
- **Reminders** are real local notifications (9:00 AM). Interest reminders fire `reminderDaysBefore` day(s) before each person's interest day; overdue reminders fire when interest is unpaid after that day. They are rebuilt after every change, so paid items stop reminding.
- New packages: `expo-local-authentication`, `expo-notifications`. After unzipping run `pnpm install`, then `cd artifacts/khata && npx expo install --fix` to pin exact SDK versions. Fingerprint and notifications need a real phone (Expo Go or an APK build).
