# E.4 Kernel/driver issues

A cyberdeck build usually combines commodity hardware modules with a general-purpose operating system. That combination is powerful, but it also means that your deck’s behavior depends on a software stack you did not write: firmware, the operating system kernel, drivers, and configuration.

When something “should work” but does not, the failure is often not mysterious. It is commonly one of four things:

1) the device is not being detected,
2) the device is detected but the wrong driver is bound,
3) the driver loads but firmware is missing,
4) the driver loads but configuration selects the wrong mode.

This chapter provides a safe, methodical troubleshooting flow organized intentionally as:

**identify device → logs → module/firmware → configuration**.

That ordering reduces guesswork and avoids destructive “try random packages” behavior.

---

## E.4.1 Definitions (zero assumed background)

A **kernel** is the core part of an operating system that manages hardware resources and provides services to other software.

A **driver** is software that allows the operating system to control a specific piece of hardware.

A **kernel module** is a driver (or other kernel component) that can be loaded into the running kernel when needed. The `modprobe` tool adds and removes modules from the kernel. [S7]

**Firmware** is device-specific software often provided as a binary file that a driver loads into hardware to make it operate correctly. The Linux kernel documentation describes the default firmware search paths and how they can be customized. [S4]

**User space** is the part of the operating system where normal applications run (as opposed to kernel code).

A **device node** is a special file (commonly in `/dev`) used by user space software to access a device through the kernel’s device interface. The Linux kernel maintains a registry of allocated device numbers for these nodes. [S5]

A **device tree** is a structured hardware description that an operating system can read so that hardware details do not have to be hard-coded into the kernel. The Linux kernel documentation describes how Linux uses device tree data. [S3]

A **Device Tree Blob** is a machine-readable form of a device tree.

A **log** is a record of events (for example, kernel messages) that can be used to infer what happened.

---

## E.4.2 Why drivers fail in cyberdecks

Cyberdecks often use:

- single-board computers with nonstandard peripherals,
- add-on boards that rely on device tree overlays,
- radio modules that need firmware,
- and operating system images that may be older than the hardware.

The key practical idea is that hardware compatibility is not only “does the connector fit?” It is also “does the operating system image include the correct driver, firmware, and configuration for this hardware?”

---

## E.4.3 Step 1: Identify the device (be precise)

Driver troubleshooting begins with a precise hardware identity.

At minimum, record:

- device name and model,
- how it connects (for example, Universal Serial Bus, a header, or a dedicated camera/display connector),
- and what it is supposed to do.

If the device is connected over Universal Serial Bus, `lsusb` can list USB buses and connected devices. [S10]

If the device is not detected at all, you have learned something important: you are not yet debugging a driver. You are still debugging detection (power, cabling, or bus enumeration).

---

## E.4.4 Step 2: Read logs (convert “it doesn’t work” into evidence)

Before installing anything, read the logs.

On Linux, `dmesg` prints messages from the kernel ring buffer. [S11]

If the system uses the systemd journal, `journalctl` prints log entries stored in the journal. [S12]

Logs can often tell you which of these situations applies:

- “device is not recognized at all,”
- “device is recognized but immediately resets,”
- “driver loads but reports missing firmware,”
- “driver loads but reports unsupported configuration.”

The important discipline is to correlate. If you unplug and replug a device, you should see a corresponding log event. If nothing appears, the operating system likely did not observe the hardware change.

---

## E.4.5 Step 3: Modules and firmware (drivers often depend on both)

### Kernel modules

If the log indicates that a driver is missing or not loading, you must determine whether the driver exists as a kernel module.

- `lsmod` shows which kernel modules are currently loaded. [S8]
- `modinfo` shows information about a kernel module, including parameters and dependencies. [S9]

The kernel documentation explains that module parameters can be specified either on the kernel command line or via `modprobe`, and it gives an example of both forms. [S2]

A common failure mode in cyberdeck builds is a mismatch between the hardware and the operating system image. The driver may exist in a newer kernel, but not in the older one you are running.

### Firmware

Some devices require firmware files that are loaded by the driver at runtime.

The Linux kernel documentation describes the firmware search paths (for example `/lib/firmware/`) and explains how to set a custom firmware search path using a module parameter or a sysfs configuration file. [S4]

If logs report missing firmware, the correct next step is to obtain the firmware from an authorized source for that device and place it in the correct path, rather than “trying random drivers.”

---

## E.4.6 Step 4: Configuration (device tree, kernel parameters, and platform settings)

Even with the correct driver and firmware, configuration can select the wrong behavior.

### Kernel command line parameters

