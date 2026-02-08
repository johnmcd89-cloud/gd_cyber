# 22.2 Device tree (builder mental model)

Single-board computers (SBCs) often feel “less automatic” than desktop computers.

On a desktop computer, you can plug in a device and it usually appears.

On many SBCs, you can plug in a peripheral and nothing happens.

This is not always a wiring mistake.

Very often, it is a **hardware description** problem.

That hardware description is frequently provided through a **device tree**.

This chapter builds a practical mental model of device tree for cyberdeck builders.

The goal is to help you answer questions like:

- Why does my peripheral not show up?
- What does it mean to “enable” a bus?
- Why does an overlay apply, but the device still does not exist?

---

## 22.2.1 What a device tree is

A **device tree** is a structured description of hardware.

It is data, not executable code.

It describes things such as:

- which buses exist (for example, Inter-Integrated Circuit (I2C) and Serial Peripheral Interface (SPI)),
- which devices are connected to those buses,
- which pins are used,
- and which interrupt lines and addresses are assigned.

The Linux kernel documentation describes device tree as a model for platform identification and device discovery on many embedded systems. [DT1]

The Devicetree Specification defines the format and the meaning of common properties in a standardized way. [DT7][DT6]

A useful builder framing is:

- device tree is the “wiring diagram” you hand to the operating system.

If the operating system does not know a device exists, it cannot load a driver for it.

---

## 22.2.2 Device tree source, blobs, and overlays (the file types)

You will see several related terms.

They are not interchangeable.

- **Device tree source (DTS)** is human-readable text that describes hardware.

- **Device tree blob (DTB)** is a compiled binary representation of a device tree.

- **Device tree overlay** is a fragment that modifies an existing device tree, usually to enable optional hardware or change pin assignments.

The Linux kernel documentation includes specific notes on overlays and how they are applied. [DT3]

In practice, overlays exist because many boards have optional hardware configurations.

A cyberdeck might add a screen, a radio module, or a keyboard controller.

An overlay can describe that addition.

---

## 22.2.3 How device tree connects to the boot chain

On many Arm-based systems, the bootloader passes a device tree blob to the kernel.

This is part of the boot handoff contract.

The Linux kernel documentation on booting Arm 64-bit (AArch64) systems describes the boot interface expectations. [DT5]

The device tree usage model documentation explains how device tree is used to describe the platform so that Linux can instantiate devices. [DT1]

Builder implication:

- device tree is not an “optional configuration file.”
- it is often the primary way the operating system learns what hardware exists.

> **Figure idea:** A boot handoff diagram: firmware → bootloader → kernel + (DTB) → drivers → user space.

---

## 22.2.4 Why some peripherals do not “show up”

In cyberdeck builds, peripherals often fail to appear for predictable reasons.

### Reason 1: the bus is not self-discovering

Many common peripheral buses used in cyberdecks are not self-describing.

For example, SPI and many GPIO-attached devices do not have a universal discovery protocol.

The operating system cannot scan the bus and reliably deduce what device is present.

So the device must be described.

The device tree usage model frames device tree as the mechanism for describing such non-discoverable devices. [DT1]

Armbian’s overlay documentation reinforces the builder reality: if the device is not described (or the overlay is not loaded), the device will not appear. [DT11]

### Reason 2: the device exists in the tree, but is disabled

Device tree nodes can have a `status` property.

The Devicetree Specification documents `status` as a conventional mechanism used to indicate whether a hardware component is enabled or disabled. [DT6]

In practical terms:

- if `status` is set to `disabled`, Linux behaves as if that device does not exist.

Raspberry Pi documentation makes this extremely concrete because many peripherals are enabled through overlays that change node status and pin configuration. [DT9]

### Reason 3: no driver binds to the described device

A device tree node usually includes a `compatible` string.

The `compatible` string is a way to match a device tree node to a driver.

If the `compatible` string is wrong, too generic, or not supported by the running kernel, then no driver will bind.

Linux kernel documentation on writing bindings emphasizes the role of device tree bindings and compatible strings as part of the driver interface contract. [DT2]

This is one of the most common “it is wired correctly but nothing appears” failure modes.

### Reason 4: overlay application depends on correct symbol and fixup metadata

Overlays are not simple “paste this text.”

They can depend on symbols and fixups to attach changes to the right place in the base tree.

The Linux kernel overlay documentation and dynamic resolution notes describe how overlays rely on symbol metadata (such as `__symbols__` and fixups). [DT3][DT4]

If the overlay was compiled without the needed metadata, the overlay may appear to load but not actually connect to the intended nodes.

The device tree compiler documentation notes options relevant to producing overlay-capable artifacts (for example, generating symbol information). [DT8]

Builder implication:

- “overlay loaded” does not always mean “device created.”

### Reason 5: resource conflicts (pins, addresses, chip selects)

Two devices can compete for the same hardware resources.

Typical conflicts include:

- pin multiplexing conflicts (the same pin cannot be used for two functions at once),
- two devices trying to use the same address on a bus,
- or two SPI devices attempting to share a chip select line.

