# 56.2 ARM vs x86 differences

Cyberdecks are built from whatever compute platform you can afford, cool, and repair.

In practice, two processor families dominate:

- **ARM**, common in single-board computers and embedded systems.
- **x86-64**, common in laptops, desktops, and small “mini personal computer” boards.

A **central processing unit (CPU)** is the processor that executes program instructions.

Both ARM and x86-64 are instruction set architectures that CPUs may implement.

This chapter is not about which is “better.”

It is about what changes in your build and maintenance workload when you pick one or the other.

The core theme is:

**x86-64 systems often follow standardized personal computer conventions.**

**ARM systems often require board-specific hardware description and vendor support.**

Both can make excellent cyberdecks.

But they fail differently.

---

## 56.2.1 Definitions (what we are comparing)

### Instruction set architecture

An **instruction set architecture** is the set of machine instructions a processor understands.

It is the boundary between software and CPU hardware.

### ARM

**ARM** is a family of instruction set architectures used widely in embedded and mobile systems.

ARM-based systems often come as single-board computers or custom boards built around a system-on-chip.

### x86-64

**x86-64** is a 64-bit extension of the x86 instruction set architecture.

It is the dominant architecture for commodity personal computers.

### System-on-chip

A **system-on-chip** (often abbreviated **SoC**) is a chip that integrates many subsystems that could otherwise be separate components.

SoCs often include not only central processing units but also graphics and peripheral controllers.

This is common in ARM platforms.

The practical consequence is that much of the hardware “layout” is specific to a particular board design.

---

## 56.2.2 The boot chain (what happens before Linux starts)

Cyberdeck builders often discover architecture differences first at boot.

### Standardized firmware on many x86-64 systems

Many x86-64 systems boot through standardized firmware paths.

The firmware typically exposes standard interfaces and tables to the operating system.

This makes it easier for a general-purpose Linux distribution to boot on “unknown” x86-64 hardware.

### More diversity on ARM

ARM boards vary widely.

Some behave like personal computers.

Many do not.

They may use bootloaders such as U-Boot, and their hardware description is frequently supplied via a device tree.

U-Boot’s device tree documentation describes using a **flattened device tree (FDT)** to control run-time configuration and emphasizes that this is the approach used by the Linux kernel on ARM and similar platforms. [S4]

### Unified Extensible Firmware Interface and the EFI boot stub

Some ARM systems use **Unified Extensible Firmware Interface (UEFI)**.

UEFI can make an ARM system feel more like a personal computer platform, because it standardizes boot mechanisms.

The Linux kernel documentation for the EFI boot stub explicitly states that on x86 and ARM platforms, the kernel can masquerade as an EFI executable so firmware loaders can load it. [S2]

**EFI** is short for *Extensible Firmware Interface*, the predecessor to UEFI.

For cyberdecks, the practical point is:

The more your platform looks like a standardized personal computer, the easier “generic” Linux installation becomes.

---

## 56.2.3 Hardware description: device tree versus ACPI

Once the kernel starts, it must discover what hardware exists.

Two mechanisms dominate.

### Device tree

A **device tree** is a structured hardware description passed to the operating system at boot.

The Linux kernel documentation describes it as a hardware description readable by the operating system so the operating system does not need to hard code machine details. [S1]

Device trees are common in ARM systems.

They are also common in embedded systems where devices on buses such as Inter-Integrated Circuit (I2C) and **Serial Peripheral Interface (SPI)** cannot be reliably auto-discovered.

Bootlin’s device tree overlay article makes this point directly: the device tree language describes hardware that cannot be automatically detected, including SoC peripherals and devices connected to buses without dynamic enumeration. [S5]

### ACPI

**Advanced Configuration and Power Interface (ACPI)** is another hardware description and configuration mechanism, widely used on personal computers.

ACPI is often associated with x86-64 platforms.

However, ACPI is also used on some arm64 platforms designed to follow personal-computer-like conventions.

The Linux kernel’s “ACPI Tables” document for arm64 lists which ACPI tables are required, recommended, optional, or not supported, illustrating that ACPI support on arm64 has specific expectations rather than being a generic “it will probably work” feature. [S3]

### What this means for cyberdeck builders

On x86-64 platforms, you more often “install Linux” and hardware mostly appears.

On many ARM platforms, you often “install an image for this board,” and then tweak hardware description or overlays when you attach new peripherals.

If your cyberdeck includes custom displays, sensors, or buttons, device tree work is more likely on ARM.

---

## 56.2.4 Driver support patterns (mainline kernels versus vendor kernels)

The Linux kernel is developed upstream.

Hardware support is strongest when it is merged into the upstream kernel.

### Vendor kernels

A **vendor kernel** is a kernel tree maintained by a hardware vendor or board project.

It often includes device-specific patches that have not been accepted upstream.

Vendor kernels are common in the ARM world.

They exist because board support is sometimes delivered faster as patches than as upstreamed code.

The cost is long-term maintenance.

If you depend on a vendor kernel, you may be tied to:

- a specific kernel version,
- a specific graphics stack,
- and a specific distribution image.

### Mainline kernels

A **mainline kernel** is the primary upstream kernel release line.

Using mainline kernels tends to improve long-term maintainability because your support comes from the broader Linux ecosystem.

Kernel.org’s release description highlights that new features are introduced in the mainline tree and that stable releases and longterm maintenance releases are managed through backports and maintained over time. [S6]

