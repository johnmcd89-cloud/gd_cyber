# 20.1 Phone-as-CPU topology

A “phone-as-CPU” cyberdeck uses a smartphone (or tablet) as the primary computer.

The deck then provides the things that phones are not optimized for:

- a better keyboard,
- a larger or more ergonomic display,
- external ports and accessories,
- and a stable power system.

This topology is attractive because modern phones offer impressive computing performance and excellent power efficiency.

It is also difficult in ways that are not obvious to first-time builders, because phones are designed around:

- a single multi-purpose port,
- tightly controlled power negotiation,
- and operating systems with strong security boundaries.

This chapter describes the common topologies and the engineering constraints behind them.

---

## 20.1.1 What “topology” means in this context

In this chapter, “topology” means:

- **how the phone connects to the deck’s peripherals**, and
- **how power flows while those peripherals are connected**.

A phone-as-CPU deck is usually built around **Universal Serial Bus Type‑C (USB‑C)**.

USB‑C is a physical connector standard designed for a wide range of devices and roles, which is why it appears on phones, tablets, and laptops. [U1]

The promise is simple:

- one connector can carry data, display output, and power.

The implementation details are not simple.

---

## 20.1.2 The baseline architecture: “dock the phone”

The baseline topology looks like this:

- phone (USB‑C) → dock (or multiport adapter) → external display + keyboard + mouse + storage

At a practical level, the dock is doing three jobs:

1. **display output adaptation** (often USB‑C to HDMI),
2. **USB expansion** (a hub for keyboards, drives, and Ethernet),
3. **charging pass-through** (so the phone does not run its battery down while acting as a computer).

> **Figure idea:** A block diagram with arrows showing: phone USB‑C to dock; dock to HDMI display; dock to USB keyboard/mouse; dock to USB‑C charger.

---

## 20.1.3 Power pass-through: why USB Power Delivery matters

Without charging, a phone-as-CPU deck is usually a short-lived demo.

To be a useful computer, the phone must be able to:

- power some attached devices,
- *and* charge itself,
- while negotiating roles correctly.

That is enabled by **USB Power Delivery (USB PD)**.

USB‑IF (the USB Implementers Forum) describes USB Power Delivery as part of the USB charging ecosystem, where devices negotiate power roles and higher power levels over USB‑C. [U2]

The USB PD 3.1 developer update emphasizes that the ecosystem has evolved to support higher power levels (including up to 240 watts) for devices that need it. [U3]

In cyberdeck terms, the key lesson is:

- “USB‑C power” is negotiated, not assumed.

A dock that supports charging pass-through is doing active role negotiation, and not all adapters do it correctly.

---

## 20.1.4 External display: DisplayPort Alternate Mode (and the trade-offs)

Many people assume USB‑C “just outputs video.”

In practice, video output over USB‑C is commonly implemented using **DisplayPort Alternate Mode**.

VESA (the Video Electronics Standards Association) describes how DisplayPort can be carried over the USB‑C connector using Alternate Mode. [U4]

A crucial physical constraint is lane sharing.

DisplayPort Alt Mode can repurpose high-speed lanes that would otherwise carry USB 3 data, which explains a common cyberdeck surprise:

- some docks give you high-resolution display output,
- but reduce simultaneous USB data performance,

because the available lanes are allocated differently.

VESA’s updated DisplayPort Alt Mode specifications aligned with USB4 increase display headroom, but they do not remove the need to think about lane allocation and dock capabilities. [U5]

> **Figure idea:** A lane allocation diagram: “four high-speed lanes” split between display lanes and USB 3 lanes in different configurations.

---

## 20.1.5 Keyboard and pointer input: wired versus wireless

Keyboard and pointer input can be delivered by:

- USB (wired),
- Bluetooth (wireless),
- or a mix.

The deck-builder takeaway is that input devices are rarely the hard part.

The hard part is keeping the phone both:

- connected to input devices,
- and charging,

without causing the phone to drop into the wrong USB role.

---

## 20.1.6 Android topologies: USB host mode, USB accessory mode, and desktop modes

Android devices support multiple USB roles.

Android developer documentation distinguishes:

- **USB host mode**, where the Android device acts as the host and enumerates attached peripherals, [U6]
- and **USB accessory mode**, where an external accessory takes the host role and the Android device behaves as a peripheral. [U7]

For cyberdecks, host mode is often what you want:

- phone as the “computer,” peripherals as “devices.”

But a dock can blur this boundary.

A reliable design therefore assumes:

- different phones behave differently,
- and a working dock setup is a compatibility outcome, not a guarantee.

### Samsung DeX as a concrete reference design

Samsung DeX is an example of an Android “desktop mode” ecosystem.

