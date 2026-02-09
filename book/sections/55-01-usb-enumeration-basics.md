# 55.1 USB enumeration basics

Universal Serial Bus (USB) is designed so that you can plug a device into a computer and have it “just work.”

The mechanism that makes this possible is called **enumeration**.

**Enumeration** is the process by which a USB **host** detects that a USB **device** has been connected, learns what the device is, assigns it an address on the bus, and chooses a usable configuration so that higher-level software (such as a driver) can communicate with it.

In cyberdeck builds, USB problems are common because cables are short, connectors are stressed, power is improvised, and hubs are often involved.

When enumeration fails, the symptom is usually simple.

The device does not appear.

But the causes range from power instability to a single wrong resistor.

This section explains what enumeration is, what the host and device exchange during enumeration, and how to observe and debug enumeration failures.

---

## 55.1.1 Roles on the USB bus (host, device, hub)

USB is not symmetric.

A USB connection has roles.

A **host** is the controller that initiates communication.

A typical host is a laptop, a single-board computer, or a microcontroller in “USB host” mode.

A **device** is the peripheral that responds to the host.

Keyboards, storage devices, audio adapters, and microcontrollers in “USB device” mode are all devices.

A **hub** is a device that expands one upstream port into multiple downstream ports.

Many cyberdecks contain internal hubs.

They make wiring convenient.

They also add failure modes.

The host is responsible for powering the port, detecting attachment, and coordinating enumeration.

The device is responsible for responding correctly to a standard set of requests defined by the USB specification.

---

## 55.1.2 What the host needs to learn (descriptors, VID/PID, endpoints)

Before a host can use a device, it needs a description of the device.

USB devices provide this description in data structures called **descriptors**.

A **descriptor** is a structured block of bytes that tells the host something about the device.

Microsoft’s USB documentation summarizes this model: the host retrieves descriptors using standard control requests called Get Descriptor (GET_DESCRIPTOR), and descriptors describe the device, its configurations, interfaces, and endpoints. [S5]

### Vendor ID (VID) and Product ID (PID)

A key part of the device description is the pair:

- **Vendor ID (VID)**: an identifier for the manufacturer.
- **Product ID (PID)**: an identifier for the specific product.

Operating systems often use the VID and PID (along with other descriptor fields) to decide which driver to load.

The `lsusb` tool on Linux explicitly notes that it associates human-readable names with the vendor ID and product ID using a hardware database. [S9]

### Configurations, interfaces, and endpoints

USB descriptors form a hierarchy.

A device has one **device descriptor**.

A device then offers one or more **configurations**.

A **configuration** is a complete, self-consistent “mode” of the device.

A configuration contains one or more **interfaces**.

An **interface** is a functional grouping (for example, the audio function of a headset).

Each interface uses one or more **endpoints**.

An **endpoint** is a logical data source or sink.

The Linux USB guide describes this hierarchy and emphasizes that configurations can affect global settings such as power consumption. [S4]

BeyondLogic’s “USB in a Nutshell” explains the same structure from a firmware perspective, including the idea that a device may provide multiple configurations, sometimes for different power situations (bus-powered versus self-powered). [S3]

---

## 55.1.3 Control transfers and the default endpoint (how the questions are asked)

During enumeration, the host and device communicate using **control transfers**.

A **control transfer** is a reliable request/response exchange used for configuration and management.

Every USB device must support a special control channel called the **default pipe**, which uses **endpoint zero**.

Requests are carried in an eight-byte **setup packet**.

BeyondLogic’s description of setup packets and request timing is a practical reminder that enumeration has real timeouts, and that “slow” firmware (for example, firmware that blocks in a debugger) can cause failures. [S2]

---

## 55.1.4 The enumeration sequence (what happens after you plug in)

Enumeration is not one packet.

It is a sequence of stages.

Different documents describe it with different levels of detail.

A useful vendor summary is FTDI’s technical note, which defines enumeration as detecting, identifying, and loading drivers, and provides an overview of the mechanics. [S7]

A particularly clear step-by-step list appears in Microchip’s application note, which includes an excerpt from the USB 2.0 specification (Chapter 9, “USB Device Framework”). [S6]

At a high level, the host does the following.

### Stage 1: attachment and power stabilization

When you plug a device in, a hub detects a physical change on the port.

The hub reports the change to the host.

The device is now physically attached and powered, but it is not yet configured.

