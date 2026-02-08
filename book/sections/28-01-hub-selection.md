# 28.1 Hub selection

A cyberdeck is rarely a single-device computer.

It is usually an ecosystem in a box:

- one compute module,
- multiple peripherals,
- and multiple cables and power domains.

A Universal Serial Bus (USB) hub is often the simplest way to connect those peripherals.

A hub lets one upstream connection behave like several downstream ports.

That convenience is not free.

A hub becomes part of your deck’s:

- power distribution,
- stability and boot reliability,
- thermal design,
- and mechanical layout.

This chapter explains how to choose a hub for a cyberdeck.

It focuses on four required topics:

- power delivery,
- stability,
- thermal behavior,
- and mounting.

---

## 28.1.1 What a USB hub is (in plain terms)

A USB hub is a device that sits between a host and multiple devices.

It has:

- an upstream-facing port that connects to the host,
- and multiple downstream-facing ports that connect to devices.

USB-IF’s base specifications (for example USB 2.0 and USB 3.2) provide the formal definition of hub behavior and constraints. [U1][U2]

Cyberdeck implication:

- the hub is not a passive splitter.

It is active electronics that must be powered and must remain stable under load.

---

## 28.1.2 Power delivery: the most common reason hubs fail in cyberdecks

Many “hub problems” are actually power problems.

A peripheral may reset, disconnect, or enumerate unreliably because:

- the hub cannot supply current,
- the upstream supply droops,
- or the hub’s power management reacts to a transient.

### Bus-powered versus self-powered hubs

A **bus-powered hub** is powered from the upstream USB connection.

A **self-powered hub** has its own power input and can supply more current per port.

USB power rules and USB Power Delivery specifications define the power negotiation and limits.

USB-IF publishes USB Power Delivery documentation as a foundational reference for these rules. [U3]

Cyberdeck implication:

- if your deck has high-current peripherals (storage, radios, displays), you often need a self-powered hub.

### Type-C and power negotiation

Many modern cyberdecks use USB Type-C connectors.

Type-C changes connector behavior and introduces explicit cable and connector requirements.

USB-IF’s Type-C cable and connector specification defines these constraints and the physical/electrical expectations for cable assemblies. [U4]

Cyberdeck implication:

- “it plugs in” does not mean “it will deliver stable power.”

### Per-port power switching and over-current behavior

A hub that can switch and monitor power per port is easier to debug.

If one device shorts or misbehaves, you want that port to be isolated.

Texas Instruments’ TUSB8041 hub documentation emphasizes hub-side power control and related behavior (including charging-related modes) as part of the hub’s feature set. [U5]

Cyberdeck implication:

- per-port power switching and over-current reporting are stability features, not luxuries. [U5]

### Dedicated hub power management

In compact decks, it can be useful to separate:

- the hub controller,
- and the hub power management.

Texas Instruments’ TPS2149 is an example of a hub-oriented power management device with integrated functions intended for bus-powered hub designs. [U6]

Cyberdeck implication:

- a dedicated power-management integrated circuit can reduce risk by providing controlled switching and protection in a tight enclosure. [U6]

---

## 28.1.3 Stability: enumeration, suspend, and “flaky” behavior

A device that “sometimes works” is often power- or timing-marginal.

### Enumeration is a timing process

When a USB device is connected, the host enumerates it.

Enumeration includes:

- detecting the device,
- resetting it,
- assigning an address,
- and reading descriptors.

Microsoft’s USB blog describes the enumeration process at a high level, which is useful because it highlights how many steps must succeed for a device to appear stable. [U9]

Cyberdeck implication:

- a hub that is marginal on reset timing or power droop can manifest as intermittent enumeration failures. [U9]

### Suspend and power management

Many systems use USB power management to save energy.

This includes autosuspend and selective suspend.

Linux documents USB power management behavior, which is relevant because suspend and resume can expose hub and device quirks. [U7]

Microsoft documents USB selective suspend as a designed feature to suspend idle ports individually without disrupting other activity. [U8]

Cyberdeck implication:

- validate suspend and resume behavior under your actual device mix, not only “plug in and it works.” [U7][U8]

---

## 28.1.4 Thermal behavior: hubs are small power distribution systems

A hub dissipates power in multiple places:

- the hub controller,
- voltage regulators,
- per-port power switches,
- and cables/connectors.

Heat can cause:

- instability,
- derating,
- and long-term reliability problems.

### Why thermal issues happen in cyberdecks

Cyberdeck enclosures are often:

- small,
- sealed,
- and made of plastic.

This reduces heat dissipation.

### PCB layout affects thermal behavior

Many thermal problems are layout problems.

Copper area, via placement, and current path impedance affect heating.

Analog Devices’ application note on PCB layout for switching supplies provides a reference point for why compact power distribution must be treated as a layout discipline. [U10]

Cyberdeck implication:

- hub stability and hub temperature are related.

