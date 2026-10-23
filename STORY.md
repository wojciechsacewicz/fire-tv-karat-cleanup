# What happened, in order

On 23 October 2026 we started with a factory-reset Fire TV Stick 4K Max (2nd generation). The aim was a quiet Home screen, fewer unwanted Amazon components and easy access to Nuvio, SmartTube, Netflix, Spotify and iPhone mirroring. A replacement operating system was not needed for that goal.

We read CleanYourTV, its Fire OS device reports, fire-tv-cleanup and Kuphi's karat setup. We checked actual code and selected files rather than treating an automated workflow or matching product name as proof of compatibility.

Kuphi's system-build guard accepted incremental 0031877644288, while this device reported 0031877644420. Its boot-image fingerprint did contain 4288. That difference did not justify bypassing the guard. Kuphi's full workflow also involved unlock and partition operations beyond the app setup we needed. We did not run it end to end.

Early source-built GhostLock 1.0.3 attempts over wireless ADB and then USB caused kernel panics and rebooted the device. Neither produced root. A stock micro-USB cable supplied power but no detected data connection; a replacement data cable worked. The official 1.0.4 archive was later downloaded and checksum-verified, but was not executed in this setup thread. Offline comparison found identical embedded exploit payload bytes in the compared 1.0.3 and 1.0.4 artifacts, so a higher version alone was not evidence of a fix.

We completed the ordinary setup first. Projectivy was tried and removed at the owner's request. Aerial was selected for its appearance. Official beta Nuvio and SmartTube builds replaced the earlier choices. Twenty-two selected packages were disabled, unwanted tiles were hidden, privacy/autoplay settings were changed, and resolution stayed Auto. Normal reboot checks confirmed the initial setup persisted. The owner confirmed iPhone AirPlay mirroring worked.

A separate investigation subsequently obtained temporary root using a diagnostic source build. This setup work independently verified working superuser access and then used that existing access. The separate investigation reported a stale embedded diagnostic payload behind some of its own probe failures. That does not establish the cause of our earlier clean-build panics. The successful session also had kernel credential metadata side effects, so it must not be described as harmless or reliably reproducible.

Root let us disable the Amazon Home entry points while leaving its native Settings implementation available, choose Aerial as actual Home and remove the accessibility workaround. A tiny Settings bridge from Kuphi restored Aerial's Settings button. We inspected the exact bridge APK before installation. Four dock apps, return to Home, native Settings, a Settings submenu and AirPlay responsiveness were checked in that session.

The remote's native app picker did not list Nuvio. We found that its two custom shortcuts were stored as serialized intents and assigned direct app launches instead. Button 1 launched Nuvio; button 2 launched Spotify. Tests used the paired remote's input mapping, because generic injected button codes misleadingly opened a partner app. No persistent button-remapping app was installed.

We stopped with temporary root still live. There was no bootloader unlock, firmware replacement or partition flash. The final root-dependent Home configuration and shortcut persistence still need a reboot test, and the system needs ordinary viewing over several days before we call the package list fully exercised.
