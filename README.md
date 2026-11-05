# Buds Core Control — cloud build for ESign

**Target:** iPhone 16 Pro / iOS 26  
**Signing/install app:** ESign  
**Bundle ID:** `com.vlad.BudsCoreControl`

## What this project does
- Native SwiftUI interface.
- BLE scan, connect, and GATT service/characteristic inspection.
- Diagnostic report shown/exported by the app.
- ANC / Ambient / Off controls remain disabled until the Buds Core command transport and packet format are verified. The app does not send guessed commands.

## Build without owning a Mac
The included GitHub Actions workflow builds an **unsigned IPA** on a GitHub-hosted macOS runner. ESign then signs that IPA on the iPhone using your `.p12` and `.mobileprovision`.

### One-time GitHub setup from iPhone
1. Open `https://github.com` in Safari and sign in (or create an account).
2. Create a new **private** repository, for example `buds-core-control`. Do not add a README, license, or `.gitignore` during creation.
3. In the repository, choose **Add file → Upload files** and upload the file `BudsCoreControl-ESign-starter.zip` from this package. Commit the upload to the `main` branch.
4. Choose **Add file → Create new file**.
5. Enter this exact filename: `.github/workflows/ios-build.yml`.
6. Paste the contents of the separate `ios-build.yml` file included in this package, then commit it to `main`.
7. Open the repository's **Actions** tab. If GitHub asks to enable workflows, enable them.
8. Select **Build Buds Core Control IPA** → **Run workflow** → **Run workflow**.
9. Wait for the run to finish with a green check. Open the completed run and download the artifact named `BudsCoreControl-unsigned-IPA`.
10. In the iPhone Files app, open the downloaded artifact ZIP. It contains `BudsCoreControl-unsigned.ipa`.

### Sign and install with ESign
1. Import your `.p12` certificate into ESign and enter its password locally.
2. Import the matching `.mobileprovision` profile.
3. Import `BudsCoreControl-unsigned.ipa` into ESign.
4. Use ESign's signing function, selecting the matching certificate and profile; keep the app Bundle ID as `com.vlad.BudsCoreControl` unless your profile requires another explicit App ID.
5. Install the signed app. If ESign reports a profile, entitlement, or device-registration error, stop and share the exact error text (do not share the certificate, profile, or password).

## Signing requirements
- For a Development or Ad Hoc profile, the iPhone's UDID must be included in the profile.
- The profile's App ID must match `com.vlad.BudsCoreControl` or cover it with a wildcard.
- A `.p12` alone does not authorize installation. The certificate and profile must be a compatible pair.
- This workflow creates an **unsigned** IPA. ESign signs it locally; the workflow never receives your certificate, provisioning profile, or password.

## Bluetooth limitation
CoreBluetooth on iOS provides BLE GATT access, not a general-purpose Bluetooth Classic SPP/RFCOMM socket. Samsung's private control transport for this specific model is not verified. BLE scanning may help collect diagnostic information, but it cannot enumerate Classic SPP/RFCOMM services. ANC/Ambient switching is therefore not claimed to work yet.

## Build locally (optional)
On macOS with Xcode and XcodeGen installed:

```sh
xcodegen generate --spec project.yml
xcodebuild -project BudsCoreControl.xcodeproj -scheme BudsCoreControl -configuration Release -sdk iphoneos -destination 'generic/platform=iOS' CODE_SIGNING_ALLOWED=NO build
```
