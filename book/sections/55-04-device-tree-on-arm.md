# 55.4 Device tree on ARM

Many cyberdeck builds use small, efficient computers based on the **ARM** family of processor architectures.

These systems often integrate most functionality into a single chip.

When hardware is integrated like this, the operating system frequently cannot “discover” it by probing a standard bus.

It needs to be told what hardware exists, where it is connected, and how it should be configured.

On Linux systems in this category, that hardware description is usually provided using a **device tree**.

This chapter explains what device trees are, why they exist, how they are delivered to the Linux kernel at boot, and how device tree mistakes show up as confusing “missing device” problems.

---

## 55.4.1 Definitions (ARM, system-on-chip, device tree)

### ARM

**ARM** is a family of instruction set architectures used by many embedded and mobile processors.

It is common in single-board computers.

### System-on-chip

A **system-on-chip** (often shortened to **system on a chip**) is an integrated circuit that combines many subsystems that used to be separate chips.

This is sometimes abbreviated as **SoC**.

It may contain:

- central processing units,
- memory controllers,
- graphics,
- and peripheral controllers such as serial ports.

The key practical consequence is that many peripherals are “on the chip,” not on a self-describing external bus.

### Device tree

A **device tree** is a structured description of hardware that is meant to be read by an operating system.

The Linux kernel documentation describes the “Open Firmware Device Tree” (often shortened to Devicetree, or abbreviated **DT**) as a data structure and language for describing hardware, so that the operating system does not need to hard code details of a specific machine. [S1]

Devicetree.org similarly frames the devicetree as a data structure passed to the operating system at boot time, describing hardware instead of hard coding details in the operating system. [S2]

---

## 55.4.2 Device tree source and device tree blob

You will encounter device trees in two main forms.

### Device tree source

A **device tree source** is a human-readable text form.

It is usually written with the file extension `.dts`.

A device tree source describes hardware as a tree of nodes with properties.

### Device tree blob (device tree binary)

A **device tree blob** (also called a **device tree binary**) is a compiled binary form that the kernel can consume efficiently.

It is usually written with the file extension `.dtb`.

It is often abbreviated as **DTB** (Device Tree Binary).

Bootlin’s device tree overlay article uses this abbreviation and explains that the DTB is passed to the kernel at boot time to tell the kernel which system-on-chip and devices to initialize. [S5]

---

## 55.4.3 Why device tree exists (what it solves)

Device tree solves a practical scaling problem.

Without it, the kernel would need a different compiled “board file” for each hardware variant.

With it, one kernel can support many boards, as long as each board provides an accurate hardware description.

The Linux kernel device tree usage model explicitly emphasizes this goal: describe hardware in data rather than hard code it into the kernel, and reuse common conventions called bindings. [S1]

Bootlin’s article gives a cyberdeck-relevant example: devices connected to buses such as Inter-Integrated Circuit (I2C) and Serial Peripheral Interface (SPI) do not always provide a mechanism for dynamic enumeration and identification, so you must describe them. [S5]

---

## 55.4.4 How the bootloader and kernel interact (passing the device tree)

A device tree is usually provided to the kernel by the **bootloader**.

A bootloader is the program that runs first at power-on and loads the operating system.

U-Boot is a widely used bootloader in embedded Linux systems.

U-Boot’s documentation describes using a **flattened device tree (FDT)** as a “vehicle for run-time configuration,” enabling a single U-Boot binary to support multiple boards, with the exact configuration controlled by the devicetree. [S3]

In a typical boot flow:

- the bootloader chooses the correct `.dtb` file (or modifies it),
- it places the device tree blob in memory,
- and it passes a pointer to the kernel.

From the cyberdeck builder’s perspective, the key idea is:

If the wrong device tree blob is used, the kernel may “correctly” ignore hardware that physically exists.

---

## 55.4.5 Basic device tree concepts (nodes, properties, compatible strings)

A device tree is a tree of **nodes**.

A node represents a hardware component or a logical bus.

Each node has **properties**, which are key/value pairs.

The Linux kernel documentation describes this structure: named nodes with arbitrary properties, plus mechanisms to create links outside the natural tree structure. [S1]

### Compatible strings

One of the most important properties is the **compatible** property.

A compatible string is a text identifier that indicates what a node represents.

Drivers match on compatible strings.

If a compatible string is wrong, the kernel may not bind the correct driver.

The result looks like a missing device.

### Bindings

A **binding** is a convention that defines how to describe a certain kind of hardware in the tree.

The Linux kernel documentation calls these usage conventions “bindings” and warns against inventing new ones without first checking what already exists. [S1]

In practice, bindings are why two different boards can describe “an I2C controller” in a consistent way.

---

## 55.4.6 Overlays (making incremental changes)

A **device tree overlay** is a piece of device tree data intended to modify a base device tree.

