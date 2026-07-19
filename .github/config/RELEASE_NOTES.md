**IMPORTANT DISCLAIMER**

> [!CAUTION]
> This software is provided for testing and educational purposes only. Use at your own risk. The developers are not responsible for any damage, data loss, or issues that may occur. Please ensure you have proper backups before installation.
# Wild Kernels Fork for GKI2 Devices 5.10+ Release 6

# Features
## From Wild Kernels
- [KernelSU-Next](#kernelsu-next)
- [SUSFS v2.2.0](#susfs-v220)
- [Baseband Guard (BBG)](#baseband-guard-bbg)
- [DroidSpaces-OSS](#droidspaces-oss)
- [Networking Improvements](#networking)
- [NTSync](#ntsync)
- [Misc](#misc)
## From MomenToMoiX GKI Kernel
- [MomenToMoiX Driver](#momentomoix-driver)
- [Memory Management](#memory-management)
- [Power Management](#power-management)
- [Scheduler & I/O](#scheduler--io)

## [KernelSU-Next](https://github.com/pershoot/KernelSU-Next)

A kernel-based root solution for Android devices.

> [!WARNING]
> This release uses the [pershoot/KernelSU-Next](https://github.com/pershoot/KernelSU-Next) fork. The fork maintainer has said it is not ready for production use, so treat it as use at your own risk.

Manager: {{KSU_MANAGER}}

> [!IMPORTANT]
> For best compatiblity ensure your Manager Version and Kernel Version match eg. 30100 = 30100.

**Version**  
`33225`

**Tag**  
`v3.3.0`

**Branch**  
`dev-susfs`

**Commit**  
`032b799f2a4dbd2235fc462cadacbdfd7bffef33`

## [SUSFS v2.2.0](https://gitlab.com/simonpunk/susfs4ksu)

A KSU addon for hiding root using kernel patches and a userspace module!

Reccomended Module: [susfs4ksu-module by sidex15](https://github.com/sidex15/susfs4ksu-module)

- SUS_PATH - Hide suspicious paths
- SUS_MOUNT - Hide mount points (no CLI support)
- SUS_KSTAT - Spoof kernel statistics
- SPOOF_UNAME - Kernel version spoofing
- SPOOF_CMDLINE - Boot parameter spoofing
- OPEN_REDIRECT - File access redirection
- SUS_MAP - Memory mapping protection
- AVC_SPOOF - Spoof procfs avc denial logs

**Commit**  
`a53475a4399f5b19b656dca28b26fff208f47df4`

## [Baseband Guard (BBG)](https://github.com/vc-teahouse/Baseband-guard)

A lightweight LSM (Linux Security Module) for the Android kernel, designed to block unauthorized writes to critical partitions/device nodes at the system level.

## [DroidSpaces-OSS](https://github.com/ravindu644/Droidspaces-OSS)

A lightweight, LXC-inspired container runtime for Android and Linux. Run full Linux distributions natively with zero performance penalty.

## Networking

- BBRv1 - Improved TCP congestion control
- BBRv3 - Improved TCP congestion control (coming soon!)
- Wireguard - Built-in VPN support
- IP Set & IPv6 NAT Support - Advanced firewall capabilities
- TTL Target Support - Network packet manipulation
- CAKE, fq, fq_codel - Traffic shaping and fair queuing for reduced lag and balanced bandwidth
- connmark - Connection marking for packet classification
- TCP congestion control - CUBIC, BIC, Westwood, and HTCP for optimized performance across different network conditions

## Other Features

- TMPFS_XATTR - Extended attributes for tmpfs (Mountify support)
- TMPFS_POSIX_ACL - POSIX ACLs for tmpfs

## [NTSync](#ntsync)

Provide high-performance, low-latency synchronization primitives compatible with the Windows NT kernel API

## [Misc](#misc)

- Ptrace Leak Fix: For kernels < 5.16
- Unicode Fix: Prevent path traversal and other detections using non-printable Unicode codepoints [Experimental]
- TMPFS_XATTR: Extended attributes for tmpfs (Mountify support)
- TMPFS_POSIX_ACL: POSIX ACLs for tmpfs

## Changelog

### This Release
- Added BBRv3 (delayed)
- Added NTSync
- Ptrace Leak Fix - For kernels < 5.16
- Unicode Fix - Prevent path traversal and other detections using non-printable Unicode codepoints [Experimental]

## MomenToMoiX Driver

A smart, adaptive kernel-level optimization driver. It dynamically scales CPU behavior, frequencies, and I/O scheduling in real-time based on Screen State (ON/OFF) and Charging Status.

**How it works:**

- **Screen OFF (Battery Saver):** Isolates background apps to power-efficient cores (CPU 0), caps max frequency (with a separate bias for charging vs. non-charging states), switches the I/O scheduler to a low-overhead mode, and arms a thermal hold if the device is running hot — maximizing deep sleep and eliminating idle battery drain.
- **Screen ON (Instant):** Restores stock kernel profiles within milliseconds for a fluid, lag-free experience the moment the screen turns on. If a thermal hold is active, frequency restrictions are extended briefly until temperatures cool down.

**Requirements:**

MomenToMoiX detects screen state via `fb_notifier` (event-driven) with automatic fallback to sysfs polling. For the sysfs polling fallback to work, your device needs one of these paths:

- DPMS: `/sys/class/drm/card0-DSI-1/dpms`
- Backlight: `/sys/class/backlight/panel0-backlight/brightness`

**Verify it's running:**

```bash
su -c 'dmesg -w | grep momx'
```
after the device finishes booting.

Based on `https://github.com/LoggingNewMemory/SuiKernel-Release`'s Tenebrion logic and SELinux rules.

## Memory Management

- MGLRU (Multi-Generational LRU)
- ZRAM with ZSTD compression support
- ZSMALLOC
- KSM (Kernel Samepage Merging)
- Compaction & Migration
- Transparent Huge Pages (THP)

## Power Management

- TEO Idle Governor
- WQ_POWER_EFFICIENT
- SCHED_MC
- NO_HZ_IDLE
- Strict 100 max wakelocks limit with automated GC

## Scheduler & I/O

- Full Preemption (CONFIG_PREEMPT)
- BFQ I/O Scheduler (with Group IOSCHED)
- MQ-Deadline
- TCP FastOpen

## Other Features

- Full LTO (Link Time Optimization) builds
- Google Common Kernel LTS tracking

## Big Thanks

https://github.com/LoggingNewMemory/SuiKernel-Release — Tenebrion logic and SELinux rules

## Recommended Tools

[Kernel Flasher](https://github.com/fatalcoder524/KernelFlasher)
- Recommended flashing utility

[PixelFlasher by badabing2005](https://github.com/badabing2005/PixelFlasher)
- Pixel phone flashing GUI utility with features.

## Installation Instructions

### Prerequisites
- Unlocked bootloader.
- Backup your current boot image.
- Have root access using Magisk / KernelSU / Apatch (Any forks).

### Via Kernel Flasher
Download the correct AnyKernel3 ZIP for your device.
If you previously used another root method, clean it up first:
a. Magisk: perform a complete uninstall after flashing the AnyKernel3 ZIP.
b. KSU LKM (boot/init_boot/vendor_boot‑patched): Flash back the stock boot/init_boot/vendor_boot depending on what you patched.
c. KSU GKI: if you are 100% sure you already flashed stock init_boot/boot/vendor_boot, no action is needed; otherwise, follow the same steps as KSU LKM.
d. APatch: remove /data/adb contents to avoid leftover root conflicts after flashing the AnyKernel3 ZIP.
Flash the ZIP to the active slot using Kernel Flasher.
Install the KernelSU‑Next Manager APK, same version as mentioned in the release notes.
Open the KernelSU‑Next app.
Reboot the device if you performed any cleanup in step 2

---

Wild Kernels Fork is based on and developed from [WildKernels/GKI_KernelSU_SUSFS](https://github.com/WildKernels/GKI_KernelSU_SUSFS)  
@koneko_dev [t.me/Koneko_dev](https://t.me/Koneko_dev)

