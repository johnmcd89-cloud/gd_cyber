# 28.2 OTG vs host pitfalls

USB feels simple when it works.

You plug a device in.

It appears.

In cyberdeck builds, USB often does not “just work,” especially on single-board computers.

This is not only a driver problem.

It is often a role and power problem.

This chapter explains the most common failure mode behind the question:

> “Why isn’t this device recognized?”

The answer is frequently:

- the port is in the wrong role,
- the cable is the wrong kind,
- or the power path is wrong.

The focus is a required topic:

- SBC port roles and common recognition pitfalls.

---

## 28.2.1 Definitions: host, device, and dual-role behavior

A USB link is asymmetric.

One side is the **host**.

The host provides:

- bus management,
- device enumeration,
- and usually power on the bus.

The other side is the **device** (also called a peripheral).

A keyboard, a flash drive, or a camera is usually a device.

Some ports can operate in either role.

This is often described as:

- dual-role,
- or “On-The-Go” (OTG) behavior.

Modern USB Type-C ports can support role selection and role swapping.

Role swapping is not magic.

It depends on:

- physical signaling on the connector,
- and policy decisions implemented by the port controller and operating system.

USB-IF publishes compliance and policy update documents for USB Type-C, Power Delivery, and cable/connector behavior.

These documents are a practical entry point into how role and cable policy evolves over time. [O1][O2][O3]

---

## 28.2.2 The simplest pitfall: wrong port, wrong cable

Many SBCs have multiple USB connectors.

Some are power-only.

Some are data.

Some are dual-role.

If you connect a peripheral to a power-only port, nothing will enumerate.

### Raspberry Pi Zero class example

Raspberry Pi Zero class boards famously have two micro-USB connectors:

- one for power,
- and one for data/OTG.

If you plug your device into the power-only port, it will not be recognized.

Raspberry Pi’s “USB gadget mode” post is a practical reference because it explicitly uses the OTG-capable port for gadget mode and treats the power port separately. [O12]

Raspberry Pi forum threads frequently show this exact confusion in practice. [O13]

Cyberdeck implication:

- always identify which physical connector is the data-capable port.

---

## 28.2.3 Gadget mode: when you accidentally turn your port into a device

A common cyberdeck goal is:

- “SSH over USB,”
- or “network gadget mode,”

where the cyberdeck appears as a USB device to another computer.

This configuration can intentionally make the SBC act as a device.

That means the port is no longer acting as a host.

So you cannot expect a USB keyboard or flash drive to enumerate on that same port.

Raspberry Pi’s gadget mode guide demonstrates this configuration pattern and is a common cause of “my USB devices stopped working” when the same port is repurposed. [O12]

Cyberdeck implication:

- decide which port is your “device-to-host” port.

Do not assume it can also be your peripheral host port.

---

## 28.2.4 Verifying the role in Linux (do not guess)

When USB is failing, guessing is expensive.

On Linux, you can inspect kernel role state.

### usb_role class

Linux exposes a role switch interface in sysfs.

The `usb_role` sysfs class provides a role attribute that can show:

- none,
- host,
- or device.

The kernel ABI documentation describes this sysfs interface. [O8]

Cyberdeck implication:

- if a device is not recognized, first confirm the port role is actually host.

### Type-C sysfs state

On Type-C systems, Linux also exposes Type-C role information.

The kernel ABI documentation for the Type-C class describes attributes such as data role and power role. [O9]

Cyberdeck implication:

- a Type-C port can be “connected” physically but still negotiated into an unexpected role.

---

## 28.2.5 DWC2 and SBC controller configuration pitfalls

Many SBCs use USB controller blocks that can run in multiple modes.

One common controller is DWC2.

Its behavior is often configured by device tree settings such as a “device role mode.”

If the role mode is wrong, the port will not behave like you expect.

Linux device tree binding documentation for DWC2 provides the formal definition of configuration properties. [O11]

Raspberry Pi forum discussions show the practical consequence: misconfiguration leaves the board in the wrong role, and devices do not enumerate. [O13]

Cyberdeck implication:

- if you are using an SBC in a custom carrier or custom kernel configuration, treat USB role configuration as part of bring-up.

---

## 28.2.6 Capturing what happens during enumeration

If the port is in the correct role, the next question is:

- does enumeration begin at all?

Linux provides usbmon, a tracing facility for USB traffic.

The kernel documentation describes how usbmon works and how it can be used for debugging. [O10]

Cyberdeck implication:

- usbmon can separate “nothing happens” from “enumeration starts then fails.”

Those are different root causes.

---

## 28.2.7 USB Type-C specifics: CC resistors and “nothing detected” failures

USB Type-C adds a dedicated signaling mechanism to determine:

- attachment,
- cable orientation,
- and role behavior.

This relies on configuration channel (CC) termination.

If CC termination is wrong:

- the port may not detect attachment,
- or it may negotiate an unintended role.

Microchip’s documentation on CC resistors provides a clear explanation of the role of pull-up and pull-down resistors in Type-C behavior. [O4]

Infineon’s knowledge base article on Rp/Rd/Ra termination resistors provides additional practical framing for the same concept. [O7]

Cyberdeck implication:

- on a custom cyberdeck carrier board, CC resistors are not optional.

They are part of “does it connect at all.”

### Real-world example: Raspberry Pi 4 USB-C issue

The Raspberry Pi 4 had a well-known early USB-C behavior issue related to how it presented itself to certain Type-C cables.

Hackaday’s technical writeup is a useful community reference for how subtle Type-C termination details can produce “it doesn’t work with this cable” failures. [O14]

Cyberdeck implication:

- if a Type-C port works with one cable and not another, suspect Type-C identification and current advertisement behavior.

---

## 28.2.8 Hub power switching and back-powering

Sometimes the device is not recognized because:

- the hub is powering incorrectly,
- or current is flowing backward.

### Per-port versus ganged power behavior

Hubs can:

- power all ports together,
- or power ports individually.

Overcurrent behavior can:

- shut down one port,
- or collapse an entire segment.

Texas Instruments’ hub product documentation can include power switching and overcurrent behavior modes that shape these symptoms. [O5]

Microchip’s hub evaluation board documentation is another example where individual port power and overcurrent sensing are explicit design features. [O6]

Cyberdeck implication:

- the same “device disappears” symptom can be caused by power switch policy, not enumeration logic.

### Cables and unsafe assumptions

Nonstandard cable choices can create power-path hazards.

USB-IF compliance update documentation includes cable and connector guidance that warns against dangerous or noncompliant combinations. [O3]

Cyberdeck implication:

- do not debug role problems with unknown cables.

Use known-good cables.

---

## 28.2.9 A practical debug checklist

When a device is not recognized:

1) Confirm the physical port.

Is it the data port or the power-only port? [O12][O13]

2) Confirm the role.

On Linux, inspect `/sys/class/usb_role`. [O8]

On Type-C systems, inspect `/sys/class/typec`. [O9]

3) Confirm the configuration.

Check DWC2 role configuration and VBUS setup if using a dual-role controller. [O11]

4) Confirm whether enumeration begins.

Use usbmon to see whether traffic exists. [O10]

5) Confirm Type-C correctness.

If Type-C is involved, verify CC termination and try multiple known-good cables. [O4][O7][O14]

6) Check power switching and back-power paths.

Try a self-powered hub.

Try removing high-current devices.

Inspect per-port overcurrent behavior. [O5][O6]

---

## 28.2.10 Practical takeaway

USB recognition failures on cyberdecks are often role failures.

The fastest path is to stop guessing:

- confirm the physical connector and whether it carries data, [O12]
- confirm the data and power roles in the operating system, [O8][O9]
- and treat Type-C termination and cable identity as first-class variables. [O4][O7]

Once role and power are correct, then drivers matter.

Until role and power are correct, “driver troubleshooting” is usually wasted effort.

---

## Sources

- [O1] USB-IF: compliance updates (USB Type-C) — https://compliance.usb.org/index.asp?Format=Standard&UpdateFile=USBC
- [O2] USB-IF: compliance updates (USB Power Delivery) — https://compliance.usb.org/index.asp?Format=Standard&UpdateFile=PowerDelivery
- [O3] USB-IF: compliance updates (cables and connectors) — https://compliance.usb.org/index.asp?Format=Standard&UpdateFile=Cables+and+Connectors
- [O4] Microchip: CC resistors and devices (Type-C termination) — https://onlinedocs.microchip.com/oxy/GUID-DAF68E19-EB64-4E93-83DB-9E883647B776-en-US-1/GUID-02E3AD6F-2676-4B83-9D2E-31D93E162FA3.html
- [O5] Texas Instruments: TUSB2036 hub (power switching / overcurrent modes) — https://www.ti.com/product/TUSB2036
- [O6] Microchip: EVB-USB5744 evaluation board (individual port power / overcurrent) — https://www.microchip.com/en-us/development-tool/EVB-USB5744
- [O7] Infineon: Type-C Rp/Rd/Ra termination resistors (KBA) — https://community.infineon.com/t5/Knowledge-Base-Articles/USB-Type-C-connector-Rp-Rd-and-Ra-termination-resistors/ta-p/253544
- [O8] Linux kernel ABI: sysfs-class-usb_role — https://www.kernel.org/doc/Documentation/ABI/testing/sysfs-class-usb_role
- [O9] Linux kernel ABI: sysfs-class-typec — https://www.kernel.org/doc/Documentation/ABI/testing/sysfs-class-typec
- [O10] Linux kernel docs: usbmon — https://docs.kernel.org/usb/usbmon.html
- [O11] Linux kernel devicetree binding: DWC2 — https://www.kernel.org/doc/Documentation/devicetree/bindings/usb/dwc2.txt
- [O12] Raspberry Pi: USB gadget mode (SSH over USB) — https://www.raspberrypi.com/news/usb-gadget-mode-in-raspberry-pi-os-ssh-over-usb/
- [O13] Raspberry Pi forums: USB on Raspberry Pi Zero W not working — https://forums.raspberrypi.com/viewtopic.php?t=220562
- [O14] Hackaday: Raspberry Pi 4 USB-C issue in-depth — https://hackaday.com/2019/07/16/exploring-the-raspberry-pi-4-usb-c-issue-in-depth/
