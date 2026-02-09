# 55.2 Kernel modules and firmware blobs

When a piece of hardware “does not work” on Linux, there are usually only a few broad reasons.

Either the operating system does not have a compatible **driver**, the driver is present but not being used, or the driver is waiting for **firmware** that is missing.

In cyberdeck builds this often appears as:

- Wi‑Fi interfaces that never show up,
- Bluetooth that cannot be enabled,
- graphics that fall back to a slow mode,
- or a USB device that exists electrically but never becomes usable.

This chapter explains two key building blocks that sit between hardware and applications:

- **kernel modules**, which are loadable pieces of kernel code (often drivers), and
- **firmware blobs**, which are device-specific binary files that some drivers must upload into hardware before it can operate.

The goal is not to turn you into a kernel developer.

The goal is to help you recognize the common patterns quickly and debug them safely.

---

## 55.2.1 Definitions (kernel, driver, module, firmware)

### Kernel

A **kernel** is the core software that manages hardware resources and provides basic services to the rest of the system.

It is the part of the operating system that can directly control hardware.

### Device driver

A **device driver** (often shortened to “driver”) is software that knows how to control a specific class of hardware.

A driver translates a general operating-system request such as “send a network packet” into the hardware-specific steps required by a particular network card.

On Linux, many drivers live inside the kernel.

### Kernel module

A **kernel module** is a piece of code that can be loaded into (or unloaded from) the running kernel.

Kernel modules extend the kernel without requiring a reboot.

The ArchWiki kernel module overview describes modules as code that can be loaded and unloaded “upon demand.” [S8]

Modules are commonly used for device drivers, but not only for drivers.

Some kernel features are built as modules for flexibility.

The `modprobe` tool is the standard user-space command that adds or removes modules from the running kernel. [S4]

### Firmware and firmware blob

**Firmware** is a small program (or data) that runs on a hardware device itself.

Many devices contain an internal processor.

They require firmware to operate.

A **firmware blob** is firmware distributed as a binary file (often without corresponding source code).

The Ubuntu Kernel/Firmware guide describes firmware as “a small piece of code that is uploaded directly to the device for it to function correctly,” and notes that it is often treated as a black box. [S10]

The ArchWiki Linux firmware page similarly describes the `linux-firmware` collection as “binary blobs” distributed alongside the kernel for functionality of certain devices, and highlights that the redistribution situation is different from the kernel’s license. [S9]

---

## 55.2.2 Why modules exist (built-in versus loadable)

A driver can be built into the kernel image, or provided as a module.

Both can work.

The trade-off is practical.

A built-in driver is always present, which can be important for boot-critical hardware.

A module can be loaded only when needed, which can reduce memory usage and makes experimentation easier.

From a cyberdeck perspective, modules are useful because you can:

- reload a driver without rebooting,
- pass driver parameters at load time,
- and temporarily disable (blacklist) conflicting drivers.

However, module loading depends on a working user-space environment.

During early boot, not all filesystems and services are available.

This is why firmware and modules sometimes need special handling during boot.

---

## 55.2.3 How Linux chooses a driver (hardware identification and binding)

Linux does not “guess.”

Hardware identifies itself.

For example:

- A device on the **Peripheral Component Interconnect (PCI)** bus reports vendor and device identifiers.
- A device on the **Universal Serial Bus (USB)** reports a vendor identifier and product identifier.

The kernel publishes device events.

User-space device management software then reacts.

A key part of that user-space layer is **udev**, which receives device events (called “uevents”) from the kernel and applies rules that may load drivers and create device nodes. [S6]

When a driver is available as a module, the system typically loads it automatically.

The ArchWiki kernel module page summarizes this modern behavior as “all necessary modules loading is handled automatically by udev.” [S8]

When this automatic binding fails, the result can look like a hardware failure.

But it is often only a missing module, a blacklisted module, or a mismatch between the running kernel and the installed modules.

---

## 55.2.4 How firmware is loaded (request_firmware and search paths)

A driver that needs firmware must obtain the firmware bytes from somewhere.

On Linux, drivers generally use the kernel firmware interface, which is an **application programming interface (API)** provided by the kernel for requesting firmware files.

The kernel documentation for `request_firmware()` describes the typical workflow: the driver requests firmware by name, receives a firmware image if available, copies it into the device, and then releases the image. [S2]

### Where the kernel looks for firmware

If firmware is loaded from the filesystem, the kernel uses specific search paths.

The Linux kernel documentation “Firmware search paths” lists the default search order, including:

- `/lib/firmware/updates/<kernel release>/`
- `/lib/firmware/updates/`
- `/lib/firmware/<kernel release>/`
- `/lib/firmware/` [S3]

