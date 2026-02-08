# 22.5 Recovery design

A cyberdeck is not only a computer.

It is a computer you have physically reconfigured.

That makes it more interesting, but it also makes failure more likely.

The most important reliability skill for cyberdeck builders is not selecting parts.

It is designing a recovery path.

This chapter treats recovery as a design requirement.

It explains why recovery fails in the field, what “good recovery design” looks like, and how to build a layered recovery plan using:

- jumpers and straps,
- buttons and forced modes,
- hidden service ports,
- and external boot media.

The goal is practical: when something breaks, you should be able to restore function without improvising.

---

## 22.5.1 What “recovery” means (and why it is a design problem)

Recovery is the ability to return a system to a usable state after something goes wrong.

“Usable” is not always “all data preserved.”

In many cyberdeck scenarios, a recovery that preserves the operating system and tools is enough, because user data can be restored separately.

Recovery is a design problem because the hardest failures are the ones that remove your usual tools.

If the system does not boot, you cannot rely on:

- the screen,
- the keyboard,
- the network,
- or even a running operating system.

So the recovery design must include tools that work before (or without) the operating system.

---

## 22.5.2 A failure taxonomy for cyberdecks

Different failures require different recovery layers.

A practical taxonomy is:

1) **Configuration failures**

You changed a configuration file and the system behaves incorrectly.

2) **Boot partition failures**

The system cannot load the kernel or hardware description.

On many single-board computer (SBC) systems, this includes device tree problems.

3) **Storage failures**

The storage device is corrupted, disconnected, or dying.

4) **Firmware and bootloader failures**

Boot-critical firmware was updated incorrectly or interrupted.

5) **Power and hardware interface failures**

A cable, connector, or power rail is unstable.

Recovery design should be layered to address each category.

---

## 22.5.3 The “recovery ladder”: layers from easiest to hardest

Good recovery design uses multiple layers.

The correct layer depends on how early the failure occurs.

A useful mental model is a ladder.

1) **Reversible software changes**

This includes rolling back a configuration change.

2) **Snapshots and backups**

This includes restoring a known-good system state.

3) **External boot media**

This includes booting a rescue system from removable storage.

4) **Alternate boot paths / forced boot modes**

This includes forcing the platform to use a safe boot source even when normal boot is broken.

5) **Serial console access**

This includes debugging and controlling early boot stages.

6) **Protocol recovery**

This includes bootloader-level update protocols such as Device Firmware Upgrade (DFU).

7) **External programming**

This includes reflashing firmware using an external programmer.

> **Figure idea:** A ladder diagram showing these layers, labeled with “works without OS?” and “requires opening enclosure?”

---

## 22.5.4 External boot media: the most field-realistic recovery layer

When you are in the field, the most realistic recovery tool is often removable media.

On UEFI systems, the boot manager model is designed to try a sequence of boot options.

It also includes boot option recovery behavior when normal boot options fail. [R3]

On many SBC systems, boot is less standardized, but removable media is still a primary recovery mechanism.

Community recovery threads frequently rely on “boot from a known-good card” workflows. [R14]

### Design implications

If you want external boot media to be a real recovery path:

- the boot slot must be physically accessible,
- the boot medium must be easy to swap,
- and you must keep at least one tested rescue image.

Many enclosures accidentally defeat recovery by burying the microSD card under the battery and a dozen screws.

If you do that, you have chosen “no field recovery” as a design feature.

---

## 22.5.5 Jumpers, straps, and forced boot paths

One of the most powerful recovery tools is a hardware override.

A hardware override is a physical configuration (a jumper, strap, or button) that forces the platform into a safe mode.

### Example: Compute Module boot overrides

Raspberry Pi Compute Module platforms document explicit boot and flashing workflows that involve hardware-level behavior.

These workflows are designed to support recovery and re-imaging, and they include hardware concepts such as boot overrides and bootloader state. [R1]

Jeff Geerling’s practical walkthrough for flashing Compute Module storage illustrates why this matters: recovery often uses a specific USB “device” connection path and a deliberate boot mode, not the normal USB host port. [R11]

