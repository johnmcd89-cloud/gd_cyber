# 81.2 Keyboard/HID devices

A cyberdeck can have excellent radios, storage, and power engineering and still feel unusable if the input devices are fragile, ambiguous, or difficult to update. Keyboards, keypads, knobs, and pointing devices are the operator’s primary control surface. They are also one of the easiest subsystems to “customize into a corner,” where only the original builder can maintain the device.

This chapter explains how input devices are represented to an operating system, how custom cyberdeck input devices fit into that model, and how to design macro layers and firmware update workflows that remain robust over time.

The emphasis is on **legitimate**, **operator-safe**, and **maintainable** workflows.

---

## 81.2.1 Terms and mental models

A keyboard is both an electrical device and a logical protocol device.

Electrically, most keyboards are built from a **switch matrix**, meaning rows and columns of conductive traces with a switch at each intersection. A microcontroller scans the matrix to decide which switches are pressed.

Logically, the host computer does not care about your matrix. The host cares about **events**, such as “a key was pressed,” “a knob was turned,” or “the pointer moved.”

The most common way for a custom cyberdeck input device to present itself to a host is as a Universal Serial Bus (USB) **Human Interface Device (HID)**. HID is a USB device class intended for keyboards, mice, game controllers, and similar peripherals. The HID class definition is designed to be compact, extensible, and robust, and it specifies how host-side software extracts data from devices. [S1]

A useful design rule is therefore:

> Treat your input device as an event generator with a well-defined contract, not as “a bunch of switches.”

That contract should be stable across operating systems, across sleep and wake cycles, and across firmware revisions.

### Suggested figure

**Figure 81.2-A: Cyberdeck input stack (from switch to shell).**

```text
[Keys/Encoders/Trackball]
          |
      (scan/ADC)
          v
     [MCU Firmware]
  (debounce, layers,
   macros, HID reports)
     |           |
  USB HID    BLE HID (Bluetooth Low Energy)
     |           |
     v           v
[Host HID driver stack]
          |
   [Keymap/daemon?]
 (kmonad/kanata/etc.)
          |
      [Apps/TMUX]
```

This diagram is useful because it shows where a “macro” can exist: in the device firmware, in the host operating system configuration, or in a host-side daemon.

---

## 81.2.2 What the host actually sees: reports, usages, and keymaps

### HID reports and report descriptors

HID devices send **reports**, which are short, structured messages describing the current state of controls (for example, which keys are down, or how far a pointer moved since the last report).

Every HID device also provides a **report descriptor**, a compact data structure that tells the host what fields to expect in its reports.

This is why two devices can both be “HID” but behave differently: their report descriptors describe different sets of controls.

If you design a custom input device, the report descriptor is part of your public interface. Changing it can break host-side scripts and assumptions. In practice, many “mysterious” input bugs are actually interface contract bugs.

### Usage codes: naming what a control means

In HID, a **usage** is a standardized identifier for the semantic meaning of a control (for example, a keyboard key, a consumer-media control like volume up, or a system control like sleep). The HID Usage Tables are the canonical mapping from usages to meaning. [S2]

For cyberdecks, this matters because it is the difference between:

- A volume knob that works as a volume control across systems, and
- A volume knob that emits an arbitrary keystroke sequence that only works on your preferred desktop environment.

When possible, prefer standardized HID usages over ad-hoc macro keystrokes.

### Scan codes, keycodes, and keymaps (the “translation chain”)

The same physical key can be described in multiple ways as it moves through the system.

- A **scan code** is a low-level identifier produced by the device firmware when a switch position is detected.
- A **keycode** (in firmware) is a symbolic mapping from scan codes to a logical meaning (for example, “this position is Escape”).
- A **keymap** (on the host) is the operating system’s mapping from key events to characters, shortcuts, and application actions.

The practical lesson is that you can “fix” a key in at least two places: in device firmware or on the host. Firmware fixes travel with the device; host fixes travel with the computer.

---

## 81.2.3 Switch matrices, ghosting, and why diodes show up in keyboard builds

A switch matrix is an efficient way to connect many keys to a small microcontroller. Instead of having one microcontroller pin per key, a matrix uses a grid. The microcontroller activates one row at a time and reads the columns (or vice versa).