This matters because “the firmware is installed” is not enough.

It must be installed in a path the kernel will search.

Ubuntu’s guide explicitly notes that firmware files are placed into `/lib/firmware`. [S10]

### Missing firmware and fallbacks

Sometimes firmware loading fails because the filesystem is not ready yet, or because firmware cannot be installed in the usual location.

The kernel documentation describes fallback mechanisms and lists real reasons direct lookup can fail, such as races at boot or resume. [S1]

For cyberdeck builders, the key lesson is simple.

A firmware load failure can be a timing and packaging issue, not only “you forgot to install a file.”

---

## 55.2.5 Built-in firmware (when firmware must be inside the kernel)

In some cases, firmware must be available before the filesystem is accessible.

Or the system designer wants to avoid an initial ram filesystem.

An **initial ram filesystem** (often called an “initramfs”) is an early boot environment used to load drivers and mount the real root filesystem.

The kernel documentation explains that firmware can be built directly into the kernel image using configuration options such as `CONFIG_EXTRA_FIRMWARE`, and gives motivations such as boot speed and boot-device access. [S5]

The same document also lists reasons built-in firmware may be undesirable or impossible, including licensing compatibility (for example, incompatibility with the **GNU General Public License (GPL)** used for the Linux kernel) and the inconvenience of rebuilding the kernel for firmware updates. [S5]

Gentoo’s handbook section on firmware installation provides a practical packaging view: firmware packages may require accepting specific licenses, and the distribution’s `linux-firmware` package covers many but not all devices. [S11]

---

## 55.2.6 Detecting missing firmware (the pattern you will see)

The most reliable way to diagnose missing firmware is to read kernel messages.

### Kernel message buffer (dmesg)

The `dmesg` command prints the kernel ring buffer, which includes driver probe messages and firmware load errors. [S7]

A typical missing firmware pattern is a message like:

- “Direct firmware load for … failed with error …”

You should not copy log excerpts from internet forums into your documentation.

But you should recognize the structure.

It tells you the firmware filename the driver asked for.

That filename is your lead.

### Using modinfo to see what firmware a module expects

The `modinfo` utility extracts metadata from kernel modules. [S12]

Many driver modules declare which firmware files they may request.

This is useful when you are building an initramfs or when you are trying to predict what package you need.

### Common root causes

Missing firmware is usually one of these:

- The firmware package is not installed.
- The firmware exists, but is in the wrong path.
- The firmware exists, but your driver expects a different filename or version.
- The driver itself is missing or incompatible with the running kernel.

---

## 55.2.7 Driver mismatch patterns (when the wrong driver loads, or none does)

Not all failures are missing firmware.

Sometimes the driver is missing, blacklisted, or incorrect.

### Module not present for this kernel

Kernel modules are tied to a specific kernel build.

If you install a new kernel but do not install the matching module set, the system may not be able to load the driver.

From the tooling side, `modprobe` relies on dependency metadata generated by `depmod`. [S4] [S13]

If dependency data is stale or mismatched, you can see confusing module load failures.

### Conflicting drivers

Some hardware has multiple drivers.

One may bind but not work correctly.

In cyberdeck contexts this happens with:

- uncommon Wi‑Fi chipsets,
- USB serial adapters,
- and low-cost USB audio devices.

The symptom is a device that “exists” but behaves incorrectly.

Blacklisting a module (preventing it from loading) can be a diagnostic step.

But you should do this carefully and reversibly.

### Firmware present, but the device is not fully functional

Some hardware will work partially without optional firmware.

For example, graphics may work but lack performance features.

Or Wi‑Fi may associate but have poor reliability.

The kernel built-in firmware documentation explicitly notes that some firmware may be optional. [S5]

---

## 55.2.8 A practical cyberdeck workflow (safe, repeatable debugging)

This workflow is designed to be repeatable and low-risk.

1) **Identify the device.** Record the bus and identifiers.

   For USB devices, record the vendor identifier and product identifier.

   For internal devices, record the bus location (for example, a PCI address).

2) **Watch kernel messages while probing.**

   Run `dmesg` before and after you attach, enable, or power-cycle the device. [S7]

   Search for lines mentioning “firmware” or the driver name.

3) **Confirm whether a module is loaded.**

   If the device should have a kernel module driver, check whether that module is present and loaded.

   The ArchWiki kernel module page lists `lsmod` and `modinfo` as basic tools for this. [S8]

4) **Inspect module metadata.**

   Use `modinfo` to see what the module claims to support and, when listed, which firmware files it expects. [S12]

