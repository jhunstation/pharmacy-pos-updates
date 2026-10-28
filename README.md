# Pharmacy POS — update channel

This small **public** repository tells every installed Pharmacy POS (APK or HTML) when a new
version is out. It holds only `version.json` and the release files — never the source code
and never `keygen.json`.

The app reads:

```
https://raw.githubusercontent.com/jhunstation/pharmacy-pos-updates/main/version.json
```

when it opens (if online) and every 6 hours.

## One-time setup

1. Signed in as **jhunstation** on GitHub, create a **public** repository named exactly
   `pharmacy-pos-updates` (branch `main`) and upload this `version.json` to the top level.
2. The app is already set to read from `jhunstation/pharmacy-pos-updates` — nothing to change.
3. Build the APK and give it to clients as usual. From now on they will be told about updates.

## Testing it once (before any client has the app)

1. Install the 1.0.0 APK on your own phone, open it and log in.
2. In `version.json` on GitHub, change `"latest": "1.0.0"` to `"latest": "1.0.1"` and commit.
3. On the phone: More → App Update → **Check now**. The update popup appears.
   (raw.githubusercontent.com can take up to ~5 minutes to show the new file.)
4. Change `"latest"` back to `"1.0.0"` when done, so no client sees a fake update.

## Publishing an update (e.g. 1.0.1)

1. Put the new HTML in `PharmacyPOS/app/src/main/assets/index.html`, set `ver:'1.0.1'` in the
   `APP` line of the HTML, and raise `versionCode` / `versionName` in `PharmacyPOS/app/build.gradle`.
   Add a line for the new version in `CHANGES` in the HTML (shown on the "What's new" screen).
2. Build the APK (GitHub Actions or Android Studio). It must be signed with the **same key** as
   before (`app/debug.keystore` or your release keystore), or it will not install over the old app.
3. In `pharmacy-pos-updates`: **Releases → Draft a new release** → tag `v1.0.1` → attach
   `PharmacyPOS-1.0.1.apk` (and `Pharmacy-POS-App_v1.0.1.html` for browser users) → Publish.
4. Edit `version.json`:

```json
{
  "latest": "1.0.1",
  "min": "1.0.0",
  "date": "2026-11-01",
  "apk": "https://github.com/jhunstation/pharmacy-pos-updates/releases/download/v1.0.1/PharmacyPOS-1.0.1.apk",
  "html": "https://github.com/jhunstation/pharmacy-pos-updates/releases/download/v1.0.1/Pharmacy-POS-App_v1.0.1.html",
  "notes": { "en": ["What changed…"], "tl": ["Ano ang nagbago…"] }
}
```

Within a few minutes, every device that opens the app sees the update popup, a notification in
the bell, a red "!" on the gear, and "NEW v1.0.1" under More → App Update.

## Forcing an update

Set `"min"` to the new version (e.g. `"min": "1.0.1"`). Devices older than that are blocked with
an "Update required" screen until they install it. Use this only for serious bugs — data stays safe.

## What clients do

Tap **Download & install update** → the APK downloads → open it → **Install**. No uninstall needed;
data and license stay. After opening the new version they see a "What's new" screen once.

Admins can point a device to another `version.json` (for testing) in More → App Update →
Update source (admin).