Samsung’s documentation and guidance describe external display usage, and common workflows where users connect keyboards and mice (Bluetooth or USB through adapters) while also charging through multiport adapters. [U8][U9]

This is valuable even if you do not use DeX specifically, because it shows a proven pattern:

- external display + hubbed input + charging pass-through on one cable.

---

## 20.1.7 iPad (and iPhone) topologies: strong hardware, different constraints

Apple’s tablet ecosystem also uses USB‑C on many models.

Apple support documentation describes using the USB‑C port on iPad to charge and connect accessories, which includes connecting through hubs and adapters and charging while connected. [U10]

However, capabilities vary by model and operating system version.

A safe cyberdeck builder mindset is:

- do not assume “USB‑C on the device” implies “desktop-like docking.”

You must verify:

- external display behavior (mirroring versus extended display),
- output resolution limits,
- and accessory compatibility.

---

## 20.1.8 Failure modes that commonly surprise builders

### 1) “It outputs video, but my USB 3 devices are slow”

Lane allocation in DisplayPort Alt Mode can trade USB 3 bandwidth for display bandwidth. [U4]

### 2) “It charges sometimes, but not when everything is plugged in”

USB PD role negotiation and adapter quality determine whether charging pass-through works under load. [U2][U3]

### 3) “My phone won’t go into desktop mode”

Desktop modes are device- and vendor-dependent.

Even within Android, desktop behavior varies widely.

### 4) “The deck works on one phone but not another”

USB role behavior, DisplayPort Alt Mode support, and dock firmware compatibility vary by device.

This is why a selection checklist matters.

---

## 20.1.9 Selection checklist (what to verify before you build)

1. **Does the device support external display over USB‑C?** [U4]
2. **Can it charge while using a dock (USB PD pass-through)?** [U2]
3. **Can it use a USB hub reliably in host mode?** [U6]
4. **Does your desired workflow require a desktop mode (DeX or similar)?** [U8]
5. **Do you have at least one known-good dock/adapter model tested with your device?**

> **Figure idea:** A one-page “dock compatibility matrix” table: device model × dock model × display works × USB works × charging works.

---

## 20.1.10 Practical takeaway

Phone-as-CPU cyberdecks are real and increasingly practical.

They leverage:

- USB‑C as a unified physical interface, [U1]
- USB PD for negotiated, bidirectional power, [U2][U3]
- and DisplayPort Alt Mode to drive external displays. [U4][U5]

Success depends on treating the dock as part of the platform.

A phone-as-CPU deck is not “one cable.” It is a negotiated system.

Community projects show the approach is active and evolving, not just theoretical. [U11][U12]

---

## Sources

- [U1] USB‑IF: USB Type‑C® Cable and Connector Specification (overview) — https://www.usb.org/usb-type-cr-cable-and-connector-specification
- [U2] USB‑IF: USB Charger (USB Power Delivery overview) — https://www.usb.org/usb-charger-pd
- [U3] USB Promoter Group: USB PD 3.1 Developer Update (PDF; higher power levels including 240 W class) — https://www.usb.org/sites/default/files/2021-05/USB%20PG%20USB%20PD%203.1%20DevUpdate%20Announcement_FINAL.pdf
- [U4] VESA: DisplayPort Alt Mode on USB Type‑C (lane repurposing and transport concept) — https://vesa.org/featured-articles/vesa-brings-displayport-to-new-usb-type-c-connector/
- [U5] VESA: Updated DisplayPort Alt Mode spec for USB4 / USB‑C — https://vesa.org/featured-articles/vesa-releases-updated-displayport-alt-mode-spec-to-bring-displayport-2-0-performance-to-usb4-and-new-usb-type-c-devices/
- [U6] Android Developers: USB host overview — https://developer.android.com/develop/connectivity/usb/host
- [U7] Android Developers: USB accessory overview — https://developer.android.com/develop/connectivity/usb/accessory
- [U8] Samsung Business Insights: Beginner’s guide to Samsung DeX — https://insights.samsung.com/2024/08/26/the-beginners-guide-to-samsung-dex-13/
- [U9] Samsung Support: How to use a Samsung USB‑C to HDMI adapter — https://www.samsung.com/ae/support/mobile-devices/how-to-use-a-samsung-usb-c-to-hdmi-adapter/
- [U10] Apple Support: Charge and connect with the USB‑C port on your iPad — https://support.apple.com/en-us/108894
- [U11] Hackaday: SPACEdeck (phone-based cyberdeck example) — https://hackaday.com/2025/06/05/spacedeck-is-half-cyberdeck-half-phone-case-all-style/
- [U12] Hackaday.io: Mobile C‑deck project (phone-as-compute build culture) — https://hackaday.io/project/203116-mobile-c-deck