5) **Install firmware packages (or the correct subset).**

   On many distributions, firmware is packaged separately from the kernel.

   Arch Linux recommends installing `linux-firmware` to obtain common firmware blobs. [S9]

   Ubuntu describes multiple sources, including packages that are not installed by default due to redistribution licensing. [S10]

6) **Reload the driver cleanly.**

   If the driver is a module, you can remove and reinsert it with `modprobe`.

   The `modprobe` manual page describes adding and removing modules and notes that module failures are often accompanied by kernel messages (visible via `dmesg`). [S4]

   If the driver is built into the kernel, you will need a reboot to re-probe.

7) **Escalate only when needed.**

   If you still cannot make progress, capture a complete `dmesg` log and the device identifiers.

   At that point, searching for the exact firmware filename is usually more productive than searching for the device’s marketing name.

---

## 55.2.9 Suggested figures

1) **Layer diagram**: Applications → kernel → (kernel module drivers) → hardware; with firmware as a file loaded into hardware by the driver.

2) **Firmware load sequence**: Driver calls request_firmware → kernel searches `/lib/firmware` paths → user-space provides file (if needed) → driver programs device. (Based on kernel request_firmware documentation and Ubuntu’s walkthrough.) [S2] [S10]

3) **Failure pattern flowchart**: “Device not working” → check kernel logs → missing firmware filename? → install firmware → reload module → verify.

4) **Boot timing diagram**: early boot before root filesystem → initramfs stage → normal system; show where “built-in firmware” can be necessary. [S5]

---

## 55.2.10 Practical takeaway

Kernel modules and firmware blobs are not “mystical Linux problems.”

They are distribution and packaging choices layered on top of clear technical mechanisms.

A driver might be present but not loaded.

Or it might be loaded but waiting for a firmware file.

Your best debugging tool is the kernel log.

Once you find the driver name and the firmware filename, the problem usually becomes tractable.

---

## Sources

- [S1] Linux kernel documentation: *Fallback mechanisms* (why firmware lookup can fail; race conditions; configuration options) — https://www.kernel.org/doc/html/latest/driver-api/firmware/fallback-mechanisms.html
- [S2] Linux kernel documentation: *request_firmware API* (typical firmware request workflow; constraints) — https://www.kernel.org/doc/html/latest/driver-api/firmware/request_firmware.html
- [S3] Linux kernel documentation: *Firmware search paths* (default `/lib/firmware` lookup order; customization) — https://www.kernel.org/doc/html/latest/driver-api/firmware/fw_search_path.html
- [S4] man7.org: *modprobe(8)* (load/unload modules; dependency file usage; failures visible in dmesg) — https://man7.org/linux/man-pages/man8/modprobe.8.html
- [S5] Linux kernel documentation: *Built-in firmware* (CONFIG_EXTRA_FIRMWARE motivations and limitations; licensing considerations) — https://www.kernel.org/doc/html/latest/driver-api/firmware/built-in-fw.html
- [S6] man7.org: *udev(7)* (device events; udev rules; kernel uevents) — https://man7.org/linux/man-pages/man7/udev.7.html
- [S7] man7.org: *dmesg(1)* (kernel ring buffer; primary source for probe and firmware errors) — https://man7.org/linux/man-pages/man1/dmesg.1.html
- [S8] ArchWiki: *Kernel module* (what modules are; tools like lsmod/modinfo; auto-loading via udev) — https://wiki.archlinux.org/title/Kernel_module
- [S9] ArchWiki: *Linux firmware* (firmware blob packaging; redistribution context; typical device classes needing firmware) — https://wiki.archlinux.org/title/Linux_firmware
- [S10] Ubuntu Wiki: *Kernel/Firmware* (clear definition of firmware vs driver; `/lib/firmware`; debugging workflow; packaging/licensing notes) — https://wiki.ubuntu.com/Kernel/Firmware
- [S11] Gentoo Handbook: *Configuring the kernel — Installing firmware and/or microcode* (firmware packaging and license acceptance; typical device classes) — https://wiki.gentoo.org/wiki/Handbook:X86/Installation/Kernel
- [S12] man7.org: *modinfo(8)* (module metadata inspection; useful for finding firmware dependencies) — https://man7.org/linux/man-pages/man8/modinfo.8.html
- [S13] man7.org: *depmod(8)* and *modules.dep(5)* (module dependency database; relationship to modprobe) — https://man7.org/linux/man-pages/man8/depmod.8.html and https://man7.org/linux/man-pages/man5/modules.dep.5.html

Additional textbook reference (print):

- Jonathan Corbet, Alessandro Rubini, Greg Kroah-Hartman: *Linux Device Drivers* (O’Reilly). (Background on drivers/modules; useful conceptual foundation even though some details evolve.)