Microchip’s excerpt notes that the host waits at least 100 milliseconds to allow insertion to complete and power at the device to become stable. [S6]

### Stage 2: port reset

The host commands the hub (or the host controller) to reset the port.

Reset places the device into a known initial state.

### Stage 3: address assignment

Initially, a newly attached device responds at a default address.

The host assigns a unique address using a standard request called Set Address (SET_ADDRESS).

After this, the device responds on its new address.

### Stage 4: descriptor reads

The host reads the device descriptor and then reads the configuration information.

The goal is to learn:

- what the device is,
- what configurations and interfaces it offers,
- what endpoints it needs,
- and what power it claims to require.

The Microsoft USB descriptor documentation describes how GET_DESCRIPTOR requests retrieve device and configuration information. [S5]

### Stage 5: configuration selection

The host chooses one configuration and sends a request called Set Configuration (SET_CONFIGURATION) that activates it.

The Linux USB guide notes that configurations are activated only through this standard control transfer. [S4]

### Stage 6: driver binding and normal operation

Once configured, the operating system selects and loads a driver.

From this point forward, most communication happens on non-zero endpoints (for example, bulk endpoints for storage, interrupt endpoints for keyboards, or isochronous endpoints for audio).

---

## 55.1.5 Power negotiation and “why it enumerates sometimes”

Many builders treat USB power as “5 volts is 5 volts.”

In practice, USB power has rules.

If you violate them, enumeration can become intermittent.

### Default power and MaxPower

USB devices advertise their power needs as part of their configuration.

Windows’ USB configuration descriptor documentation shows an example configuration descriptor containing `bmAttributes` (bus-powered versus self-powered) and `MaxPower` (the maximum current draw when bus-powered). [S8]

BeyondLogic also calls out that the configuration descriptor includes whether the device is self-powered or bus-powered and the maximum power consumption for that configuration. [S3]

From a design perspective, a critical early constraint is that a device should behave conservatively before it has been configured.

A widely cited practical explanation is the Dangerous Prototypes note on proper current advertisement.

It emphasizes that the descriptors do not “grant” power by themselves, and that devices must manage their own current draw so that attaching a device does not cause the bus voltage to droop. [S11]

### Inrush current and bus droop

When a device is plugged in, capacitors charge.

This creates a brief **inrush current**.

Even if a device’s steady-state current draw is within limits, inrush current can momentarily pull the bus voltage down.

If the voltage droops at the wrong moment, the device may reset or misbehave during descriptor reads.

The practical result is an error such as:

- “device descriptor read/64, error -71”
- “device not accepting address”
- “unable to enumerate USB device”

Community discussions about the `MaxPower` field and about real-world host behavior reinforce that the relationship between requested current, actual current limiting, and user-visible failures is messy in practice. [S12]

### Type-C and USB Power Delivery (brief note)

USB Type-C is a connector and port system that can advertise higher current capabilities.

USB Power Delivery is a negotiation protocol that can raise voltage and power.

However, the key cyberdeck lesson is simple.

Do not assume that a Type-C connector guarantees high current.

A cable, port, or hub may only provide the default current.

Design your power budget as if you will only get the minimum until you have verified otherwise.

For authoritative details, refer to USB-IF’s published specification library for USB 2.0 and related documents. [S1]

---

## 55.1.6 Observing enumeration on Linux (dmesg, lsusb, usbmon)

Enumeration failures are easiest to debug when you can see what the host thinks happened.

Linux provides several tools for this.

### Kernel logs (dmesg)

The `dmesg` command prints the kernel ring buffer, which includes USB attach, reset, and enumeration messages. [S10]

In practice, you often run something like:

- `dmesg -w` to follow new messages as you plug and unplug devices.

If you see a repeated pattern of “new device,” “read descriptor,” and then an error, that is a strong signal that enumeration is failing early.

### Device listing (lsusb)

The `lsusb` utility lists USB buses and the devices connected to them.

Its manual page notes that verbose mode (`-v`) displays detailed descriptor information, including configuration descriptors. [S9]

This matters because enumeration is literally “reading descriptors.”

If `lsusb -v` cannot read descriptors, you are often dealing with a physical-layer or power problem rather than a driver problem.

### Bus tracing (usbmon)

When logs are not enough, you can trace USB traffic.

The Linux kernel’s `usbmon` facility collects traces of input/output requests on the USB bus, analogous to packet capture on a network. [S13]

