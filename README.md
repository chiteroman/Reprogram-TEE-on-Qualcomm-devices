# Reprogram TEE on Qualcomm devices
Guide to reprogram the TEE on Qualcomm devices to fix lost attestation keys.

## Why?

If you are here it is because your phone has lost the TEE attestation keys, possibly due to flashing the persist partition of your phone.

If opening [Key Attestation Demo](https://github.com/vvb2060/KeyAttestation) does not give you any error, do **NOT** follow this guide.

This guide will **NOT** provide instructions for passing Strong verdict.

## WARNINGS:

- Your data will be lost, so backup your phone data first.
- The original keys that your phone may include in the TEE will be lost.
- I am not responsible for any other problems that may arise, these instructions are made on a POCO X3 Pro (vayu) and work perfectly, they may not work as they should on other phones.

## You will need:

- A working brain.
- A working computer.
- An unlocked bootloader.
- BASIC KNOWLEDGE OF LINUX.
- A valid keybox.xml file, you can use this one in the repo.
- The engineering ROM for your device.

## Instructions:

1. Flash engineering ROM.
2. Phone must be connected to PC, then execute this commands IN ORDER:

> adb root

> adb disable-verity

> adb reboot

> adb root

> adb remount

> adb shell mkdir -p /data/nativetest64/qti_keymaster_tests/

> adb push keybox.xml /data/nativetest64/qti_keymaster_tests/

For non StrongBox devices:

> adb shell LD_LIBRARY_PATH=/vendor/lib64/hw KmInstallKeybox /data/nativetest64/qti_keymaster_tests/{KEYBOX FILE} {KEYBOX DEVICE ID} {ATTEST PROPS?}

For StrongBox devices:

> adb shell LD_LIBRARY_PATH=/vendor/lib64/hw KmInstallKeybox /data/nativetest64/qti_keymaster_tests/{KEYBOX FILE} {KEYBOX DEVICE ID} {ATTEST PROPS?} {KEYBOX FILE} {KEYBOX DEVICE ID} {ATTEST PROPS?}

{KEYBOX FILE}: Should be "keybox.xml"

{KEYBOX DEVICE ID}: Open keybox file and search for "DeviceID", repo keybox uses "0"

{ATTEST PROPS?}: Boolean, should be true/false, I recommend setting it as true always. If it gives any error use false.

So, for the keybox in the repo you must run:

> adb shell LD_LIBRARY_PATH=/vendor/lib64/hw KmInstallKeybox /data/nativetest64/qti_keymaster_tests/keybox.xml 0 true

If your device has StrongBox:

> adb shell LD_LIBRARY_PATH=/vendor/lib64/hw KmInstallKeybox /data/nativetest64/qti_keymaster_tests/keybox.xml 0 true keybox.xml 0 true
