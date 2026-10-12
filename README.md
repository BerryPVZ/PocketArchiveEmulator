# PocketArchive

A legacy mobile app preservation project for Android. Import your collection, browse a catalog, choose an HLE engine, and bring compatible old iOS apps back to life.


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


## Credits and preservation

PocketArchive builds on the work of the [touchHLE contributors](https://github.com/touchHLE/touchHLE), [BerryPVZ's HyperHLE fork](https://github.com/BerryPVZ/HyperHLE-Fork), and [KlugKlugTG's HyperHLE fork](https://github.com/KlugKlugTG/HyperHLE-Fork). Catalog data comes from [LegStore](https://github.com/BerryPVZ/LegStore).

This project is intended for preservation. Use apps you are entitled to preserve and respect the rights of their creators. PocketArchive does not grant redistribution rights to downloaded IPAs or APKs containing them, and does not bypass executable encryption.

Source files and bundled components retain their applicable license notices. Consult the included license files and upstream projects before redistributing code, native libraries, or standalone wrappers.
