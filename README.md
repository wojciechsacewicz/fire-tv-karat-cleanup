# Fire TV Stick 4K Max (2nd generation) cleanup notes

A model-specific report from one owner's device, tested on 23 October 2026. The owner has confirmed the setup works after reboot and marked usage testing complete. This is not a universal compatibility claim or a guarantee of reliable rooting.

## Tested device

| Field | Observed value |
|---|---|
| Device | Amazon Fire TV Stick 4K Max, 2nd generation |
| Model / board | AFTKRT / karat |
| Fire OS | 8.1.7.4 |
| Build | RS8174.3648N |
| System incremental | 0031877644420 |
| Boot-image fingerprint incremental | 0031877644288 |
| Kernel | 5.10.43-android12-9-ge1cb3cb5ad98 |
| App ABI used | armeabi-v7a (32-bit Android userspace) |

The two incremental values came from different properties on the same unit. They are not proof that another build has equivalent exploit or flashing compatibility.

## Result

- Aerial STANDARD 1.41 replaces Amazon Home. The owner confirmed it remained Home after a reboot on 23 October 2026.
- Twenty-two selected packages were disabled without root, with persistence checked across two earlier reboots.
- Native Settings opens from Aerial through a small Settings bridge.
- Nuvio, SmartTube Beta, Netflix and Spotify are available from Home.
- The open-source AirPlay receiver was tested with an iPhone and PIN pairing.
- Native remote shortcuts were set to 1 = Nuvio and 2 = Spotify, without a background remapping app. The owner confirmed the configured setup works after reboot.

## Guides

- [Cleanup without root](CLEANUP.md).
- [Temporary-root build and run guide](ROOT-GUIDE.md).
- [Required tools and downloads](DOWNLOADS.md).
- [Home, Settings and native remote-button configuration](EXISTING-ROOT.md).
- [The full story](STORY.md).
- [Upstream sources and APK hashes](SOURCES.md).

## Scope

Tested on the model and firmware above. Post-reboot operation and usage acceptance are owner-confirmed. The elapsed test duration was not independently recorded. We do not claim universal compatibility or a measured speedup.

The root payload disables OTA updates and clears updater data, and its successful session had kernel metadata side effects. Read the root guide before using it. No bootloader unlock or partition flashing was performed.

Appstore, native Settings, account, input, Alexa and casting services were retained. Hiding a launcher tile does not disable its package or remove every Amazon service or in-app advertisement.