This is a recovery design lesson:

- A good platform gives you a way to bypass “whatever is on the deck’s storage” and enter a predictable flashing mode.

### Physical design rule

If your platform uses a strap or jumper for recovery, your enclosure should provide:

- access to that jumper without full disassembly,
- and a clear label.

A hidden recovery strap that requires removing the motherboard is not a recovery feature.

It is a puzzle.

---

## 22.5.6 Buttons and hidden service ports

Many boards have a recovery button or a “mode select” button.

These buttons often trigger a low-level recovery mode in the system-on-chip.

A common pattern is an on-the-go (OTG) USB port that is used as a device port during recovery.

Radxa’s documentation on entering Maskrom mode (a forced boot mode used on some Rockchip-based boards) is a clear example: recovery is a physical interaction that enables a USB-based restore path. [R10]

### The cyberdeck mistake

Builders often expose the “nice” ports and hide the “ugly” port.

But the ugly port is usually the recovery port.

Recovery design means:

- you expose the service port (or at least make it accessible),
- and you route it safely.

---

## 22.5.7 Serial console: your last interface when everything else is gone

A **serial console** is a text interface over a serial port.

It is often the only reliable way to see what happens before the operating system starts.

The Linux kernel documentation describes how to configure and use a serial console, including the `console=` kernel parameter format. [R6]

On many systems, the bootloader prints diagnostic messages to a serial console.

If you can interrupt the bootloader, you can:

- inspect variables,
- change boot targets,
- or boot from alternate media.

Armbian’s troubleshooting guidance explicitly treats serial access as the foundation for deep recovery. [R5]

U-Boot’s DFU documentation assumes bootloader-level control, which is often easiest to reach through serial access when normal boot fails. [R2]

### Designing for UART access

UART (universal asynchronous receiver-transmitter) pins are often exposed as small header pads.

If you do not plan for them, you will eventually need them and regret it.

Raspberry Pi documentation describes early-stage serial behavior (including options intended to support early debugging). [R7]

Jeff Geerling’s UART debugging guide is a practical builder reference that emphasizes the “real world details” you must know: pins, voltage levels, and how to connect. [R12]

Design implications:

- expose the UART pins on a header or a test connector,
- label the pins (ground, transmit, receive),
- document the voltage (for example 3.3 volts),
- and leave physical clearance to attach a cable.

> **Figure idea:** A “service header” diagram showing a four-pin debug header: ground, transmit, receive, and power (optional).

---

## 22.5.8 Protocol recovery: DFU and bootloader-assisted reflashing

If you can reach the bootloader, you can often recover without a full operating system.

DFU is an example of a recovery protocol designed for firmware transfer.

U-Boot documents DFU as a mechanism that can download and upload firmware. [R2]

This is valuable for cyberdecks because:

- it can work even when the root filesystem is corrupted,
- and it can be part of a repeatable recovery process.

A builder-friendly rule is:

- prefer platforms and boot chains with a documented recovery protocol.

---

## 22.5.9 External programming: design for the “worst day”

External programming is the last resort.

It usually means:

- attaching an external programmer to a flash chip,
- reading the contents,
- verifying the backup,
- and writing a new image.

flashrom’s documentation on in-system programming discusses the practical realities of clips, access, and the engineering constraints of flashing chips in place. [R8]

coreboot’s flashing tutorial provides a broader safety framing for external flashing and emphasizes careful procedures. [R9]

Hackaday’s practical write-up on using a small microcontroller board as a programmer illustrates an important point:

- external flashing is not exotic; it is a normal recovery technique when designed for. [R13]

### Designing for external flashing

If external flashing is in your recovery plan, your enclosure and board layout should support:

- access to the flash chip or a dedicated programming header,
- correct voltage compatibility,
- and short, stable wiring.

Also, your process should minimize risk.

In particular:

- prefer reading and verifying a backup before writing,
- and avoid writing more than you must.

