# Temporary-root guide for the tested karat build

Updated 23 October 2026. This is the source-build route behind our successful session, with a small logging patch and a byte-for-byte rebuild check. It does not unlock the bootloader, install Magisk or flash firmware. Root ends at reboot.

## Scope and consequences

Our unit was AFTKRT / karat, Fire OS 8.1.7.4, RS8174.3648N, kernel 5.10.43-android12-9-ge1cb3cb5ad98. The system incremental was 0031877644420; the boot-image fingerprint contained 0031877644288. Upstream selected its 4288 profile. This observation does not authorize guessing offsets or bypassing another project's exact-build checks.

Earlier attempts panicked and rebooted this unit. One later diagnostic build succeeded. The successful session also modified shared kernel credential metadata, left SELinux permissive and mounted a temporary su overlay. No immediate failure was observed afterward, but that does not make the kernel side effects harmless.

The payload automatically disables OTA packages and clears updater app data. Those stored changes remain after reboot even though root goes away. Re-enabling a protected package may require root again, and it cannot restore cleared app data. A factory reset is not a repair method for damaged verified partitions. We did not create a full recovery image or verify a complete unbrick route.

## 1. Get the tools and source

This build was reproduced on Linux x86-64 with GNU Make and Android NDK r29. See [DOWNLOADS.md](DOWNLOADS.md) for publisher links. Install Git, Make, unzip and Android Platform-Tools. Obtain Google's NDK r29, extract it and record its actual directory. Do not substitute the current NDK by default when trying to reproduce these bytes.

Clone this guide repository and fetch the pinned upstream source in a separate directory:

```sh
git clone https://github.com/wojciechsacewicz/fire-tv-karat-cleanup.git
git clone --branch 5.10 https://github.com/R0rt1z2/GhostLock.git GhostLock-karat
cd GhostLock-karat
git checkout --detach 2752d2e5aef40c5007f23f36ce89ee29e1a6a4bc
git apply --check ../fire-tv-karat-cleanup/root-diagnostic-logging.patch
git apply ../fire-tv-karat-cleanup/root-diagnostic-logging.patch
```

The included patch adds per-stamp diagnostics only. It excludes the experimental sentinel and read-only probe branches from the private investigation. It applies to this exact upstream revision.

## 2. Build the karat target

Use a fresh checkout. Set NDK_ROOT to your extracted r29 directory:

```sh
export NDK_ROOT="$HOME/android-ndk-r29"
make PROJECT=karat NDK_ROOT="$NDK_ROOT" \
  PROBE_CFLAGS='-DROOT_CHAIN -DSTAMP_DIAG_PROBE -DSTAMP_DIAG_FULL'
sha256sum build/karat/bin/preload
```

Expected SHA-256 of the successful binary:

```text
41a67de4c02fa8c223861f46035d59ffc02940d69deec7f862e56de769e30308
```

We reproduced this exact hash using both the full private diagnostic source and the public logging-only patch, in isolated offline build environments. The public source build was not run on the TV again; its bytes match the previously successful artifact.

Keep the flags explicit. The private diagnostic source had a branch that entered a probe-only loop unless STAMP_DIAG_FULL was defined; the original handoff command omitted it. This public patch removes that branch, but the documented flags preserve the verified build configuration.

Changing compiler defines does not reliably invalidate all outputs in this Makefile. For a rebuild with changed flags, remove the generated `build/karat` directory in this checkout or use another fresh checkout. Do not reuse unknown embedded diagnostic artifacts. A stale diagnostic payload explained some later probe failures, not necessarily our initial clean-build panics.

## 3. Establish USB ADB and verify identity

Keep HDMI connected for visibility and use a data-capable micro-USB cable with stable power. The stock cable in our case powered the Stick but did not expose USB data. Enable ADB debugging and accept the computer authorization prompt on the TV.

Choose the actual USB serial from `adb devices -l`. Replace the placeholder below. Port 5037 is normal; if another ADB server owns the connection, use that server's port. Our laptop had two shared servers, so blindly restarting them would interrupt another device.

