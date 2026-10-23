# Sources and selected artifacts

Reviewed on 23 October 2026. Versions below describe this setup, not a promise that they are the latest when you read this.

- [CleanYourTV contribution rules](https://github.com/Gathin23/cleanyourtv/blob/main/CONTRIBUTING.md). Device submissions require packages actually used for a few days, exact firmware identity and both index rows.
- [Existing Fire OS 8 report](https://github.com/Gathin23/cleanyourtv/blob/main/examples/amazon-fire-tv-stick-4k-2023/debloat.txt). Different firmware. Its AFTKRT notes informed conservative choices; they do not verify this unit.
- [Maintainer discussion on Fire TV scope](https://github.com/Gathin23/cleanyourtv/pull/467). The 22 October discussion requests debloat/troubleshooting separately from Home takeover. Use the PR discussion if an individual comment link changes.
- [Kuphi setup](https://github.com/Kuphi/fire-tv-karat-setup/tree/21f69f24fd7fb5c91b51b25714d669b356b86d01). Inspiration and selected Settings bridge, not an end-to-end workflow executed on this unit.
- [fire-tv-cleanup](https://github.com/itsmeterrylin/fire-tv-cleanup). Additional Fire OS accessibility-based Home experience on another device.
- [GhostLock](https://github.com/R0rt1z2/GhostLock). Upstream root research. Our setup documentation does not distribute the exploit or claim its safety.
- [Aerial 1.41](https://github.com/pengchujin/aerial-tv/releases/tag/v1.41). STANDARD APK. Maintainer's [Fire TV discussion](https://github.com/pengchujin/aerial-tv/issues/175) explicitly says Home takeover depends on firmware and was not tested by them.
- [Nuvio TV releases](https://github.com/NuvioMedia/NuvioTV/releases). Installed 1.1.0-beta.8 ARM32; check the original release asset before reproducing.
- [SmartTube](https://github.com/yuliskov/SmartTube). Installed 32.68 Beta ARM32.
- [AirPlay receiver 0.0.36](https://github.com/jqssun/android-airplay-server/releases/tag/v0.0.36). Not AirScreen or another similarly named commercial app.

## SHA-256 of selected installed APKs

| Artifact | SHA-256 |
|---|---|
| Aerial STANDARD 1.41 | 47e8b4f151280fa979798f59aa4658dfb2342b27dd6fd6c74002368f8ae787cf |
| Nuvio 1.1.0-beta.8 ARM32 | 508492a9277863a977428c715a4136b4af5dc997247e9a6f2d97e5dff84f8f67 |
| SmartTube Beta 32.68 ARM32 | a5cd4348126d53d7adf29517fc49ce17601f5cd6d7c05e9881c8b660c79d7e47 |
| AirPlay 0.0.36 | b8c7c595ed93f91562feedce92c27800f08f9c84df424f6ac2045e82324cf2a4 |
| Kuphi Settings bridge | 79820e925dd747cf37f57ee31c3e6f981792f421cc1afb1e5043bafd1afb4878 |

These identify the artifacts selected in this session. They do not replace publisher/signature verification. Download from the publisher; no APKs or upstream source files are mirrored here. Upstream projects retain their own licenses.
