# PocketArchive

**Old apps. New pocket.**

A legacy mobile app preservation project for Android. Import your collection, browse a catalog, choose an HLE engine, and bring compatible old iOS apps back to life.

Previously called **Cosma**. Current release: **1.3**.

> Current emulation focuses on **legacy 32-bit ARM iOS apps**. The catalog includes entries from the iOS 2–10 era, but catalog availability does not mean an app is compatible with the bundled engines. Other mobile platforms are on the roadmap.

## Features

- **Your own app library** — import IPAs and display their original names and icons when readable.
- **Direct app launch** — start the selected app inside the emulator without another app picker.
- **Three HLE engines** — choose a global default or a different engine for each app.
- **Legacy app catalog** — browse and search catalog data sourced from LegStore.
- **Concurrent downloads** — download several apps at once with an adjustable worker limit.
- **Standalone APK export** — package an IPA and its selected emulator engine into an Android app.
- **Collection tools** — favorites, search, sorting, recent launches, and storage information.
- **First-launch welcome screen** and an icon zoom/fade opening animation, with a reduced-motion option.

## Requirements

| Requirement | Supported configuration |
| --- | --- |
| Android version | Android 6.0 or later |
| Device architecture | ARM64 / arm64-v8a |
| Current guest format | Legacy iOS `.ipa` |
| Current guest architecture | 32-bit ARM, subject to engine compatibility |
| Internet access | Needed for catalog downloads; local imports do not need downloading |

Allow enough free storage for downloaded IPAs, temporary files, and any APKs you export. Some apps depend on services that no longer exist.

## Getting started

1. Install **PocketArchive-1.1-arm64.apk** on a supported Android device.
2. Open PocketArchive and complete the welcome screen.
3. Select your default HLE engine.
4. Import a local IPA, or find an entry in the catalog and download it into your library.
5. Open an app from your library. Long-press a library item for quick launch.

You can reopen onboarding through **Tools → Welcome & quick guide**.

### Updating from Cosma or PocketArchive 1.0

The supplied 1.1 APK uses the same release signing key and Android application ID, `org.cosma.emulator.v2`, as Cosma 0.2.0 and PocketArchive 1.0. Install it over the existing app to retain its data. Cosma 0.1.x uses a different package and does not migrate automatically.

Do not uninstall your existing installation just to update: uninstalling can remove app data and the local signing key used for exported wrappers. A build signed with a different key cannot replace the existing installation directly.

## HLE engines