### Thermal compliance is becoming explicit

USB-IF publishes thermal compliance test specifications for some cable classes.

Even if you do not test against these specifications, their existence is a signal: thermal behavior is a first-class concern in modern USB ecosystems. [U11]

Cyberdeck implication:

- if your deck relies on dense cabling and power delivery, thermal headroom is part of selection.

> **Figure idea:** A thermal map sketch of a hub board showing hotspots: regulators, power switches, controller.

---

## 28.1.5 Mounting: a hub is also a mechanical subsystem

Mounting problems create electrical problems.

If a connector is stressed, it can intermittently disconnect.

If a cable rubs or bends, it can fail.

### Strain relief and connector access

A cyberdeck hub should be mounted so that:

- port connectors cannot flex the printed circuit board,
- cables are strain-relieved,
- and insertion forces are taken by the enclosure.

Cyberdeck implication:

- the enclosure should carry connector load.

The hub board should not.

### Serviceability

Hubs fail.

Cables fail.

Design so that:

- you can replace the hub,
- and you can access the ports.

---

## 28.1.6 Community constraints: what builders run into

Community cyberdeck logs repeatedly show practical constraints:

- physical fit in tight cases,
- powered hub choices to avoid brownouts,
- and problems caused by back-powering or unexpected current paths.

Hackaday.io cyberdeck build details illustrate that internal hubs are often chosen for physical packaging reasons and then adapted to the enclosure. [U12]

Reddit cyberdeck build updates similarly highlight that power headroom and physical integration are recurring selection drivers. [U13][U14]

These are not controlled engineering tests.

But they are valuable because they reveal how hubs fail in practice.

---

## 28.1.7 A bring-up checklist

Before committing to a hub in a cyberdeck:

1) Validate power.

- Test with your maximum peripheral load.
- Watch for resets and dropouts.

2) Validate enumeration.

- Cold boot.
- Hot plug.
- Multiple devices simultaneously.

Enumeration guidance helps interpret failures as timing processes, not mysteries. [U9]

3) Validate suspend and resume.

- Test with autosuspend and selective suspend enabled.

Use OS documentation to understand expected behavior. [U7][U8]

4) Validate thermal behavior.

- Run sustained loads.
- Measure temperatures.
- Check for throttling or instability.

Treat layout and current paths as thermal drivers. [U10]

5) Validate mounting.

- Shake test.
- Cable strain.
- Repeated insertion.

---

## 28.1.8 Practical takeaway

Hub selection is cyberdeck systems engineering.

A “good” hub is not only fast.

It is a hub that:

- supplies stable power under worst-case load, [U3][U5]
- enumerates reliably across boots and hot-plug events, [U9]
- survives suspend/resume behavior on your target operating system, [U7][U8]
- dissipates heat safely in your enclosure, [U10][U11]
- and can be mounted so connectors do not become failure points. [U12]

If you treat the hub as a power distribution and mechanical part, you avoid most of the “USB is flaky” problems.

---

## Sources

- [U1] USB-IF: USB 2.0 specification — https://www.usb.org/document-library/usb-20-specification
- [U2] USB-IF: USB 3.2 Revision 1.1 — https://www.usb.org/document-library/usb-32-revision-11-june-2022
- [U3] USB-IF: USB Power Delivery — https://www.usb.org/document-library/usb-power-delivery
- [U4] USB-IF: USB Type-C cable and connector specification (Release 2.4) — https://www.usb.org/document-library/usb-type-cr-cable-and-connector-specification-release-24
- [U5] Texas Instruments: TUSB8041 (USB 3.0 hub) — https://www.ti.com/product/TUSB8041
- [U6] Texas Instruments: TPS2149 (hub power management) — https://www.ti.com/product/TPS2149
- [U7] Linux kernel docs: USB power management — https://docs.kernel.org/driver-api/usb/power-management.html
- [U8] Microsoft: USB selective suspend — https://learn.microsoft.com/en-us/windows-hardware/drivers/usbcon/usb-selective-suspend
- [U9] Microsoft USB blog: how the USB stack enumerates a device — https://techcommunity.microsoft.com/t5/microsoft-usb-blog/how-does-usb-stack-enumerate-a-device/ba-p/270685
- [U10] Analog Devices: AN-136 (PCB layout for switching power supplies) — https://www.analog.com/en/resources/app-notes/an-136.html
- [U11] USB-IF: USB active cable thermal compliance test specification — https://www.usb.org/document-library/usb-active-cable-thermal-compliance-test-specification
- [U12] Hackaday.io: Cyberdeck Red (internal build details) — https://hackaday.io/project/187494-cyberdeck-red/details
- [U13] r/cyberDeck: offline cyberdeck build update — https://www.reddit.com/r/cyberDeck/comments/1owzzs3
- [U14] r/cyberDeck: Nanuk 904 cyberdeck build — https://www.reddit.com/r/cyberDeck/comments/17npxtx
