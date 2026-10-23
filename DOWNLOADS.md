# Tools and downloads

Checked 23 October 2026. Download from the original publishers. This repository does not host executable attachments.

| Needed item | Source | Use in this setup |
|---|---|---|
| Android Platform-Tools | [Google downloads](https://developer.android.com/tools/releases/platform-tools) | adb for USB control |
| Android NDK r29 | [Google NDK releases](https://github.com/android/ndk/releases/tag/r29), [download archive](https://github.com/android/ndk/wiki/Unsupported-Downloads) | Reproduces the successful Linux x86-64 build; r29 is older than the current NDK |
| GhostLock source | [Pinned revision](https://github.com/R0rt1z2/GhostLock/tree/2752d2e5aef40c5007f23f36ce89ee29e1a6a4bc) | PROJECT=karat source build |
| Local diagnostic patch | [root-diagnostic-logging.patch](root-diagnostic-logging.patch) | Logging-only changes, byte-identical result verified |
| Official karat 1.0.4 ZIP | [XDA attachment](https://xdaforums.com/attachments/ghostlock-karat-v1-0-4-zip.6386789/) | Verified archive only; not the successful artifact, not executed here |
| Settings bridge | [Kuphi's selected APK](https://github.com/Kuphi/fire-tv-karat-setup/blob/21f69f24fd7fb5c91b51b25714d669b356b86d01/karat-automation/assets/FireTV-Settings-Bridge.apk) | Native Settings entry point after the optional Home configuration |

The [root guide](ROOT-GUIDE.md) contains the pinned source, exact build flags, expected binary hash, manual single-attempt invocation and verification steps. [SOURCES.md](SOURCES.md) contains the launcher, streaming apps and AirPlay links and hashes.

Google's current NDK download page lists r30. This guide deliberately uses r29 to reproduce the earlier artifact, not because r29 is the newest release. Check Google's published checksums and release information before installing the toolchain.
