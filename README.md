# Spec Studio (Android, offline)

Offline app: projects, discovery questions, gap checker, and PRD / SRS / AGENTS.md / client summary / AI prompt pack generated from your answers. No internet or API key needed.

## Get the APK (no Android Studio needed)
1. Create a free GitHub account and a new **private** repository.
2. On the repo page tap **Add file > Upload files** and upload these 4 files: `index.html`, `package.json`, `capacitor.config.json`, `README.md`. Commit to the `main` branch.
2b. The build recipe must sit in a hidden folder. Tap **Add file > Create new file**. In the name box type exactly `.github/workflows/build-apk.yml` (typing the slashes creates the folders). Open `build-apk.yml` from this zip in any text app, copy everything, paste it into GitHub, and tap Commit.
   (If your file manager shows a `.github` folder, you can upload that instead. Many phones and computers hide folders starting with a dot.)
3. Open the **Actions** tab. The "Build APK" run starts by itself (about 5 to 10 minutes).
4. When it finishes, open the run and download **SpecStudio-debug-apk** (installs right away, for testing).

## Make the signed release APK
Do this once. Run on your computer (needs Java):

    keytool -genkeypair -v -keystore release.jks -alias specstudio -keyalg RSA -keysize 2048 -validity 10000

Back up `release.jks` and its passwords somewhere safe. If you lose them you can never update the app.

Convert the keystore to text:
- Mac: `base64 -i release.jks`
- Linux: `base64 -w0 release.jks`
- Windows PowerShell: `[Convert]::ToBase64String([IO.File]::ReadAllBytes("release.jks"))`

In GitHub: Settings > Secrets and variables > Actions > New repository secret. Add four:
`KEYSTORE_BASE64` (the text above), `KEYSTORE_PASSWORD`, `KEY_ALIAS` (specstudio), `KEY_PASSWORD`.

Then Actions > Build APK > Run workflow. Download **SpecStudio-signed-apk**.

## Install on your phone
Send the .apk to your phone, open it, and allow "Install unknown apps" for that app when asked.

## Notes
- Data is stored on the phone. Use Backup (home screen) to share a JSON copy and Restore to paste it back.
- Change the app name or id in `capacitor.config.json`. Edit the app itself in `index.html`.
- If the build fails, open the failed step in Actions and send me the error text.
