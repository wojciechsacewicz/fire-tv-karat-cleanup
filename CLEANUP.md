# Cleanup without root

Scope: the AFTKRT build in README.md. These operations were performed on one unit on 23 October 2026. The owner subsequently confirmed the setup works after reboot and marked usage testing complete. The elapsed duration was not independently recorded.

## Connect and record the baseline

Keep the Stick on the TV with stable power. A laptop connection can use wireless ADB on the same network; HDMI is for the TV picture, not a laptop data connection. A USB connection requires a data-capable micro-USB cable. A cable that supplies power may carry no data.

Enable Developer Options under My Fire TV > About by selecting the device name seven times, then enable ADB Debugging. Authorize the computer on the TV. Discover your own device with `adb devices -l` and use an explicit device selector on every command. Do not copy another owner's serial or address.

Record package enable states before changing them. `adb -s DEVICE shell pm list packages -d` records disabled packages. Configuration dumps can contain private information, so keep them off public repositories. A package/settings snapshot is not a recovery image.

## Apps

Install from the publishers linked in SOURCES.md. This unit needed ARM32 APKs. Use the Amazon Appstore for Netflix and Spotify. Nuvio and SmartTube beta were the owner's preference, not evidence that beta releases are always more reliable.

Aerial can be installed as an app without root. Reliable Home replacement depends on firmware. This unit initially used Aerial's accessibility HomeGuard with Amazon Home retained. It returned to Aerial after reboot, but Aerial's control-center Settings action was unreliable. Opening the actual Amazon launcher tile allowed native Settings. The later existing-root configuration is documented separately.

For AirPlay, install the jqssun receiver, choose a distinct receiver name and require a PIN. On this unit its defaults for automatic startup, startup at boot and background operation were retained. No overlay permission was granted. iPhone mirroring and PIN were owner-tested. Netflix and other DRM-protected video should use native apps rather than relying on screen mirroring.

## Privacy and display

Through Settings > Preferences, turn off Device Usage Data, Collect App Usage Data and Interest-based Ads. Under Featured Content, turn off video and audio autoplay. Use the real menus rather than assuming generic Android setting keys alter Amazon's account-synced preferences.

Keep resolution on Auto. Animation scales were left stock because the upstream Fire OS 8 report documents broken Settings side-menu navigation at 0.5. That failure was not reproduced on this exact older build.

## Selected package changes

`packages-no-root.txt` records the 22 packages actually disabled. Review it against your apps first. It includes Amazon Kids, Amazon Music and shopping/game/tutorial components; keep anything you use. This is a record of this owner's choices, not a requirement to disable everything.

Apply one reviewed package at a time with `adb -s DEVICE shell pm disable-user --user 0 PACKAGE`. Restore that package with `adb -s DEVICE shell pm enable --user 0 PACKAGE`, only if it was enabled in your baseline. Stop and restore the latest change if a user flow breaks. Do not bypass protected-package errors.

The list does not include Amazon Home, Settings, OTA, Appstore, keyboard, remote/Bluetooth, account or casting components. It never uninstalls system packages. Eight unwanted launcher tiles were also hidden through Aerial without disabling the underlying services.

After a small batch, check Home, native Settings, Appstore, app launch, Wi-Fi and the physical remote. Check casting and actual playback. Reboot only when ready, then confirm the intended state and startup behavior. Our 22 disables persisted through two earlier reboots. The owner later confirmed everything was working after reboot and marked usage testing complete. This owner confirmation does not establish a measured speedup or independently measured playback/DRM resolution.

If you keep using wireless ADB, remember it is a privileged administration interface. Turn debugging off when you no longer need it.
