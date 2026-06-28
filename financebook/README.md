# FinanceBook Update Endpoint

This directory is the OTA update source for the FinanceBook desktop app.
`version.json` is polled at startup to detect new releases.

---

## How to ship an update

1. Build the new installer and name it (e.g. `FinanceBookSetup-1.1.0.exe`).
2. Upload the installer file to this directory (`financebook/`).
3. Edit `version.json`:
   - Bump `latest_version` to match the new build (must match `CURRENT_VERSION` in `updater.py`).
   - Update `download_url` if the filename changed.
   - Write human-readable release notes in `notes`.
4. Commit and push. GitHub Pages serves the update within ~1–2 minutes.

## Mandatory updates

Set `"mandatory": true` in `version.json` to force users to update before they can use the app.
The desktop app's updater must check this flag and block launch if it is `true`.
Use sparingly — only for security patches or breaking schema changes.

## Desktop app reminder

`CURRENT_VERSION` in `updater.py` must be set to the version string that was baked
into that specific build. If it doesn't match `latest_version` in this file, the app
will incorrectly report itself as out of date (or never out of date).

## File layout

```
financebook/
  version.json          # polled by the app — keep valid JSON at all times
  FinanceBookSetup.exe  # current installer (upload alongside version.json edit)
  README.md             # this file
```

## Notes on .exe serving

GitHub Pages will serve `.exe` files as binary downloads. Browsers treat them as
downloads by default (not inline). No extra server config is needed for the file to
download correctly. GitHub Pages does not support custom `Content-Disposition` headers
per file, but this is fine — browsers will not attempt to open an `.exe` inline.