However, matrices have a classic failure mode called **ghosting**, where pressing some combinations of keys can create a phantom keypress that is not actually pressed. This is especially relevant for compact cyberdeck keyboards that aggressively reuse a small matrix.

A common mitigation is to add a diode per switch so current can only flow in one direction through each switch position. This prevents some phantom paths and enables more reliable multi-key combinations.

### Suggested figure

**Figure 81.2-B: Matrix scanning and diodes (why ghosting happens).**

```text
   Col0   Col1   Col2
R0  o--|<--o--|<--o
R1  o--|<--o--|<--o
R2  o--|<--o--|<--o

Scan loop:
- Drive R0, read Cols
- Drive R1, read Cols
- Drive R2, read Cols

Inset: ghost example without diodes (3-key chord => phantom 4th)
```

---

## 81.2.4 Custom input devices for cyberdecks

Cyberdecks often benefit from input devices that are not “a normal laptop keyboard.” The reasons include enclosure constraints, glove use, field repairability, and the desire to dedicate controls to specific tasks.

A cyberdeck builder should think in terms of *roles* rather than parts: a primary text entry device, a primary pointing device, and a small set of dedicated controls for high-frequency tasks.

### Hardware categories

Compact keyboards, split keyboards, macropads, rotary encoders, and trackballs can all present as HID devices. When they do, the host sees the same kind of contract: reports with usages. That shared interface is what makes mixing-and-matching possible.

### Transport options

Input devices can connect to the compute platform in multiple ways. USB HID is usually the default because it is simple and tends to work even in early boot environments.

A useful planning tool is a short comparison table.

**Table 81.2-A: Transport options for cyberdeck input devices.**

```text
| Transport              | Latency | Power source  | Works in firmware menus* | Complexity | Notes |
|-----------------------|---------|---------------|---------------------------|------------|-------|
| USB HID               | low     | host-powered  | often yes                 | low        | Best default for most decks |
| BLE HID (Bluetooth LE)| medium  | device battery| often no                  | medium     | Pairing UX becomes part of your UI |
| 2.4 GHz dongle        | low     | device battery| sometimes                 | high       | Proprietary stacks are common |
| UART → host daemon    | low     | host-powered  | no                        | medium     | Great for internal “front panel” controllers |

* Many people call these “BIOS” menus; more generally they are firmware configuration interfaces.
```

---

## 81.2.5 Macro layers: capability without fragility

Macro layers are a way to map the same physical controls to different meanings depending on mode.

A **layer** is a mapping table that changes what a key does. A layer might be activated momentarily (while a key is held), toggled (latched on), or set as the default mode.

### Why layers matter in cyberdecks

Cyberdecks often have fewer physical keys than a full keyboard, but they often need more functions: radio tuning, logging markers, map control, camera switching, and device management. Layers let you keep the device compact without turning it into an unmemorable maze.

### Tap-hold behaviors and timing

Many firmware stacks support “tap-hold,” where a key acts as one command when tapped and as a modifier or layer switch when held. This can be ergonomic and space-efficient, but it is also a reliability risk when the operator is stressed, wearing gloves, or working in a moving environment.

Treat timing parameters as part of your user interface. If a key frequently triggers the “hold” behavior when the user intended a tap, you have effectively created accidental commands.

### Suggested figure

**Figure 81.2-C: Layer model (momentary, toggle, one-shot) with tap-hold.**

```text
Layers (top wins):
L2 NAV    (active while held)
L1 FN     (toggle or momentary)
L0 BASE   (default)

Example behaviors:
- Space (tap)  => " "
- Space (hold) => momentary layer L1
- Caps         => one-shot Control
```

### Safety and auditability

Macros and layers can do destructive things quickly. On a cyberdeck, the operator may be tired, distracted, or working in poor conditions. Macro design is therefore a safety problem, not just a productivity feature.

A conservative posture is to keep irreversible actions off single keystrokes and to make “mode” visible.

### Where to run macros: firmware vs host

Macros can run inside the input device firmware or on the host.

Firmware macros are portable and work on almost any host, but they are less context-aware and can be harder to audit if multiple firmware revisions exist in the field.

