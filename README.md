# Fire TV Stick 4K Max (2nd generation) cleanup notes

A model-specific report from one owner's device, tested on 23 October 2026. This is a provisional guide, not a universal compatibility claim or a rooting tool.

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

- Aerial STANDARD 1.41 provides Home, with Nuvio, SmartTube Beta, Netflix, Spotify and Appstore in the dock.
- Twenty-two selected packages were disabled without root. Their state and the initial accessibility-based launcher setup survived two normal reboots.
- Privacy switches and featured-content autoplay were turned off through Amazon's menus.
- The open-source AirPlay receiver worked with an iPhone, including the TV PIN. Its delayed boot startup was also verified before the later root changes.
- Existing temporary root later allowed a real default Home, working native Settings through a small bridge, and native remote shortcuts 1 = Nuvio and 2 = Spotify.
- The final post-root Home configuration and remote shortcuts were tested during that running session. They have NOT been tested through a subsequent reboot.

Use [the no-root guide](CLEANUP.md) first. [The story](STORY.md) explains the decisions. [The existing-root reference](EXISTING-ROOT.md) records the additional configuration without bundling an exploit. [Sources](SOURCES.md) identify the upstream projects and selected APK hashes.

## Limits

This does not remove every Amazon service, every possible advertisement, or in-app advertising. Appstore, account services, input, native Settings, Alexa services and casting services were retained. Hiding a tile does not disable its package.

There is no measured performance improvement. Native Netflix launch was verified, but playback, DRM resolution and streaming quality were not verified. Spotify reached its login page; its playback was not tested. Preserved Alexa services are not evidence of a completed voice-search test.

The connected display advertised up to 1080p at 60 Hz, with an observed 59.94 Hz mode. Resolution was left on Auto. This does not establish the Stick's output limit on another display.

The root payload disabled OTA packages and cleared updater data. That is separate from the no-root cleanup, prevents normal updates and is not a default recommendation. Temporary root also left runtime kernel metadata side effects; an apparently stable session does not establish safety or repeatability. No bootloader unlock or partition flashing was performed.

No binaries, proprietary APKs, raw logs, account data, device identifiers, screenshots or firmware backups are distributed here. This is newly written documentation, not a copy of the private setup directory.
