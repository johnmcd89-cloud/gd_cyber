# B.4 OS and driver docs

Most cyberdeck failures are not “hardware” or “software.”

They are boundary failures: a driver is missing, a device tree description is wrong, permissions prevent access, or a cable presents as an intermittent bus.

This chapter provides a map of documentation sources that matter for cyberdeck bring-up and troubleshooting, with emphasis on:

- distribution (distro) documentation,
- Linux kernel device tree documentation,
- udev (userspace device management),
- and a practical troubleshooting resource map.

---

## B.4.1 Vocabulary: distribution, kernel, driver, device tree, udev

A **distribution** (often shortened to “distro”) is a packaged operating system built around the Linux kernel with a specific set of tools, defaults, and update policies.

A **kernel** is the core of an operating system that manages hardware, memory, and process execution.

A **driver** is software that allows the kernel to control a hardware device.

A **device tree** is a data structure describing hardware so that the kernel can manage those components. The devicetree is passed to the operating system at boot time and describes hardware without hard-coding every detail into the kernel. [S6] [S7]

**udev** is a device manager for the Linux kernel that supplies device events, manages permissions of device nodes, and may create additional device symlinks or rename interfaces. [S8] udev also provides a rules system to create stable device identifiers despite nondeterministic enumeration order. [S9]

---

## B.4.2 The documentation layers that matter

When you are debugging, it helps to know which layer owns which behavior.

A practical layering model is:

1) vendor firmware and bootloader configuration,
2) Linux kernel drivers,
3) device tree descriptions (common on single-board computers),
4) udev rules and permissions,
5) distribution policies and user-space tooling.

The same symptom can originate in different layers.

For example, “device not found” may mean:

- the kernel driver is not loaded,
- the device tree does not describe the device,
- or udev permissions prevent access.

### Suggested figure

**Figure B.4-A: Driver stack diagram.**

A layered diagram showing firmware/bootloader → kernel driver → device tree → udev → user-space applications.

---

## B.4.3 Distro documentation: where to start

Distribution documentation is where you learn:

- supported platforms,
- package names,
- policy defaults,
- and recommended troubleshooting workflows.

### Debian

Debian publishes a documentation hub linking to installation guides, a frequently asked questions document, and a “Debian Reference” aimed at shell-level usage. [S1]

The Debian Wiki is a practical complement, with pages on specific hardware and workflows. [S2]

### Ubuntu

Ubuntu publishes official desktop and server documentation and provides it by release version. This is useful when you need “what does this release expect?” rather than a generic answer. [S3]

### Armbian (SBC-focused)

Armbian’s documentation is explicitly focused on single-board computers and includes getting started and advanced configuration guidance. For cyberdecks built around non-Raspberry Pi boards, Armbian documentation is often the fastest route to platform-specific boot and device tree details. [S4]

### Arch Linux and the ArchWiki

For troubleshooting, the ArchWiki is often useful even if you do not run Arch, because it tends to be explicit about mechanisms and commands.

For example, the ArchWiki’s udev page explains the role of udev, notes that module loading order is not preserved across boots, and recommends relying on persistent identifiers. [S9]

---

## B.4.4 Kernel device tree references

Device tree is common on embedded Linux and many single-board computers.

The Devicetree Project describes devicetree as a hardware description data structure passed to the operating system at boot time and points to the devicetree specification releases. [S7]

The Linux kernel documentation includes a device tree section for Open Firmware and devicetree concepts and related documentation. [S10]

Practical advice: when a peripheral “does not exist,” confirm whether it is described in the device tree for your board, and whether overlays or configuration are involved.

---

## B.4.5 udev references (rules and diagnostics)

udev is a frequent source of confusion because it is an invisible policy layer.

The udev manual page describes its role: receiving kernel device events, matching rules, creating device node permissions, and creating meaningful symlinks in /dev. [S11]

The udevadm tool provides a set of subcommands for querying the udev database, triggering events, and monitoring device events, and is the primary diagnostic interface. [S12]

For conceptual understanding and practical examples, the ArchWiki udev page is a good reference and emphasizes persistent naming and stable identifiers. [S9]