Host-side macro daemons are easier to log and can be context-aware (for example, “if the radio application is focused, this key does X”), but they only work on hosts where the daemon is installed and functioning.

### Suggested figure

**Figure 81.2-D: Macro execution boundary (firmware vs host).**

```text
Keypress
  |
  +--> Firmware macro: emits HID sequence
  |      Pros: portable; works offline
  |      Cons: limited context; update discipline matters
  |
  +--> Host macro daemon: receives key event, runs script
         Pros: context-aware; powerful; easy logs
         Cons: host-dependent; requires daemon running
```

---

## 81.2.6 “More than keys”: composite HID devices and control families

A cyberdeck control surface is often more than a keyboard. It may include volume, brightness, sleep, display toggles, or a dedicated knob that should behave consistently.

A single physical device can expose multiple HID interfaces as a **composite device**. For example, one interface can act as a keyboard, another as “consumer control” (media keys), and a third as a vendor-defined interface for custom features.

This design is powerful, but it also increases the importance of interface stability. In particular, some environments only support a subset of HID features.

### Suggested figure

**Figure 81.2-E: HID report “family portrait” (keyboard + consumer + vendor-defined).**

```text
USB Composite Device
- Interface 0: HID Keyboard (boot protocol)
  Report: [Modifiers][Reserved][Key1..Key6]
- Interface 1: HID Consumer Control
  Report: [UsageID (e.g., VolumeUp)]
- Interface 2: HID Vendor-defined
  Report: [cmd][payload...]

Callouts:
- 6-key rollover vs N-key rollover
- Boot protocol fallback
```

A brief note on rollover: **6-key rollover** means the device can report up to six simultaneous non-modifier keys in a standard report format; **N-key rollover** means it can report an arbitrary number of simultaneous keys. Both can be valid choices depending on your goals.

---

## 81.2.7 Robust firmware updates (do not brick your input devices)

Custom input devices contain firmware. Firmware is the software that runs on the microcontroller inside the keyboard or controller.

If you cannot update that firmware reliably, you do not have a product; you have a demo.

### Why firmware updates go wrong

Firmware updates fail for predictable reasons.

They fail because power is interrupted mid-update. They fail because the wrong image is flashed. They fail because bootloader mode is undocumented. They fail because a new build changes the HID report descriptor and breaks a host-side configuration.

The cyberdeck-specific hazard is that a cyberdeck may have only one convenient input device. If that device is bricked, the deck becomes hard to recover.

### Bootloaders, recovery modes, and update formats

A **bootloader** is a small program that starts before the main firmware and can load it. Many designs use a bootloader to accept firmware updates.

Two widely used approaches are:

- **UF2**, a file format intended for drag-and-drop updates over USB mass storage. UF2 is designed for robustness against out-of-order writes and stray filesystem traffic from the host. [S3]
- **USB Device Firmware Upgrade (DFU)**, a USB class specification for downloading firmware while a device is in DFU mode. [S4]

### Resilient update patterns

If your deck is meant to be maintained over time, consider a “dual-bank” update pattern where a known-good firmware can be retained while testing a new firmware. In higher-assurance systems, firmware images can also be signed and verified before boot.

### Suggested figure

**Figure 81.2-F: Robust firmware updates (dual-bank + recovery).**

```text
[Bootloader ROM/Protected]
   | verify integrity (and optionally signature)
   v
[Slot A firmware] <---- rollback if boot fails
[Slot B firmware]

Recovery:
- Hold BOOT key => DFU/UF2 mode
```

### Practical “don’t brick it” procedures

For cyberdeck maintenance, prefer procedures that are repeatable and low-risk.

Keep one known-good firmware release tagged in version control. Label the device physically with its firmware family and update method. Test updates with the deck on external power. If the deck depends on the device for input, keep a fallback input path (for example, a basic USB keyboard tucked into the deck).

---

## 81.2.8 Firmware ecosystems (high level): QMK, ZMK, and related stacks

The keyboard community has produced mature firmware ecosystems.

QMK describes itself as an open source community centered around developing computer input devices, with firmware and tooling widely used in programmable keyboards. [S5]

ZMK is an open source keyboard firmware built on the Zephyr real-time operating system, with an explicit emphasis on power efficiency and support for wireless input devices. [S6]