| Engine | Upstream project |
| --- | --- |
| BerryPVZ HyperHLE fork | [BerryPVZ/HyperHLE-Fork](https://github.com/BerryPVZ/HyperHLE-Fork) |
| KlugKlugTG HyperHLE fork | [KlugKlugTG/HyperHLE-Fork](https://github.com/KlugKlugTG/HyperHLE-Fork) |
| touchHLE | [touchHLE/touchHLE](https://github.com/touchHLE/touchHLE) |

Compatibility varies by app and engine. An app that fails with one engine may behave differently with another. PocketArchive bundles engine builds; selecting an engine does not automatically fetch the latest upstream version.

## Downloads

Open **↓** in the header or **Tools → Downloads / workers**.

- Choose **1–6 concurrent workers**. The default is **3**, and your setting is remembered.
- Track individual download states, transferred bytes, and total size when available.
- Cancel or retry one download, cancel all active downloads, or clear completed history.
- Download into your library or download a catalog entry for APK export through the same queue.
- Select **Make APK** on a completed download to open the exporter.

Raising the worker limit starts more waiting downloads. Lowering it lets existing transfers finish before the lower limit takes effect. Clearing history does not delete completed library IPAs.

Downloads can continue after leaving the download or export screen through an Android foreground service. Android battery policies or process termination can still interrupt them. Unfinished jobs restored after a process restart are marked **Interrupted** and require Retry. **Retries start from the beginning; byte-range resume is not implemented.**

Incoming files are staged and checked before appearing in the library. Duplicate active URLs share a job, and separate transfers use unique filenames to avoid overwriting one another. Up to 150 finished history entries are retained across launches.

## IPA → APK

Create a standalone Android wrapper that contains your IPA and the selected HLE engine. Opening that APK automatically launches the embedded app, without PocketArchive's library or an emulator app picker.

**This is emulation, not a native iOS-to-Android code conversion.** Exporting an APK does not make an incompatible app compatible.

### From a local IPA

1. Open the **IPA → APK** exporter and choose an IPA, or select a library app and use **App actions → Export as standalone APK**.
2. Review the detected name, version, icon, and architecture.
3. Choose an HLE engine.
4. Tap **Build standalone APK**, then **Save APK**.
5. Open the saved APK in your Android file manager to install it.

Keep PocketArchive open while building an APK.

### From a catalog entry

1. Select an entry in the catalog.
2. Choose **Get / make APK → Make APK from this entry**.
3. Tap **Download catalog IPA**.
4. Once the download completes, review the IPA's actual metadata and select an engine.
5. Build and save the APK.

Completed catalog downloads are also added to your library. A catalog record alone cannot produce a runnable APK: its IPA must be available and pass inspection.

### Export restrictions and signing

- ARM64-only IPAs and mixed 32/64-bit executables are rejected.
- Simulator-only and encrypted executables are rejected.
- Wrappers require an ARM64 Android device running Android 6.0 or later.
- Wrapper signing keys are stored in this PocketArchive installation's Android Keystore. Uninstalling PocketArchive loses that key, preventing same-key updates to those wrappers.
- Different engine selections produce separate wrapper packages and saves.

## Managing your collection

- Sort by name, recent opening, recent import, or size.
- Mark library favorites and filter with **★ Only**. **Catalog ★** is a separate catalog favorites view.
- Select an engine for an individual app or inherit the global default.
- Use **Tools → Storage overview** to inspect library size and available storage.
- Use **Tools → Reduce opening animation** to skip the launch transition.

Metadata and icons are read locally. Missing or unreadable icons use a fallback. Legacy CgBI PNG icons are supported; icons stored only in `Assets.car` are not currently decoded. Removing an imported IPA preserves its existing save data.


## Troubleshooting

| Problem | What to check |
| --- | --- |
| An app crashes, hangs, or renders incorrectly | Try another HLE engine and record the app version, engine, and error details. Successful import does not establish compatibility. |
| A catalog download fails | Retry from Downloads, check the source's availability, or import a locally preserved IPA. |
| A download says Interrupted | Retry it. The transfer will restart from byte zero. |
| APK export rejects an IPA | Check the reported architecture or encryption status. The exporter does not support 64-bit or encrypted executables. |
| An icon is missing | The IPA may lack a readable icon or store it in an unsupported asset format. |
| An APK update will not install | Check the device requirements, package, and signing key. Exported wrappers need the same signing key for updates. |

## Development and validation

The 1.1 Android release was built using **JDK 17, Gradle 8.11.1, Android Gradle Plugin 8.10.1, and Android SDK 35**, with minimum API 23. The release retains the existing native engine binaries.

The source includes regression tests under `cosma/tests`. From the project root, after building the Android release APK and configuring the Android SDK and Java environment:

The wrapper verification script also requires the Android build tools and the cached `apksig` dependency, or an explicit `APKSIG_JAR` path.

Validation covers queue concurrency and cancellation, retry and interrupted recovery, IPA inspection, download writes, and generated wrappers for all three engines, including signatures and alignment. **These checks do not establish successful gameplay or replace physical-device testing.** Repository-wide lint still includes inherited SDL/API/permission diagnostics.

## Contributing

Useful contributions include compatibility reports, reproducible fixes, accessibility improvements, catalog corrections, and work toward additional legacy platforms.

For a bug report, include:

- PocketArchive version, device model, and Android version.
- App name, version, and selected HLE engine.
- Reproduction steps and what happened versus what you expected.
- Relevant logs or screenshots, with personal information removed.

## Credits and preservation

PocketArchive builds on the work of the [touchHLE contributors](https://github.com/touchHLE/touchHLE), [BerryPVZ's HyperHLE fork](https://github.com/BerryPVZ/HyperHLE-Fork), and [KlugKlugTG's HyperHLE fork](https://github.com/KlugKlugTG/HyperHLE-Fork). Catalog data comes from [LegStore](https://github.com/BerryPVZ/LegStore).

This project is intended for preservation. Use apps you are entitled to preserve and respect the rights of their creators. PocketArchive does not grant redistribution rights to downloaded IPAs or APKs containing them, and does not bypass executable encryption.

Source files and bundled components retain their applicable license notices. Consult the included license files and upstream projects before redistributing code, native libraries, or standalone wrappers.