Cyberdeck implication:

If a board requires a vendor kernel to get Wi‑Fi, graphics, or suspend working, it can still be a great platform.

But you should assume more upgrade friction.

---

## 56.2.5 Graphics stacks (kernel drivers plus user-space)

Cyberdecks are increasingly graphical.

Even “text-first” builds often rely on a browser.

Graphics on Linux is not one component.

It is a stack.

At a minimum:

- a kernel driver for the graphics device,
- and user-space libraries that expose graphics application programming interfaces.

### The kernel side

The **Direct Rendering Manager (DRM)** subsystem is the kernel framework used by many Linux graphics drivers.

The kernel’s DRM internals documentation describes services the DRM layer provides, including memory management, output management, command submission, and suspend/resume support. [S7]

This matters because graphics failures are often not just “a bad driver.”

They can be an integration problem between kernel DRM support and user-space graphics libraries.

### ARM-specific friction

Many ARM SoCs have integrated graphics processors.

Some have excellent open driver support.

Some require vendor-specific binary components, firmware, or user-space libraries.

This is where “vendor kernel” and “vendor graphics stack” coupling is most painful.

### x86-64 patterns

On commodity x86-64 hardware, graphics stacks are more standardized.

Drivers for common integrated graphics devices are usually in the upstream kernel, and user-space libraries are packaged in mainstream distributions.

The net effect is not “x86 has no graphics problems.”

It is that the path to a working graphical desktop is more often paved.

---

## 56.2.6 Power and thermal implications (portable reality)

Cyberdecks are usually portable.

Portability is a power-and-heat problem.

ARM platforms are often designed for efficiency.

x86-64 platforms are often designed for peak performance and compatibility.

Both can be efficient.

But the typical cyberdeck outcomes are:

- ARM boards often have lower idle power and simpler power rails.
- x86-64 boards often offer higher performance but require more careful thermal design.

This is not a law.

It is a budgeting intuition.

Your actual result depends on:

- the SoC,
- the board power regulators,
- and your software workload.

---

## 56.2.7 Practical selection advice (what to decide up front)

The fastest way to choose between ARM and x86-64 is to decide what you want to optimize.

### Choose x86-64 when

You want:

- the broadest compatibility with general-purpose Linux distributions,
- fewer board-specific boot and hardware-description steps,
- and better odds that “it works like a normal computer.”

### Choose ARM when

You want:

- lower power budgets and small form factors,
- tight integration with embedded peripherals,
- and you are willing to work with board-specific images, device trees, and occasionally vendor kernels.

Arch Linux ARM’s “About” page is a good example of this ecosystem reality: it exists specifically as a port of a distribution to ARM architectures, and it documents that ARM support has its own platform and architecture constraints and a dedicated build infrastructure. [S8]

---

## 56.2.8 Suggested figures

1) **Boot chain diagram**: firmware/bootloader → kernel → init system, with an “x86 typical” path versus an “ARM typical” path.

2) **Hardware description diagram**: device discovery via ACPI tables versus device tree nodes.

3) **Graphics stack diagram**: kernel DRM driver → user-space graphics libraries → application.

4) **Risk matrix**: mainline support versus vendor kernel dependency, plotted against maintenance burden.

---

## 56.2.9 Practical takeaway

ARM and x86-64 can both power excellent cyberdecks.

The difference is usually not raw performance.

It is the integration surface.

x86-64 platforms tend to inherit decades of personal computer standardization.

ARM platforms tend to inherit embedded diversity: device trees, board-specific images, and vendor kernel stacks.

Pick the platform whose failure mode you can tolerate.

---

## Sources

- [S1] Linux kernel documentation: *Linux and the Devicetree (usage model)* (device tree definition; bindings context) — https://www.kernel.org/doc/html/latest/devicetree/usage-model.html
- [S2] Linux kernel documentation: *The EFI Boot Stub* (UEFI boot stub on x86 and ARM; arm64 implementation notes) — https://docs.kernel.org/admin-guide/efi-stub.html
- [S3] Linux kernel documentation: *ACPI Tables (arm64)* (arm64 ACPI expectations and required/recommended tables) — https://www.kernel.org/doc/html/latest/arch/arm64/acpi_object_usage.html
- [S4] U-Boot documentation: *Devicetree Control in U-Boot* (flattened device tree for run-time configuration across multiple boards) — https://docs.u-boot.org/en/latest/develop/devicetree/control.html
- [S5] Bootlin: *Using Device Tree Overlays, example on BeagleBone boards* (device tree necessity for non-enumerating buses; DTB passed by bootloader; overlay motivations) — https://bootlin.com/blog/using-device-tree-overlays-example-on-beaglebone-boards/
- [S6] Linux kernel archives: *Releases* (mainline/stable/longterm release model; maintenance framing) — https://www.kernel.org/category/releases.html
- [S7] Linux kernel documentation: *DRM Internals* (kernel graphics driver responsibilities and services; suspend/resume relevance) — https://www.kernel.org/doc/html/latest/gpu/drm-internals.html
- [S8] Arch Linux ARM: *About* (ARM as a distinct port with architecture constraints; build and release realities for ARM) — https://archlinuxarm.org/about

Additional secondary reference:

- Wikipedia: *Rolling release* (general definition and contrast with point releases; examples) — https://en.wikipedia.org/wiki/Rolling_release