Usbmon is not a “normal user” tool.

But for cyberdeck debugging, it is one of the most direct ways to see whether control transfers succeed or time out.

---

## 55.1.7 Common failure modes in cyberdecks (and why they look random)

Enumeration problems usually have one of three causes.

The bus is not powered correctly.

The signal quality is poor.

Or the device firmware does not respond correctly or quickly enough.

The confusing part is that these causes can produce the same symptom.

The device simply does not appear.

### Power instability

If your 5-volt rail sags when a device is plugged in, the device can reset in the middle of enumeration.

This can produce repeated “device descriptor read” errors.

Secondary troubleshooting guides aimed at administrators often recommend exactly the cyberdeck-relevant fixes: replace the cable, try a powered hub, and address power supply issues. [S14]

### Cable and connector issues

USB cables are transmission lines.

A “random” cable from a parts bin may have high resistance, poor shielding, or bad connectors.

In a cyberdeck enclosure, connectors may be mechanically stressed.

The Raspberry Pi forum thread centered on `device descriptor read/64, error -71` is a typical example: the builder tried multiple power supplies and adapters and still saw enumeration failure, illustrating how hard it can be to separate power and cabling from software symptoms. [S15]

### Hub edge cases

Internal hubs are common in cyberdecks.

A bus-powered hub shares a limited upstream power budget across all downstream ports.

Adding one high-current device can destabilize everything.

Even when a hub is powered, poor wiring can create ground bounce and voltage drop.

### Host controller and internal “hidden” USB devices

Some systems include internal USB devices such as webcams, Bluetooth radios, or touchpads.

The Arch Linux thread on error `-71` shows an important diagnostic point: you can see enumeration errors even when you believe you have “nothing connected,” because internal devices or ports can be failing. [S16]

### Firmware timing and correctness

Enumeration has timeouts.

If device firmware stalls, responds with malformed descriptors, or violates timing rules, the host may refuse to configure the device.

Microchip’s and Espressif’s documentation both frame enumeration as a state machine with required actions such as reset handling, descriptor reads, and configuration selection, reinforcing that enumeration is a real protocol, not a suggestion. [S6] [S17]

---

## 55.1.8 A practical debug workflow (what to do when a device will not enumerate)

The goal is to determine whether you have:

- a power problem,
- a physical connection problem,
- a hub/topology problem,
- or a firmware/driver problem.

A reliable workflow is:

1) **Reduce variables.** Test with one host port, one cable, one device.

2) **Test a known-good device on your port.** If even a simple keyboard fails, your port, hub, or power is suspect.

3) **Test your device on a known-good host.** If it fails everywhere, suspect the device.

4) **Prefer a powered hub for high-current devices.** This separates “data works” from “power is marginal.”

5) **Watch `dmesg -w` while plugging in.** If errors occur during descriptor reads or address assignment, suspect power integrity or signal integrity first.

6) **Use `lsusb` and `lsusb -v`.** If the device appears intermittently, record the VID and PID when it does.

7) **If you are developing firmware, validate descriptors.** A single wrong length field can break enumeration.

8) **If needed, capture traffic with usbmon.** This can show whether control transfers are timing out or returning malformed data.

---

## 55.1.9 Suggested figures

1) **Enumeration timeline diagram**: A sequence chart showing “attach → wait for power stable → reset → SET_ADDRESS → GET_DESCRIPTOR (device) → GET_DESCRIPTOR (configuration) → SET_CONFIGURATION → driver bind.” (Based on the USB 2.0 Chapter 9 excerpt in Microchip doc4290.) [S6]

2) **Descriptor hierarchy tree**: Device descriptor at the root; one or more configuration descriptors; under each, interface descriptors; under each, endpoint descriptors. (Matches the Linux USB guide and BeyondLogic description.) [S3] [S4]

3) **Annotated Linux log example**: A small excerpt of `dmesg` output with callouts for “new device,” “reset,” “read descriptor,” and “error -71.” (Use excerpts from your own system logs rather than copying from forums.)

4) **Power budget diagram**: Upstream port current budget → hub distribution → downstream devices; show where voltage drop across a cable can cause failures.

---

## 55.1.10 Practical takeaway

USB enumeration is a structured conversation.

The host resets the port, assigns an address, reads descriptors, chooses a configuration, and then loads software support.

