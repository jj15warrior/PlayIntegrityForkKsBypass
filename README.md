# Play Integrity Fork KeyStore Bypass
This fork-fork aim is to bypass the AndroidKeyStore by implementing a similar process software side.
The current implementation provides DEVICE through the classic PIF, while having a test environment that runs the KeyStore bypass in the KeyAttestation app.
At the moment this repository contains:
- Import and parsing of keybox.xml from module folder
- Keypair and its certificate attestation generation through a simplified software implementation of AndroidKeyStore
- Certificate chain retrieval

The whole implementation provides a valid attestation to the [KeyAttestation app](https://github.com/vvb2060/KeyAttestation).

*This integration is inspired by chiteroman's BootloaderSpoofer and FrameworkPatch. Parts of the certificate parsing and attestation generation are based on his work*

## Lack of time to work on the module
Due to lack of time I am unable to keep working on the implementation. I will provide some open points to expand on the work I did.

### Open points
- In order to better grasp the droidguard key requirements, an analysis of the `AlgorithmParameterSpec` provided while initializing the `CustomkeyPairGenerator` should suffice in order to understand the attestation requirements and implement the relative, half implemented and commented, spec handlers.
- Testing could be done with a keybox.xml with associated device parameters, even if the keybox has been invalidated.
- This implementation **has not been tested in droidguard yet**. This means that droidguard could implement checks that validate the KeyPairGenerator, as well as the KeyStoreSpi, through, for example, checking their class name. As well as other checks that could detect any of the current injection methods.
- Remember that each test you do with a valid keybox in droidguard could be detected and result in its future invalidation. I never had the pleasure of testing droidguard with a valid keybox (even if i had multiple prior to the last ban wave) due to not having time to check out the whole `AlgorithmParameterSpec` requested by droidguard, I did only test the current implemented features with key attestation app.

## Thoughs on the future
As far as I can tell, even with a bypass working, without a reliable way to retrieve the key from the device you own (prior to RKP), this approach is and will be unsustainable in the long run, due to possible improvements in droidguard detection. 
The already existing remote key provisioning system also limits the usage of provisioned keys to a limited time. Expanding on that, emulating the provisioning system will also required reliable extraction from the device TEE that, will be certainly done but, will never be reliable enough for a wide spread use.

The key point being that google has finally found a way to kick the modding community, custom rom/unlocked bootloader users, out of its ecosystem in a consistent way.

## A message to the walled garden keepers
While I do appreciate your dedication and your ways of providing the community with working tools, the way you act shows you more as "entitled" than "helpful". That is at least in my non-existing experience because, even trying, I was unable to have a private conversation with any of you in any way. This could, and probably already is, blocking out useful contributions making this look less like a community effort and more like a personal achievement.


# Original Play Integrity Fork README
*PIF forked to be more futureproof and develop more methodically*

[![GitHub release (latest by date)](https://img.shields.io/github/v/release/osm0sis/PlayIntegrityFork?label=Release&color=blue&style=flat)](https://github.com/osm0sis/PlayIntegrityFork/releases/latest)
[![GitHub Release Date](https://img.shields.io/github/release-date/osm0sis/PlayIntegrityFork?label=Release%20Date&color=brightgreen&style=flat)](https://github.com/osm0sis/PlayIntegrityFork/releases)
[![GitHub Releases](https://img.shields.io/github/downloads/osm0sis/PlayIntegrityFork/latest/total?label=Downloads%20%28Latest%20Release%29&color=blue&style=flat)](https://github.com/osm0sis/PlayIntegrityFork/releases/latest)
[![GitHub All Releases](https://img.shields.io/github/downloads/osm0sis/PlayIntegrityFork/total?label=Total%20Downloads%20%28All%20Releases%29&color=brightgreen&style=flat)](https://github.com/osm0sis/PlayIntegrityFork/releases)

A Zygisk module which fixes "ctsProfileMatch" (SafetyNet) and "MEETS_DEVICE_INTEGRITY" (Play Integrity).

To use this module you must have one of the following (latest versions):

- [Magisk](https://github.com/topjohnwu/Magisk) with Zygisk enabled (and Enforce DenyList enabled if NOT also using [Shamiko](https://github.com/LSPosed/LSPosed.github.io/releases) or [Zygisk Assistant](https://github.com/snake-4/Zygisk-Assistant), for best results)
- [KernelSU](https://github.com/tiann/KernelSU) with [Zygisk Next](https://github.com/Dr-TSNG/ZygiskNext) module installed
- [APatch](https://github.com/bmax121/APatch) with [Zygisk Next](https://github.com/Dr-TSNG/ZygiskNext) module installed

## About module

It injects a classes.dex file to modify fields in the android.os.Build class. Also, it creates a hook in the native code to modify system properties. These are spoofed only to Google Play Services' DroidGuard (SafetyNet/Play Integrity) service.

The purpose of the module is to avoid hardware attestation.

## About 'custom.pif.json' file

You can fill out the included template [example.pif.json](https://raw.githubusercontent.com/osm0sis/PlayIntegrityFork/main/module/example.pif.json) from the module directory (/data/adb/modules/playintegrityfix) then rename it to custom.pif.json to spoof custom values to the GMS unstable process. It will be used instead of any included pif.json (none included currently).

Note this is just a template with the current suggested defaults, but with this fork you can include as few or as many android.os.Build class fields and Android system properties as needed to pass DEVICE verdict now and in the future if the enforced checks by Play Integrity change.

As a general rule you can't use values from recent devices due to them only being allowed with full hardware backed attestation. A script to extract the latest Xiaomi.eu public fingerprint is included with the module; see the autopif section below for usage and caveats, and see the Resources below for information and scripts to help find a working private fingerprint.

Older formatted custom.pif.json files from cross-forks and previous releases will be automatically migrated to the latest format. Simply ensure the filename is custom.pif.json and place it in the module directory before upgrading.

A migration may also be performed manually with `sh migrate.sh` and custom.pif.json in the same directory, or from a file explorer app that supports script execution.

<details>
<summary><strong>Resources</strong></summary>

- FAQ:
  - [PIF FAQ](https://xdaforums.com/t/pif-faq.4653307/) - Frequently Asked Questions (READ FIRST!)

- Guides:
  - [How-To Guide](https://xdaforums.com/t/module-play-integrity-fix-safetynet-fix.4607985/post-89189572) - Info to help find build.prop files, then manually create and use a custom.pif.json
  - [Complete Noobs' Guide](https://xdaforums.com/t/how-to-search-find-your-own-fingerprints-noob-friendly-a-comprehensive-guide-w-tips-discussion-for-complete-noobs-from-one.4645816/) - A more in-depth basic explainer of the How-To Guide above
  - [UI Workflow Guide](https://xdaforums.com/t/pixelflasher-a-gui-tool-for-flashing-updating-rooting-managing-pixel-phones.4415453/post-87412305) - Build/find, edit, and test custom.pif.json using PixelFlasher on PC
  - [Tasker PIF Testing Helper](https://xdaforums.com/t/pif-testing-helper-tasker-profile-for-testing-fingerprints.4644827/) - Test custom.pif.json using Tasker on device

- Scripts:
  - [gen_pif_custom.sh](https://xdaforums.com/t/tools-zips-scripts-osm0sis-odds-and-ends-multiple-devices-platforms.2239421/post-89173470) - Script to generate a custom.pif.json from device dump build.prop files
  - [pif-test-json-file.sh](https://xdaforums.com/t/module-play-integrity-fix-safetynet-fix.4607985/post-89561228) - Script to automate generating and testing json files to attempt to find working fingerprints 
  - [install-random-fp.sh](https://xdaforums.com/t/script-for-randomly-installing-custom-device-fingerprints.4647408/) - Script to randomly switch between multiple working fingerprints found by the user

</details>

## About 'custom.app_replace.list' file

You can customize the included default [example.app_replace.list](https://raw.githubusercontent.com/osm0sis/PlayIntegrityFork/main/module/example.app_replace.list) from the module directory (/data/adb/modules/playintegrityfix) then rename it to custom.app_replace.list to systemlessly replace any additional conflicting custom ROM spoof injection app paths to disable them.

## About 'autopif.sh' and 'killgms.sh' script files

There's intentionally no pif.json in the module because the goal remains to be futureproof, and including something that may be banned and obsolete within days would be contrary to that goal. If you don't care to have your own private fingerprint to use or don't have time to look for one currently then simply run the extraction script from a root prompt with `sh autopif.sh` in the module directory (/data/adb/modules/playintegrityfix), or from a file explorer app that supports script execution.

The autopif script extracts the latest working Xiaomi.eu fingerprint (though frequently banned and may be banned for RCS use while otherwise passing Play Integrity and SafetyNet) ideally to test an initial setup.

The killgms script forces the Google Play Services DroidGuard process (com.google.android.gms.unstable) to end, making it restart with the next attestation attempt; useful for testing out different private fingerprints without requiring a reboot in between.

## Troubleshooting

Make sure Google Play Services (com.google.android.gms) is NOT on the Magisk DenyList if Enforce DenyList is enabled since this interferes with the module; the module does prevent this using scripts but it only happens once during each reboot.

### Failing BASIC verdict

If you are failing basicIntegrity (SafetyNet) or MEETS_BASIC_INTEGRITY (Play Integrity) something is wrong in your setup. Recommended steps in order to find the problem:

- Disable all modules except this one
- Try a different (ideally known working) custom.pif.json

Note: Some modules which modify system (e.g. Xposed) can trigger DroidGuard detections, as can any which hook GMS processes (e.g. custom fonts).

### Failing DEVICE verdict (on KernelSU/APatch)

- Disable Zygisk Next
- Reboot
- Enable Zygisk Next
- Reboot again

### Failing DEVICE verdict (on custom kernel/ROM)

- Check the kernel release string with command `adb shell uname -r` or `uname -r`
- If it's on the [Known Banned Kernel List](https://xdaforums.com/t/module-play-integrity-fix-safetynet-fix.4607985/post-89308909) then inform your kernel developer/ROM maintainer to remove their branding for their next build
- You may also try a different custom kernel, or go back to the default kernel for your ROM, if available/possible

### Failing DEVICE verdict (on custom ROM)

- Check the ROM signing keys with command `adb shell unzip -l /system/etc/security/otacerts.zip` or `unzip -l /system/etc/security/otacerts.zip`
- If the output shows the ROM is signed with the AOSP testkey then inform your ROM maintainer to start signing their builds with a private key for their next build and ideally also provide a ROM signature migration build to allow you to update to it without requiring a data wipe
- You may also try a different custom ROM, or go back to the stock ROM for your device, if available/possible

### Failing Play Protect/Store Certification and/or Google Wallet Tap To Pay Setup Security Requirements

- Reflash the module in your root manager app
- Clear Google Wallet (com.google.android.apps.walletnfcrel) and/or Google Pay (com.google.android.apps.nbu.paisa.user) cache, if you have them installed
- Clear Google Play Store (com.android.vending) and, if present, Google Play Protect Service (com.google.android.odad) cache and data
- Clear Google Play Services (com.google.android.gms) cache and data, or, optionally skip clearing data and wait some time (~24h) for it to resolve on its own
- Reboot

Note: Clearing Google Play Services app ***data*** will then require you to reset any WearOS devices paired to your device.

### Read module logs

You can read module logs using one of these commands directly after boot:

`adb shell "logcat | grep 'PIF/'"` or `su -c "logcat | grep 'PIF/'"`

Add a "verboseLogs" entry with a value of "0", "1", "2", "3" or "100" to your custom.pif.json to enable higher logging levels; "100" will dump all Build fields, and all the system properties that DroidGuard is checking. Adding the entry can also be done using the migration script with the `sh migrate.sh --force --advanced` or `sh migrate.sh -f -a` command.

## Can this module pass MEETS_STRONG_INTEGRITY?

No.

## About Scripts-only mode

An advanced feature intended for older Android <10 ROMs, mostly stock ROMs or those with stock-like values, (and some other rare special cases), since they generally only need a few prop changes to pass Play Integrity DEVICE verdict. Due to this the majority of the previous information does not apply to or contradicts that of Scripts-only mode, so to avoid confusion it's contained in the Details area below.

<details>
<summary><strong>Details</strong></summary>

- Manually opt-in by creating a file named scripts-only-mode in the module directory, either from a root prompt with `mkdir -p /data/adb/modules/playintegrityfix; touch /data/adb/modules/playintegrityfix/scripts-only-mode` or from a file explorer app, and then re/flashing the module. Scripts-only mode will remain enabled until this file is removed and the module is reflashed again.

- During install all unused default mode files (including custom.pif.json) are removed from the module directory, effectively disabling the Zygisk components of PIF: attestation fallback and device spoofing. You'll see "Scripts-only mode" indicated in the module description in your root manager app.

- For best results, you should still most likely enable Magisk's Enforce DenyList option if NOT also using [Shamiko](https://github.com/LSPosed/LSPosed.github.io/releases) or [Zygisk Assistant](https://github.com/snake-4/Zygisk-Assistant). The module will automatically add the Google Play Services DroidGuard process (com.google.android.gms.unstable) to the Magisk DenyList, if missing, since for Scripts-only mode it's necessary on some configurations (generally Android 9).

</details>

## About Play Integrity (SafetyNet is deprecated)

[Play Integrity API](https://xdaforums.com/t/info-play-integrity-api-replacement-for-safetynet.4479337/) - FAQ/information about PI (Play Integrity) replacing SN (SafetyNet)

## Credits

Module scripts were adapted from those of kdrag0n/Displax's Universal SafetyNet Fix (USNF) module, please see the commit history of [Displax's USNF Fork](https://github.com/Displax/safetynet-fix/tree/dev/magisk) for proper attribution.