---

## B.4.6 Troubleshooting resource map (what to read first)

The goal of troubleshooting is to gather evidence before changing the system.

A practical “first 10 minutes” resource map is:

### Kernel message buffer

The dmesg tool prints (or controls) the kernel ring buffer. It is used to examine kernel messages, including those produced by device drivers during discovery and error conditions. [S13] [S14]

### System logs

If your system uses systemd, journalctl prints log entries stored in the systemd journal and supports filtering by fields such as service unit name. [S15]

### Hardware enumeration

For Universal Serial Bus (USB) devices:

- lsusb lists USB devices and can show detailed descriptors, using udev’s database to associate human-readable names to vendor and product identifiers. [S16]

For Peripheral Component Interconnect (PCI) devices:

- lspci lists PCI devices and provides guidance on capturing verbose output for driver issues. [S17]

### Module state

When troubleshooting missing drivers, confirm module state.

- lsmod shows currently loaded modules. [S18]
- modprobe loads and unloads kernel modules and describes how it finds modules by kernel version. [S19]
- depmod generates module dependency maps. [S20]

### Suggested figure

**Figure B.4-B: Evidence collection checklist.**

A checklist with “symptom,” “dmesg excerpt,” “journalctl excerpt,” “lsusb/lspci output,” “udevadm info output,” and “what changed last.”

---

## B.4.7 Sources

[S1] Debian, *Documentation* (hub of installation guide, FAQ, Debian Reference, and wiki). https://www.debian.org/doc/

[S2] Debian Wiki, *FrontPage* (entry point to Debian’s community-maintained documentation). https://wiki.debian.org/

[S3] Ubuntu Documentation, *help.ubuntu.com* (official documentation by release; desktop and server). https://help.ubuntu.com/

[S4] Armbian Documentation, *Introduction* (SBC-focused distro documentation entry point). https://docs.armbian.com/

[S5] Wikipedia, *systemd* (context for journalctl and userspace integration). https://en.wikipedia.org/wiki/Systemd

[S6] Wikipedia, *Devicetree* (device tree definition and usage context). https://en.wikipedia.org/wiki/Device_tree

[S7] Devicetree.org, *The Devicetree Project* (specification context and description). https://www.devicetree.org/

[S8] Wikipedia, *udev* (device manager definition and context). https://en.wikipedia.org/wiki/Udev

[S9] ArchWiki, *udev* (conceptual explanation; persistent identifiers; rules overview). https://wiki.archlinux.org/title/Udev

[S10] Linux kernel documentation, *Open Firmware and Devicetree* (kernel devicetree documentation entry point). https://www.kernel.org/doc/html/latest/devicetree/index.html

[S11] man7.org, *udev(7)* (udev purpose: device events, permissions, and symlinks; rules behavior). https://man7.org/linux/man-pages/man7/udev.7.html

[S12] man7.org, *udevadm(8)* (udev management and debugging tool). https://man7.org/linux/man-pages/man8/udevadm.8.html

[S13] man7.org, *dmesg(1)* (kernel ring buffer tool). https://man7.org/linux/man-pages/man1/dmesg.1.html

[S14] Wikipedia, *dmesg* (kernel message buffer concept). https://en.wikipedia.org/wiki/Dmesg

[S15] man7.org, *journalctl(1)* (systemd journal query tool). https://man7.org/linux/man-pages/man1/journalctl.1.html

[S16] man7.org, *lsusb(8)* (USB enumeration and descriptor display). https://man7.org/linux/man-pages/man8/lsusb.8.html

[S17] man7.org, *lspci(8)* (PCI enumeration; recommended verbose outputs). https://man7.org/linux/man-pages/man8/lspci.8.html

[S18] man7.org, *lsmod(8)* (module listing via /proc/modules). https://man7.org/linux/man-pages/man8/lsmod.8.html

[S19] man7.org, *modprobe(8)* (module load/unload behavior). https://man7.org/linux/man-pages/man8/modprobe.8.html

[S20] man7.org, *depmod(8)* (dependency map generation). https://man7.org/linux/man-pages/man8/depmod.8.html