This chapter does not provide step-by-step instructions for flashing arbitrary devices.

It focuses on the design decision: whether you can recover at all.

---

## 22.5.10 A recovery playbook template (write this before you need it)

Recovery fails most often because the builder has not written down the procedure.

A recovery playbook should be short.

It should be specific to your deck.

A practical template is:

**A) Identification**

- Deck name and hardware revision.
- Compute platform model.
- Storage type and location.
- Firmware/bootloader version notes.

**B) First-line recovery (no tools)**

- How to force a safe boot mode (button or jumper).
- How to select alternate boot media.

**C) Second-line recovery (minimal tools)**

- Which port is the service port.
- Which cable is required.
- Which rescue image to use.

**D) Serial console recovery**

- Header location.
- Pinout (ground, transmit, receive).
- Voltage.
- Baud rate.

**E) External flashing (last resort)**

- Where the flash chip or programming header is.
- Which programmer/cable is compatible.
- Where the known-good firmware image is stored.

**F) Verification after recovery**

- What you check after reboot.
- How you confirm firmware version and boot behavior.

Armbian’s recovery and troubleshooting documentation illustrate how helpful it is to have a staged recovery approach that begins with boot access and escalates deliberately. [R4][R5]

---

## 22.5.11 Practical takeaway

Recovery design is not a single feature.

It is an attitude:

- assume boot will fail,
- assume the network will be unavailable,
- assume you will not have a full workshop.

Then design your deck so you can still fix it.

That usually means:

- accessible external boot media, [R3]
- at least one forced safe mode (jumper or button), [R10][R11]
- a service port you can reach, [R10]
- and serial console access. [R6][R12]

If you design for recovery, you can build more aggressively without fear.

If you do not, the first mistake becomes a full rebuild.

---

## Sources

- [R1] Raspberry Pi documentation: Compute Module (boot and flashing concepts) — https://www.raspberrypi.com/documentation/computers/compute-module.html
- [R2] U-Boot documentation: Device Firmware Upgrade (DFU) — https://docs.u-boot.org/en/latest/usage/dfu.html
- [R3] UEFI Specification 2.11: Boot Manager (boot option recovery model) — https://uefi.org/specs/UEFI/2.11/03_Boot_Manager.html
- [R4] Armbian documentation: Recovery — https://docs.armbian.com/User-Guide_Recovery/
- [R5] Armbian documentation: Troubleshooting and Recovery (serial and bootloader-level access) — https://docs.armbian.com/User-Guide_Troubleshooting/
- [R6] Linux kernel documentation: serial console — https://docs.kernel.org/admin-guide/serial-console.html
- [R7] Raspberry Pi documentation: serial ports getting started (early boot serial configuration) — https://www.raspberrypi.com/documentation/hardware/raspberrypi/serial-ports/getting-started.html
- [R8] flashrom documentation: in-system programming — https://www.flashrom.org/user_docs/in_system.html
- [R9] coreboot documentation: flashing firmware tutorial — https://doc.coreboot.org/tutorial/flashing_firmware/index.html
- [R10] Radxa documentation: entering Maskrom mode (button + USB recovery path) — https://docs.radxa.com/en/rock5/rock5b/low-level-dev/install-os/rkdevtool_maskrom
- [R11] Jeff Geerling: flashing Raspberry Pi Compute Module eMMC with usbboot — https://www.jeffgeerling.com/blog/2020/how-flash-raspberry-pi-os-compute-module-4-emmc-usbboot/
- [R12] Jeff Geerling: attaching to Raspberry Pi serial console for UART debugging — https://www.jeffgeerling.com/blog/2021/attaching-raspberry-pis-serial-console-uart-debugging/
- [R13] Hackaday: programming SPI flash chips using an RP2040/Pico with flashrom — https://hackaday.com/2023/03/06/programming-spi-flash-chips-use-your-pico/
- [R14] Raspberry Pi Forums: Pi 5 EEPROM recovery discussion — https://forums.raspberrypi.com/viewtopic.php?t=383085