Overlays are used when you want to add or modify devices without rebuilding the entire base device tree.

This is common when adding expansion hardware.

The Linux kernel’s “Devicetree Overlay Notes” describes an overlay’s purpose as modifying the kernel’s live tree so that the kernel’s device state reflects the changes: creating new device nodes should cause devices to be registered, and removing or disabling nodes should lead to deregistration. [S4]

Bootlin’s overlay article gives concrete user motivations that match cyberdeck life:

- declaring external devices connected to board connectors,
- changing pin multiplexing (choosing which internal peripheral is wired to which pins),
- and modifying operating-system-related properties. [S5]

---

## 55.4.7 Cyberdeck practice: enabling devices and recognizing configuration failures

Device tree problems are deceptively mechanical.

A small configuration mistake can prevent an entire subsystem from appearing.

### Common situations

In cyberdeck builds, device trees often come up when you add:

- a display connected via the **Serial Peripheral Interface (SPI)**,
- a sensor connected via the **Inter-Integrated Circuit (I2C)** bus,
- **general-purpose input/output (GPIO)** pins for buttons,
- or extra serial ports.

#### Serial Peripheral Interface (SPI)

**Serial Peripheral Interface (SPI)** is a bus commonly used for displays and fast peripherals.

Many SPI peripherals do not self-enumerate in a way the kernel can use.

You typically describe the peripheral in the device tree so the kernel knows:

- which SPI controller it is connected to,
- which chip-select line it uses,
- and which driver to bind.

#### Inter-Integrated Circuit (I2C)

**Inter-Integrated Circuit (I2C)** is a two-wire bus commonly used for sensors.

Many I2C sensors do not self-enumerate in a way the kernel can use.

You typically describe the sensor in the device tree so the kernel knows:

- which bus it is on,
- its address,
- and which driver to bind.

Bootlin explicitly cites I2C devices (for example, a temperature sensor) as a reason normal users may need to tweak the device tree. [S5]

### Symptoms of a misconfigured device tree

A misconfigured device tree often produces one of these outcomes:

- the kernel never creates the expected device at all,
- the kernel creates a device but binds the wrong driver,
- or the device exists but works incorrectly due to missing properties such as interrupts or pin configuration.

These symptoms can resemble hardware failure.

But if they correlate with “I just changed an overlay,” they are usually configuration failures.

A reliable way to debug is:

- confirm you are booting the intended `.dtb`,
- confirm overlays are actually applied,
- and then check kernel logs for driver probe messages.

---

## 55.4.8 Suggested figures

1) **Boot flow diagram**: bootloader selects DTB → passes pointer to kernel → kernel parses device tree → driver binding via compatible strings.

2) **Tree diagram**: a simple devicetree with a system-on-chip node containing an I2C bus node, which contains a sensor node.

3) **Overlay diagram**: base tree plus overlay that adds a new SPI device; show how the live tree changes (based on kernel overlay notes). [S4]

4) **Failure map**: “device missing” mapped to likely causes (wrong DTB, overlay not applied, wrong compatible string, missing pin configuration).

---

## 55.4.9 Practical takeaway

On ARM-based systems, device trees are the hardware map.

If a device is not described (or described incorrectly), Linux may behave as if the device does not exist.

When you add hardware to a cyberdeck, overlays are often the cleanest way to extend the hardware map.

Treat device tree debugging as configuration debugging.

The hardware can be fine.

The description can be wrong.

---

## Sources

- [S1] Linux kernel documentation: *Linux and the Devicetree (usage model)* (definitions; nodes/properties; bindings; motivation) — https://www.kernel.org/doc/html/latest/devicetree/usage-model.html
- [S2] devicetree.org: *The Devicetree Project* (high-level definition; specification link; devicetree passed at boot) — https://www.devicetree.org/
- [S3] U-Boot documentation: *Devicetree Control in U-Boot* (flattened devicetree for run-time configuration; tools; relationship to bootloaders) — https://docs.u-boot.org/en/latest/develop/devicetree/control.html
- [S4] Linux kernel documentation: *Devicetree Overlay Notes* (kernel overlay mechanism; how overlays affect device registration) — https://www.kernel.org/doc/html/latest/devicetree/overlay-notes.html
- [S5] Bootlin (Michael Opdenacker): *Using Device Tree Overlays, example on BeagleBone boards* (practical overlay motivations; I2C/SPI; DTB passed by bootloader; user workflow context) — https://bootlin.com/blog/using-device-tree-overlays-example-on-beaglebone-boards/

Additional standards reference (download):

- Devicetree.org: *Devicetree Specification v0.4* (technical format and best practices) — linked from https://www.devicetree.org/

Additional textbook reference (print):

- Chris Simmonds: *Mastering Embedded Linux Programming* (Packt). (Background on bootloaders, hardware description, and embedded Linux bring-up.)
