# Reference for changes made with existing temporary root

This records an observed configuration on the exact build in README.md. It is not a rooting procedure or an unattended installer. Have working ADB, a private baseline and a way to restore package/component state before adapting it. Protected restoration may require root again after reboot.

## Home and native Settings

Observed final state:

| Item | State |
|---|---|
| com.amazon.firehomestarter | Disabled for user 0 |
| com.amazon.tv.launcher | Package kept enabled |
| com.amazon.tv.launcher/.ui.HomeActivity_vNext | Disabled component |
| com.amazon.tv.settings.v2/.compliance.CTSDummySettingsHandler | Disabled component |
| com.amazon.tv.launcher/.ui.SettingsActivity | Disabled component |
| Default Home | app.orchard.tv/.MainActivity |
| Generic Settings handler | local.karat.settingsbridge/.MainActivity |
| Aerial accessibility HomeGuard | Removed from enabled services |
| Aerial overlay permission | Returned to default |

The bridge launches `com.amazon.tv.launcher.ui.MainSettingsActivity` and finishes. It was a normal installed app, not a system partition modification. The exact inspected APK had one activity/class, no requested permissions and no native libraries. Matching a repository checksum proves agreement with that artifact, not an independent reproducible-build or publisher authentication guarantee.

The bridge was installed before disabling the Settings trampolines. Standard Settings resolution, Aerial's actual Settings button and a native submenu were then checked. Disabling the entire Amazon launcher package would remove the Settings implementation we kept, so it is not equivalent to the component changes above.

To restore stock Home, first re-enable the three components and firehomestarter using existing root (`pm enable --user 0 COMPONENT_OR_PACKAGE`), then set Home back to `com.amazon.tv.launcher/.ui.HomeActivity_vNext` using `cmd package set-home-activity --user 0`. Check that stock Home and Settings actually open before removing the bridge. Do not blindly replace the global accessibility-services list; preserve any services the owner already uses. This rollback sequence was prepared locally but was not executed, and is not a tested recovery procedure.

## Native remote buttons

The observed preference file is `/data/user/0/com.amazon.vizzini/shared_prefs/CUSTOMIZABLE_SHORTCUT_SHARED_PREFS.xml`. Its string keys were `KEYCODE_BUTTON_1` and `KEYCODE_BUTTON_2`.

| Button | Direct launch target |
|---|---|
| 1 | com.nuvio.tv/.launcher.AppIconDefault |
| 2 | com.spotify.tv.android/.SpotifyTVActivity |

Values were Android-serialized intents with MAIN, LEANBACK_LAUNCHER, flags 0x10200000 and string extra EXECUTION_TYPE=LAUNCH_ACTIVITY. Android's `am to-intent-uri` generated the strings. Installed app versions can change activity names, so resolve the current installed targets rather than assuming these remain valid.

Only the two entries were changed after a private backup. File ownership, mode and SELinux label were preserved. The native service caches its preferences; refreshing only its process briefly interrupted Alexa and allowed it to restart. Do not kill unrelated system or root processes. To undo a mapping change, restore the owner's exact prior entries and metadata while root is available, then refresh and test the native mapping. Original mappings on this device were unassigned.

Do not publish full app-private preference directories or assume an input event number is portable. This unit's paired remote mapping was used to test the two physical-button paths; generic ADB key injection produced a different partner-button route. Both intended apps reached the foreground. A human button press is the appropriate final acceptance check for another owner's remote.

## Root side effects and persistence limits

The root payload independently disabled `com.amazon.device.software.ota`, `com.amazon.device.software.ota.override` and `com.amazon.client.metrics`, and cleared updater app data. Together with the 22 original disables and firehomestarter, that made 26 disabled packages. The three disabled components are separate from that count.

Temporary root, permissive SELinux and the mounted su overlay are runtime state. Package disables and preference edits are stored state. The owner confirmed Aerial remained Home after reboot on 23 October 2026. The owner subsequently confirmed the configured setup works after reboot and marked usage testing complete. This is owner confirmation rather than a new instrumented device check. A reboot removes runtime root and may leave protected packages that require root to restore. Enabling OTA packages cannot recover cleared updater data. No full recovery image was created.