For cyberdecks, the practical choice is not “which is best.” The choice is “which aligns with my constraints”: wired versus wireless, power consumption, update method, and how much custom logic you want to carry.

Some builders also use higher-level or more experimental stacks (for example, Python-oriented firmware), or implement their own HID device using libraries such as TinyUSB. TinyUSB is commonly used as a building block for custom USB device firmware. [S10]

### Suggested table

**Table 81.2-B: Firmware ecosystems for custom HID devices (comparison sketch).**

```text
| Stack               | Best for         | Transport focus | Macro/layers | Update story         | Notes |
|--------------------|------------------|-----------------|--------------|----------------------|------|
| QMK                | wired keyboards  | USB HID         | very rich    | bootloader/DFU varies| large ecosystem |
| ZMK                | low-power/wireless| BLE + USB       | rich         | board-dependent      | power-aware design |
| TinyUSB + custom   | bespoke devices  | USB (and some BLE) | depends   | you design it        | maximum control, maximum responsibility |
```

---

## 81.2.9 Host-side integration and troubleshooting

### “Is the device alive?”

The first diagnostic question is whether the input device enumerates on the bus.

On Linux, the kernel input subsystem documentation provides useful orientation because it explains how device drivers generate events and how event interfaces such as evdev expose them to user space. [S8]

If the device enumerates but behaves incorrectly, check whether the device is in the expected layer, whether it is emitting standardized HID usages, and whether the host is applying remaps.

### Stable behavior across sleep and wake

A cyberdeck often suspends, wakes, browns out, or hot-plugs devices in the field. Input devices should be tested under those transitions.

If your platform has unreliable sleep states, consider designing the deck so that the input device remains powered (or resets cleanly) during suspend and resume.

### Suggested figure

**Figure 81.2-G: Debugging flowchart (“my keyboard is weird”).**

```text
Start
 |
 |-- Keys not detected at all?
 |     |-- USB enumeration OK? (device shows up)
 |     |-- Enter bootloader? (DFU/UF2)
 |
 |-- Wrong keys / ghosting?
 |     |-- Diodes orientation? matrix wiring?
 |     |-- Firmware matrix config rows/cols correct?
 |
 |-- Macros misfire?
 |     |-- Layer state stuck? tap-hold timing?
 |     |-- Host daemon conflict?
 |
End: known-good firmware + factory reset + reflash
```

---

## Sources

Community and culture sources:

- [S7] Hackaday: *cyberdeck* tag index (examples of unconventional input devices and front panels in build culture) — https://hackaday.com/tag/cyberdeck/

Technical and standards references:

- [S1] USB Implementers Forum (USB-IF): *Device Class Definition for HID 1.11* (HID device model; descriptors and report extraction goals) — https://www.usb.org/sites/default/files/hid1_11.pdf
- [S2] USB Implementers Forum (USB-IF): *HID Usage Tables 1.7* (standardized usage codes for controls; semantic mapping for keyboards and consumer controls) — https://www.usb.org/sites/default/files/hut1_7.pdf
- [S3] Microsoft: *USB Flashing Format (UF2) specification* (drag-and-drop firmware update format for microcontrollers via mass storage) — https://github.com/microsoft/uf2
- [S4] USB Implementers Forum (USB-IF): *Device Firmware Upgrade Specification 1.1* (DFU device class for firmware download in bootloader mode) — https://www.usb.org/sites/default/files/DFU_1.1.pdf
- [S5] QMK: *QMK Firmware documentation* (programmable input devices; keymaps, layers, and firmware tooling) — https://docs.qmk.fm/
- [S6] ZMK: *ZMK Firmware documentation* (modern keyboard firmware emphasizing power efficiency; layers and behaviors) — https://zmk.dev/docs
- [S8] Linux Kernel Documentation: *Input subsystem introduction* (Linux input architecture; event handlers like evdev) — https://www.kernel.org/doc/html/latest/input/input.html
- [S10] TinyUSB: *TinyUSB repository* (USB device stack used in custom microcontroller firmware) — https://github.com/hathach/tinyusb

Additional textbook references (background):

- Horowitz and Hill, *The Art of Electronics* (switch interfacing and debouncing; robust digital input design).
