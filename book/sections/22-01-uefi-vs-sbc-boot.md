# 22.1 UEFI vs SBC boot

Cyberdecks are often built on two very different kinds of computing platforms.

One path is a personal-computer-like platform (for example, an x86 mini personal computer).

The other path is a single-board computer (SBC).

An **SBC** (single-board computer) is a computer where the processor, memory, and input/output hardware are integrated on one board.

These two worlds tend to boot differently.

Those differences matter because boot behavior determines:

- what you can configure without re-flashing firmware,
- how portable your storage is between machines,
- and what recovery options exist when something breaks.

This chapter compares **UEFI** (Unified Extensible Firmware Interface) style booting to common SBC boot flows.

The emphasis is practical: configuration knobs and recovery paths.

---

## 22.1.1 Definitions: firmware, bootloader, and boot chain

A computer starts in a very constrained state.

The processor begins executing instructions from a small piece of code stored in non-volatile memory.

That earliest code is part of the system’s **firmware**.

**Firmware** is software stored in persistent storage (for example, flash memory) that runs before the operating system.

A **bootloader** is a program that loads the operating system.

A **boot chain** (or boot sequence) is the sequence of programs that run from power-on to a running operating system.

Boot chains are often multi-stage.

That is not just complexity for its own sake.

Each stage has different constraints and different jobs:

- establishing memory and clocks,
- discovering storage devices,
- choosing what to boot,
- and handing control to the operating system.

---

## 22.1.2 UEFI: a standardized pre-operating-system environment

UEFI is a firmware interface specification.

It defines a standardized environment that exists before the operating system, including a boot manager, driver model, and configuration interfaces.

### What you can configure in UEFI

UEFI platforms are typically configured through standardized firmware variables stored in **non-volatile random access memory (NVRAM)**.

The UEFI boot manager specification describes a model built around:

- a set of boot options (`Boot####` entries),
- an ordered list that controls which boot option is attempted first (`BootOrder`),
- and a one-time override mechanism (`BootNext`). [B1]

The result is that many UEFI systems support user-facing configuration such as:

- boot device priority,
- one-time “boot from this device,”
- and predictable fallback behavior.

UEFI also standardizes Secure Boot concepts.

The Secure Boot section of the UEFI specification describes how trust policy is represented via key databases (for example, platform key and allowed/forbidden signature databases) and how modes influence enforcement behavior. [B2]

This matters to cyberdecks because Secure Boot configuration is not “a separate feature.”

It is part of the boot configuration surface.

### What recovery looks like in UEFI

UEFI does not guarantee that every platform has a friendly recovery menu.

But the specification does define recovery-oriented behavior.

For example, the boot manager model includes behavior for attempting alternative options when normal boot choices are exhausted, and it defines a mechanism by which the operating system can request platform-defined recovery behavior. [B1]

From a builder perspective, UEFI’s practical advantage is consistency:

- boot configuration concepts are relatively standardized,
- and boot media is often more portable.

> **Figure idea:** A UEFI boot configuration diagram: NVRAM variables → boot manager → boot option → EFI system partition → operating system loader.

---

## 22.1.3 SBC boot: board-specific, multi-stage, and often more physical

SBC boot is usually shaped by the system-on-chip (SoC) vendor’s design.

A **SoC** (system-on-chip) is a chip that integrates the processor and many peripherals (for example, memory controllers and input/output controllers).

Many SBCs begin with a **boot read-only memory (boot ROM)** routine inside the SoC.

The boot ROM typically knows how to load a first-stage program from one or more fixed locations.

After that first stage runs, it loads later stages.

### Multi-stage boot is common on Arm SBCs

On many Arm platforms, the boot chain is multi-stage.

A common pattern is that a trusted firmware component initializes low-level security and execution levels, then hands off to a general bootloader.

Trusted Firmware-A (often abbreviated TF-A) documents a staged firmware design that supports multi-stage initialization and handoff. [B8]

This structure is a major reason SBC boot behavior can be difficult to generalize:

- the system’s “boot behavior” is split across multiple components.

### U-Boot as a common SBC bootloader

**U-Boot** (Das U-Boot) is a widely used bootloader in embedded and SBC systems.

U-Boot’s documentation emphasizes configurability.

It supports a persistent environment, and it allows boot logic to be expressed through environment variables and scripts (for example, `bootcmd` and argument strings). [B5]

U-Boot also documents a more structured approach called **Standard Boot**, which adds standardized discovery and fallback logic across boot devices. [B4]

This can make SBC behavior feel more like “a boot manager” rather than a one-off script.

### Raspberry Pi as a concrete example of SBC boot configuration

Raspberry Pi systems have widely documented boot behavior and expose boot configuration controls.

Raspberry Pi’s documentation describes boot flow and bootloader configuration mechanisms, including bootloader behavior stored in EEPROM (electrically erasable programmable read-only memory). [B7]

Real-world configuration knobs include boot order selection and failover mechanisms, and recovery can be performed by using specific recovery files to repair a broken bootloader state. [B7][B13]

Jeff Geerling’s practical write-up on updating the Raspberry Pi Compute Module 4 bootloader illustrates an important builder reality:

- some boards have bootloader state that is separate from the removable storage device,
- and changing it is powerful but can also create “brick risk” if done incorrectly. [B12]

---

## 22.1.4 Device tree: why SBC boot is not just “load the kernel”

Many SBCs rely on a mechanism called a **device tree**.

A **device tree** is a data structure that describes the hardware layout (for example, which devices exist and how they are wired).

In many embedded Linux systems, the bootloader passes a **device tree blob (DTB)** (a compiled device tree) to the kernel.

The Linux kernel’s documentation describes how device tree is used as part of platform identification and device population. [B10]

The kernel also documents the expectations for booting AArch64 (Arm 64-bit) systems, including how the kernel is entered and how platform information is provided. [B9]

The Devicetree Specification provides the broader definition of the device tree format and semantics. [B11]

Cyberdeck implication:

- If your boot partition is damaged or mismatched (kernel, DTB, overlays), the system may fail to boot even though the storage device itself is fine.

This is one reason “recovery access to the boot partition” is so valuable in SBC-based decks.

---

## 22.1.5 What you can configure: UEFI versus SBC boot

Both UEFI and SBC boot flows can be configurable.

They are configurable in different ways.

### UEFI configuration tends to be standardized

UEFI defines a consistent boot option model.

You typically configure it through:

- a firmware setup user interface,
- operating-system tooling that edits NVRAM variables,
- or predictable default paths.

The UEFI boot manager model is built around standardized variable names and behaviors, which makes documentation and tooling more portable across machines. [B1]

### SBC configuration is often “files + firmware + physical access”

SBC configuration frequently lives in a mix of places:

- boot configuration files on a boot partition,
- bootloader environment variables,
- and non-removable firmware storage.

U-Boot’s environment model illustrates how powerful this can be.

But it also shows why two boards can behave very differently even when they both “use U-Boot.” [B5]

Raspberry Pi adds another pattern: significant boot behavior can be governed by an EEPROM bootloader that is not simply “whatever is on the microSD card.” [B7][B12]

> **Figure idea:** A “configuration surfaces” table:
> - UEFI: NVRAM variables + EFI system partition
> - SBC: boot ROM straps + firmware flash/EEPROM + boot partition files + bootloader environment

---

## 22.1.6 Recovery paths: how you unbrick a deck in the field

Recovery is where the practical differences become obvious.

### UEFI recovery patterns

UEFI recovery often looks like:

- choose a different boot option,
- boot from removable media,
- or invoke a firmware-defined recovery flow.

UEFI’s boot manager model includes behavior for boot option recovery and defines ways to request recovery behavior. [B1]

Even when the operating system drive is dead, the platform often still has a usable configuration interface.

### SBC recovery patterns

SBC recovery tends to depend on the board and the bootloader.

Common recovery tools include:

- a serial console for early boot debugging,
- a USB-based reflash mode,
- and alternate boot media paths.

U-Boot’s Device Firmware Upgrade (DFU) feature is an example of an explicit USB-based recovery mechanism for updating firmware or storage contents. [B6]

Raspberry Pi supports recovery patterns that can bypass broken bootloader state using recovery files, and community discussion highlights the practical steps for doing so. [B7][B13]

General-purpose SBC distributions also publish recovery guidance.

Armbian’s recovery documentation is an example of a distribution-level approach to “how to recover a system that does not boot,” including alternate access and repair workflows. [B14]

> **Figure idea:** A recovery decision tree:
> 1) Does firmware UI appear?
> 2) Can you boot alternate media?
> 3) Can you access serial console?
> 4) Is there a USB recovery mode (DFU or vendor tool)?
> 5) If not, swap storage and restore from backup.

---

## 22.1.7 Practical guidance for cyberdeck builders

### Choose a platform with recovery you can actually perform

A cyberdeck is not a lab bench.

If recovery requires delicate connectors, special vendor tools, or a second computer you will not have, then it is not a real recovery path.

If your deck must be field-robust:

- consider platforms with standardized boot configuration and predictable recovery, such as UEFI-based systems. [B1]

If you choose an SBC platform, prefer boards with:

- excellent boot documentation,
- a clearly supported recovery process,
- and a community that has already found the sharp edges. [B7][B13][B14]

### Design the enclosure to support recovery

You can “win” recovery before failures happen by designing for it.

Practical examples include:

- physical access to the boot medium,
- an accessible serial header,
- and space to temporarily connect a keyboard, display, or network.

### Keep recovery artifacts ready

A resilient deck often includes:

- a known-good spare boot medium,
- a backup of boot configuration files,
- and a written checklist of the recovery procedure for your exact platform.

The right boot chain is the one you can fix.

---

## Sources

- [B1] UEFI Specification 2.11: Boot Manager — https://uefi.org/specs/UEFI/2.11/03_Boot_Manager.html
- [B2] UEFI Specification 2.11: Secure Boot and Driver Signing — https://uefi.org/specs/UEFI/2.11/32_Secure_Boot_and_Driver_Signing.html
- [B3] TianoCore EDK II Driver Writer’s Guide: Boot Manager Driver List Processing — https://tianocore-docs.github.io/edk2-UefiDriverWritersGuide/draft/3_foundation/315_platform_initialization/31510_boot_manager_driver_list_processing.html
- [B4] U-Boot documentation: Standard Boot overview — https://docs.u-boot.org/en/latest/develop/bootstd/overview.html
- [B5] U-Boot documentation: Environment variables — https://docs.u-boot.org/en/latest/usage/environment.html
- [B6] U-Boot documentation: Device Firmware Upgrade (DFU) — https://docs.u-boot.org/en/latest/usage/dfu.html
- [B7] Raspberry Pi documentation: Raspberry Pi boot and bootloader configuration — https://www.raspberrypi.com/documentation/computers/raspberry-pi.html
- [B8] Trusted Firmware-A documentation: Firmware Design — https://trustedfirmware-a.readthedocs.io/en/stable/design/firmware-design.html
- [B9] Linux kernel documentation: Booting AArch64 Linux — https://www.kernel.org/doc/html/latest/arch/arm64/booting.html
- [B10] Linux kernel documentation: Linux and the Devicetree — https://docs.kernel.org/6.4/devicetree/usage-model.html
- [B11] Devicetree Specification — https://www.devicetree.org/specifications/
- [B12] Jeff Geerling: Compute Module 4 bootloader EEPROM update — https://www.jeffgeerling.com/blog/2022/how-update-raspberry-pi-compute-module-4-bootloader-eeprom/
- [B13] Raspberry Pi Forums: bootloader update and recovery discussion (Pi 4) — https://forums.raspberrypi.com/viewtopic.php?f=29&t=246030
- [B14] Armbian documentation: Recovery — https://docs.armbian.com/User-Guide_Recovery/