The Linux kernel documentation provides a consolidated list of kernel command-line parameters and explains how unknown parameters are passed to the init process, and how module parameters can be provided. [S2]

Kernel parameters are powerful, but they can also make systems fragile if used casually. When you change parameters, change one thing at a time and record what you changed.

### Device tree and overlays (single-board computers)

On many single-board computers, especially those used in cyberdecks, the device tree is part of how optional hardware is enabled.

The Linux kernel documentation describes device tree as a hardware description readable by the operating system so that hardware details do not have to be hard-coded. [S3]

Raspberry Pi documentation describes a practical pipeline: firmware loads a base device tree, applies user parameters and overlays selected via `config.txt`, then passes the merged device tree blob to the kernel. [S6]

This is a common cause of “the hardware is connected but not present.” If the overlay that enables it is not selected, the kernel may never create the expected device.

---

## E.4.7 Recovery and safety (avoid making the deck less bootable)

Kernel and driver work can become self-inflicted damage. A safe workflow treats recovery as a requirement.

Practical safeguards include:

- keep a known-good boot medium,
- document the last working kernel version,
- make configuration changes incrementally,
- and avoid mixing incompatible driver sources.

If you install proprietary or out-of-tree modules, note that the kernel may mark itself as “tainted,” which can complicate debugging and reduce the usefulness of bug reports. The Linux kernel documentation explains the concept and its consequences. [S13]

For deeper driver changes, the Linux kernel documentation describes how external modules are built using the kernel build system, and notes that compatibility depends on having matching configuration and headers for the kernel. [S14]

---

## E.4.8 Suggested figures

- **Figure E.4-1: The cyberdeck software stack.** A layered diagram: hardware → firmware → kernel → drivers (modules) → device nodes → user space applications.
- **Figure E.4-2: Driver triage decision tree.** “Is the device detected?” → “Do logs show a driver binding?” → “Do logs request firmware?” → “Is configuration selecting the correct mode?”
- **Figure E.4-3: Version compatibility table.** Columns: device, required kernel version (minimum), required firmware file(s), required device tree overlay (if any), and verification command.

---

## E.4.9 Sources

[S2] The Linux Kernel documentation, *The kernel’s command-line parameters* (kernel parameters; module parameter forms). https://docs.kernel.org/admin-guide/kernel-parameters.html

[S3] The Linux Kernel documentation, *Linux and the Devicetree* (definition and usage model of device tree in Linux). https://docs.kernel.org/devicetree/usage-model.html

[S4] The Linux Kernel documentation, *Firmware search paths* (default firmware lookup locations; customization via module parameter and sysfs). https://docs.kernel.org/driver-api/firmware/fw_search_path.html

[S5] The Linux Kernel documentation, *Linux allocated devices* (device numbers and `/dev` node registry; relationship to filesystem standards). https://docs.kernel.org/admin-guide/devices.html

[S6] Raspberry Pi Documentation (GitHub), *Device Trees, overlays, and parameters* (firmware loads a base device tree, applies `config.txt` parameters and overlays, and passes the merged blob to the kernel). https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/configuration/device-tree.adoc

[S7] man7.org, *modprobe(8)* (adding and removing kernel modules). https://man7.org/linux/man-pages/man8/modprobe.8.html

[S8] man7.org, *lsmod(8)* (listing loaded kernel modules). https://man7.org/linux/man-pages/man8/lsmod.8.html

[S9] man7.org, *modinfo(8)* (module metadata, including parameters and dependencies). https://man7.org/linux/man-pages/man8/modinfo.8.html

[S10] man7.org, *lsusb(8)* (listing USB devices for identification). https://man7.org/linux/man-pages/man8/lsusb.8.html

[S11] man7.org, *dmesg(1)* (kernel ring buffer inspection). https://man7.org/linux/man-pages/man1/dmesg.1.html

[S12] man7.org, *journalctl(1)* (systemd journal log inspection). https://man7.org/linux/man-pages/man1/journalctl.1.html

[S13] The Linux Kernel documentation, *Tainted kernels* (meaning and implications of kernel taint for debugging and reporting). https://docs.kernel.org/admin-guide/tainted-kernels.html

[S14] The Linux Kernel documentation, *Building External Modules* (out-of-tree module build process and kernel header/config dependencies). https://docs.kernel.org/kbuild/modules.html

[S15] Corbet, Rubini, Kroah-Hartman, *Linux Device Drivers, Third Edition* (open textbook-style reference on how drivers relate to the kernel and hardware). https://lwn.net/Kernel/LDD3/