If enumeration fails, treat it as a stage failure.

Look at logs.

Determine whether the failure happens during power-up, during descriptor reads, or during configuration.

In cyberdecks, the highest-probability causes are boring but fixable: unstable 5-volt power, bad cables, and hub topology.

---

## Sources

- [S1] USB Implementers Forum (USB-IF): *USB 2.0 Specification* (document library landing page; access point for standards) — https://usb.org/document-library/usb-20-specification
- [S2] BeyondLogic: *USB in a Nutshell — Chapter 6: USB Requests* (setup packets, request timing, SetAddress timing) — https://beyondlogic.org/usbnutshell/usb6.shtml
- [S3] BeyondLogic: *USB in a Nutshell — Chapter 5: USB Descriptors* (descriptor hierarchy; power fields in configuration descriptor) — https://beyondlogic.org/usbnutshell/usb5.shtml
- [S4] Linux USB (linux-usb.org): *Enumeration and Device Descriptors* (descriptor hierarchy; configuration/interface/endpoint definitions; SetConfiguration mention) — http://www.linux-usb.org/USB-guide/x75.html
- [S5] Microsoft Learn: *USB Descriptors* (GET_DESCRIPTOR; descriptors include VID/PID and endpoint info) — https://learn.microsoft.com/en-us/windows-hardware/drivers/usbcon/usb-descriptors
- [S6] Microchip / Atmel: *Implementing the USB Enumeration Process… (doc4290)* (enumeration steps; excerpt from USB 2.0 spec Chapter 9) — https://ww1.microchip.com/downloads/en/Appnotes/doc4290.pdf
- [S7] FTDI: *TN_113 — Simplified Description of USB Device Enumeration* (vendor overview of enumeration mechanics) — https://www.ftdichip.com/Support/Documents/TechnicalNotes/TN_113_Simplified%20Description%20of%20USB%20Device%20Enumeration.pdf
- [S8] Microsoft Learn: *USB Configuration Descriptors* (MaxPower and bus-powered/self-powered fields; configuration selection) — https://learn.microsoft.com/en-us/windows-hardware/drivers/usbcon/usb-configuration-descriptors
- [S9] usbutils (man7.org): *lsusb(8)* (lists devices; verbose shows configuration descriptors; references VID/PID mapping) — https://man7.org/linux/man-pages/man8/lsusb.8.html
- [S10] util-linux (man7.org): *dmesg(1)* (kernel ring buffer access for USB enumeration logs) — https://man7.org/linux/man-pages/man1/dmesg.1.html
- [S11] Dangerous Prototypes: *Designing USB Devices for proper current and MaxPower* (practitioner explanation of current draw rules and descriptor meaning) — http://www.dangerousprototypes.com/docs/Designing_USB_Devices_for_proper_current_and_MaxPower
- [S12] EEVblog forum: *What does a PC actually do with USB descriptor max current value?* (community discussion of MaxPower behavior, inrush, and host practice) — https://www.eevblog.com/forum/projects/what-does-a-pc-actually-do-with-usb-descriptor-max-current-value/
- [S13] Linux kernel documentation: *usbmon* (USB bus tracing facility) — https://www.kernel.org/doc/html/latest/usb/usbmon.html
- [S14] Bobcares: *Fixing Device Descriptor Read/64 Error 71* (secondary troubleshooting summary; common root causes and mitigations) — https://bobcares.com/blog/device-descriptor-read-64-error-71/
- [S15] Raspberry Pi Forums: *Error: usb 1-1 device descriptor read/64, error -71* (real-world case discussion; illustrates how enumeration failures surface in logs) — https://forums.raspberrypi.com/viewtopic.php?t=345108
- [S16] Arch Linux Forums: *[Solved] usb 1-4: device descriptor read/64, error -71* (real-world case; illustrates internal devices/ports and hardware suspicion) — https://bbs.archlinux.org/viewtopic.php?id=282613
- [S17] Espressif Systems: *USB Host Enumeration Driver (Enum)* (enumeration stages and requirements; embedded-host perspective) — https://docs.espressif.com/projects/esp-idf/en/stable/esp32s2/api-reference/peripherals/usb_host/usb_host_notes_enum.html

Additional textbook reference (print):

- Jan Axelson: *USB Complete: The Developer’s Guide* (Lakeview Research). (Widely used developer textbook; useful for deeper descriptor and enumeration details.)