Armbian’s overlay guide discusses the practical need to choose overlays that do not conflict. [DT11]

Toradex documentation on overlays highlights that conflicting overlays can require removal or restructuring, and that configuration layering matters. [DT12][DT13]

---

## 22.2.5 The most important mental model: device tree is a contract

It is tempting to treat device tree as “where you hack settings until it works.”

A better mental model is:

- device tree is a contract between hardware description and software expectations.

The Linux device tree usage model emphasizes that the tree should describe the hardware as it is, not as you wish it to behave. [DT1]

Similarly, the Devicetree Specification frames device tree as a standardized description mechanism, with defined semantics for common properties. [DT6][DT7]

Builder implication:

- do not encode “policy” into hardware description.
- describe the hardware accurately.

Policy (for example, which service to start) belongs in the operating system configuration.

---

## 22.2.6 A safe, repeatable debugging workflow

Debugging device tree is easiest when you work from observable facts.

### Step 1: inspect the live merged device tree

Linux exposes a view of the active device tree at runtime.

You can inspect it using the filesystem interface (commonly `/proc/device-tree` or a similar path).

A common approach is to use the device tree compiler tooling to read from the filesystem view.

The device tree compiler manual documents modes and input formats that support this kind of inspection. [DT8]

If the node is not present in the live tree, Linux will not create the device.

### Step 2: confirm overlay loading and boot configuration

On Raspberry Pi, overlays are often configured through boot configuration files.

Raspberry Pi’s documentation describes overlays, parameters, and practical configuration mechanisms. [DT9]

It also documents boot configuration conventions in `config.txt`, which is frequently where overlay choices are made. [DT10]

If you can access boot logs (especially via a serial console), you can often see whether overlays were applied.

### Step 3: check driver probe outcomes

If the node is present and enabled, but the device is still missing, the next question is driver binding.

Look for driver probe messages and errors.

Often the problem is:

- unsupported compatible string,
- missing kernel module,
- or a hardware resource conflict.

Toradex’s overlay guides include troubleshooting-oriented framing that matches this sequence: confirm the tree, then confirm the driver. [DT13]

> **Figure idea:** A three-layer debugging stack:
> 1) Hardware power/wiring
> 2) Device tree description (node + status + resources)
> 3) Driver binding (compatible + module + probe)

---

## 22.2.7 Builder guidance: designing for device tree reality

### Keep the boot partition recoverable

Many device tree failures are “boot partition failures.”

If you can mount and edit your boot partition from another computer, you can often recover quickly.

That is why cyberdecks benefit from:

- accessible storage,
- and a plan to edit boot configuration without the deck itself booting.

### Minimize overlay complexity

Overlays are powerful.

They are also additive complexity.

If you stack many overlays, you increase the chance of resource conflicts and hard-to-debug behavior.

Prefer:

- the minimum set of overlays that describe your hardware,
- and keep them documented.

### Remember the “three prerequisites” for a working peripheral

A peripheral usually needs all three:

1) physical connectivity and power,
2) correct device tree description,
3) a driver that binds and initializes.

If any one is missing, the peripheral does not exist from the operating system’s perspective.

---

## Sources

- [DT1] Linux kernel documentation: Device tree usage model — https://docs.kernel.org/devicetree/usage-model.html
- [DT2] Linux kernel documentation: Writing device tree bindings — https://docs.kernel.org/devicetree/bindings/writing-bindings.html
- [DT3] Linux kernel documentation: Device tree overlays — https://docs.kernel.org/devicetree/overlay-notes.html
- [DT4] Linux kernel documentation: Device tree dynamic resolution notes — https://docs.kernel.org/devicetree/dynamic-resolution-notes.html
- [DT5] Linux kernel documentation: Booting AArch64 Linux — https://docs.kernel.org/arch/arm64/booting.html
- [DT6] Devicetree Specification (basics, including status property) — https://devicetree-specification.readthedocs.io/en/stable/devicetree-basics.html
- [DT7] Devicetree.org: Specifications index — https://www.devicetree.org/specifications/
- [DT8] Device tree compiler (dtc) manual — https://kernel.googlesource.com/pub/scm/utils/dtc/dtc.git/+/9b62ec84bb2da0ce42bc978ce8dc0ba197c9fa71/Documentation/manual.txt
- [DT9] Raspberry Pi documentation: Device tree, overlays, and parameters — https://www.raspberrypi.com/documentation/configuration/device-tree
- [DT10] Raspberry Pi documentation: config.txt (boot configuration, relevant to overlays) — https://www.raspberrypi.com/documentation/hardware/computemodule/computers/config_txt.html
- [DT11] Armbian documentation: Overlays user guide — https://docs.armbian.com/User-Guide_Armbian_overlays/
- [DT12] Toradex documentation: Device tree overlays on Linux (including conflict considerations) — https://developer.toradex.com/linux-bsp/os-development/build-yocto/device-tree-overlays-linux/
- [DT13] Toradex documentation: How to write device tree overlays (troubleshooting and workflow framing) — https://developer.toradex.com/software/linux-resources/device-tree/how-to-write-device-tree-overlays/
