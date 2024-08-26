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
- The engineering ROM for your device or Stock ROM with included `KmInstallKeybox` binary.

## Instructions for Engineering ROM:

1. Flash engineering ROM.
2. Phone must be connected to PC, then execute these commands IN ORDER:

```
adb root
```
```
adb remount
```
```
adb reboot
```
```
adb shell mkdir -p /data/nativetest64/qti_keymaster_tests/
```
```
adb push keybox.xml /data/nativetest64/qti_keymaster_tests/
```
```
adb shell LD_LIBRARY_PATH=/vendor/lib64/hw KmInstallKeybox /data/nativetest64/qti_keymaster_tests/keybox.xml 0 true
```
If you are using different keybox, you must change few arguments:
> adb shell LD_LIBRARY_PATH=/vendor/lib64/hw KmInstallKeybox /data/nativetest64/qti_keymaster_tests/{KEYBOX FILE} {DEVICE ID} {ATTEST PROPS?}

## Instructions for Stock ROM:
(with KmInstallKeybox binaries)

1. Flash Stock ROM.
2. Root the device using Magisk or any Rooting method.
3. Phone must be connected to PC, then execute these commands IN ORDER:
```
adb shell su
```
4. Grant Root Access to Shell when prompted on your phone.
```
adb shell su -c mkdir -p /data/nativetest64/qti_keymaster_tests/
```
```
adb push keybox.xml /sdcard/
```
```
adb shell su -c cp keybox.xml /data/nativetest64/qti_keymaster_tests/
```
```
adb shell su -c LD_LIBRARY_PATH=/vendor/lib64/hw KmInstallKeybox /data/nativetest64/qti_keymaster_tests/keybox.xml 0 true
```

If you are using different keybox, you must change few arguments:
> adb shell su -c LD_LIBRARY_PATH=/vendor/lib64/hw KmInstallKeybox /data/nativetest64/qti_keymaster_tests/{KEYBOX FILE} {DEVICE ID} {ATTEST PROPS?}

Attest props must be true unless it gives some error.