```sh
adb devices -l
export FIRETV_SERIAL='YOUR_USB_SERIAL'
export FIRETV_ADB_PORT=5037
adb -P "$FIRETV_ADB_PORT" -s "$FIRETV_SERIAL" shell getprop ro.product.model
adb -P "$FIRETV_ADB_PORT" -s "$FIRETV_SERIAL" shell getprop ro.product.device
adb -P "$FIRETV_ADB_PORT" -s "$FIRETV_SERIAL" shell getprop ro.build.version.incremental
adb -P "$FIRETV_ADB_PORT" -s "$FIRETV_SERIAL" shell getprop ro.bootimage.build.fingerprint
adb -P "$FIRETV_ADB_PORT" -s "$FIRETV_SERIAL" shell uname -r
```

Compare the results with the scope above. Stop on an unexpected model, kernel or build. A product listing saying '4K Max' does not establish the generation or firmware.

Keep your package baseline and any existing shortcut preferences private. Confirm that you can restore the TV's user-facing setup independently of this guide. Do not start by disabling stock Home; establish and verify root first.

## 4. Run once, from a fresh boot

The following is the manual payload invocation used by the local investigation, made generic. It is not a new successful hardware test. Do not run it on an already rooted session you want to preserve.

Reboot and wait until the TV has visibly finished booting. Recheck the USB transport and ADB port after reboot. Do not assume a successful boot-completion property means the UI and network are ready.

```sh
adb -P "$FIRETV_ADB_PORT" -s "$FIRETV_SERIAL" reboot
# After the TV finishes booting, recheck adb devices and the selected port.
adb -P "$FIRETV_ADB_PORT" -s "$FIRETV_SERIAL" push \
  build/karat/bin/preload /data/local/tmp/preload
adb -P "$FIRETV_ADB_PORT" -s "$FIRETV_SERIAL" shell \
  'chmod 755 /data/local/tmp/preload; rm -f /data/local/tmp/ghostlock.proof /data/local/tmp/ghostlock.log'
adb -P "$FIRETV_ADB_PORT" -s "$FIRETV_SERIAL" shell \
  'nohup /data/local/tmp/preload > /data/local/tmp/ghostlock.log 2>&1 &'
```

The leak and exploit can take minutes. Check the local log occasionally rather than launching concurrent copies:

```sh
adb -P "$FIRETV_ADB_PORT" -s "$FIRETV_SERIAL" shell \
  'tail -25 /data/local/tmp/ghostlock.log'
```

If the TV goes offline or reboots, that is a failed attempt, not proof of root. Stop and inspect the failure before another attempt. Do not publish the raw log; it includes live kernel addresses and may include device data.

This manual sequence does not turn Wi-Fi off. The pinned upstream wrapper does disable Wi-Fi for USB attempts and may leave it disabled. The wrapper also defaults to a sunstone binary when run from a source checkout and can retry eight times. Do not use it unmodified or assume those defaults describe this karat session.

## 5. Verify real superuser access

A proof marker alone is insufficient because the payload writes it before all setup finishes. Verify working su and read its context:

```sh
adb -P "$FIRETV_ADB_PORT" -s "$FIRETV_SERIAL" shell \
  'cat /data/local/tmp/ghostlock.proof; su -c id; getenforce'
```

Our proof reported ROOTED with uid/euid 0, `su -c id` independently returned uid=0(root), and SELinux was Permissive. Check that native Settings, Home and your apps still work. Do not kill the preload children or root daemon while you need that session. The root shell listeners are on loopback ports 9999 and 9060; no port-forwarding is required for the su verification above.

Then use [EXISTING-ROOT.md](EXISTING-ROOT.md) for the configuration changes root enabled. We did not automate them into this root procedure. At the time of publication their final reboot persistence remained untested.

## Alternative archive, not our successful artifact

The [official XDA karat 1.0.4 archive](https://xdaforums.com/attachments/ghostlock-karat-v1-0-4-zip.6386789/) was downloaded and verified during the earlier investigation. Its archive SHA-256 was `b1df5a99c9ca9266be90a02eee62ba717f2cdc8850adf8b7a14fadf091c6532e`. It was not the binary used for our successful session, and we did not execute it. We do not claim it fixes the earlier panics.

## Distribution

The upstream GhostLock repository exposed no explicit license in our review. We link the source and official downloads instead of mirroring its code tree or publishing a compiled exploit release. The small local logging patch is included for reproducibility. Google tools likewise come from Google rather than this repository. No private reroot script, device identifier or raw root log is included.
