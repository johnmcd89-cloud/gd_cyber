# 25.5 Firmware remapping (high-level)

A cyberdeck often has a nonstandard keyboard.

Even when you use a conventional keyboard, you may want it to behave differently from the factory default.

You might want:

- a different placement of punctuation,
- navigation keys closer to the home row,
- one key to behave as two keys depending on context,
- or a single key to trigger a longer, repeatable action.

All of these are forms of **remapping**.

Remapping means redefining what a physical key press does.

This chapter explains remapping at a high level with an emphasis on three ideas:

- **layout layers** (how one keyboard can have multiple logical layouts),
- **macros** (how one press can trigger a sequence),
- and the **portability** of your configuration (how to keep your setup consistent across computers and across builds).

---

## 25.5.1 Where remapping can happen

A key press travels through a stack.

At the bottom is the physical hardware.

At the top is the application you are using.

Remapping can occur at multiple layers of this stack.

### Keyboard firmware remapping

**Firmware** is the software that runs on a device’s internal controller.

In a keyboard, firmware is the software that:

- scans the keyboard matrix,
- decides which “key code” a press should represent,
- and sends that key event to the host computer.

Popular open-source keyboard firmware projects include:

- **QMK** (often expanded as “Quantum Mechanical Keyboard”), which documents keymaps, layers, and macros as core concepts, [R1][R2][R3]
- and **ZMK**, which documents keymaps and macro behaviors in its own configuration system. [R4][R5]

Firmware remapping is highly portable because the keyboard carries its behavior with it.

If you plug the same keyboard into a different computer, the remapping still applies.

### Operating system remapping

An **operating system** is the foundational software that manages hardware and provides services to applications.

Operating systems often provide remapping.

Examples include:

- Linux keymap configuration using X Keyboard (often shortened to XKB) tooling such as xkbcommon, [R10]
- Windows remapping using tools such as PowerToys Keyboard Manager, [R11]
- and macOS modifier key remapping in system settings. [R12]

Operating system remapping is sometimes easier to apply quickly.

However, it is less portable.

It depends on the specific operating system installation, and it may not follow you to a new machine.

### Application-level remapping

Some applications allow remapping inside the application.

This is convenient for specialized tools.

It is also the least portable layer because it only applies within one application.

---

## 25.5.2 A practical rule: remap as low as you can

If you want a key behavior to work:

- on every host,
- in every application,
- and even in early boot screens,

then firmware remapping is usually the right place to implement it.

QMK’s keymap model is designed for this: the keyboard produces standard key events based on its internal keymap. [R1]

If you only need a small convenience change on one specific laptop, operating system remapping can be sufficient. [R10][R11][R12]

Cyberdeck implication:

- treat firmware remapping as the “portable default,” and treat operating system remapping as a local adaptation.

---

## 25.5.3 Layers: many layouts on one keyboard

A **layer** is an alternate mapping.

When a layer is active, the same physical key can produce a different result.

Layers exist because compact keyboards do not have enough physical keys to expose every function directly.

### Layer precedence and fall-through

Layer systems typically decide output by looking from the highest active layer down to the base layer.

If a key is not defined on the active layer, it can be marked as “transparent,” meaning the keyboard should use the definition from a lower layer.

QMK documents layers and the conceptual behavior of switching and stacking. [R2]

ZMK similarly documents keymaps in terms of layers and behaviors. [R4]

This “fall-through” behavior is not a minor detail.

It is what makes large keymaps maintainable.

Instead of duplicating your entire base layout on every layer, you define only what changes.

### A cyberdeck-friendly layer structure

A common approach is to create a small number of role-based layers.

For example:

- a base layer for letters,
- a symbols layer,
- a navigation layer,
- and a tools layer.

The correct structure depends on your cyberdeck’s use case.

A deck used for command-line work will often prioritize navigation and modifiers.

A deck used for data entry may prioritize numeric and template input.

> **Figure idea:** A “layer stack” diagram with four layers labeled Base, Symbols, Navigation, Tools, and arrows showing which thumb key activates which layer.

---

## 25.5.4 Macros: one press, many events

A **macro** is an action that produces a sequence.

In keyboard remapping, macros are commonly used to:

- emit a repeated phrase,
- send a multi-key shortcut,
- or automate a short, well-defined sequence.

QMK documents macro support as a feature and includes explicit caution that storing sensitive information in firmware is a bad idea. [R3]

ZMK also documents macro behavior as a reusable building block in its key behavior system. [R5]

Cyberdeck implication:

- macros can reduce typing in field use,
- but they can also create safety and security risk if they trigger destructive commands or embed secrets.

> **Figure idea:** A “macro layer” diagram: a keypad-like layer where each key expands into a short, safe command template (for example insert timestamp; insert log header; send benign diagnostic shortcut).

---

## 25.5.5 Portability: making your configuration survive new machines

A remapping configuration is part of your cyberdeck.

If it is not reproducible, it will eventually be lost.

Portability has two dimensions.

First, you want your keyboard behavior to be consistent across computers.

Second, you want your configuration to be recoverable across time.

### Portability across computers: speak standard HID

Most modern keyboards communicate with hosts using the **Human Interface Device (HID)** class.

HID defines how a keyboard describes itself and how it reports key events.

The USB-IF HID class definition and HID usage tables are the formal references for this interface. [R7][R8]

If your keyboard firmware produces standard HID usages, host operating systems can typically use generic keyboard drivers.

Microsoft’s documentation for keyboard and mouse HID client drivers provides an example of this host-side “generic HID keyboard” view. [R9]

Cyberdeck implication:

- you generally want your remapping to be implemented as standard HID key outputs,
- because that is what keeps the keyboard compatible across machines.

### Portability across time: version your keymap

The simplest portability tool is version control.

**Version control** is a method for tracking changes to files over time.

The most common system is Git.

QMK explicitly documents an “external userspace” workflow intended to keep your custom keymap in its own directory or repository, rather than embedding it as a one-off modification. [R13]

There is also a community repository template for organizing QMK userspace layouts and sharing them. [R14]

A widely used community example of a portable, opinionated layout that targets multiple firmware ecosystems is the Miryoku layout repository. [R6]

Cyberdeck implication:

- treat your keymap as a project asset;
- keep it in a repository;
- and make it easy to reuse on the next keyboard you build.

---

## 25.5.6 When operating system remapping is appropriate

Operating system remapping is appropriate when:

- you cannot change the keyboard firmware,
- you only need a small change,
- or you are testing an idea before committing it to firmware.

Examples of operating system remapping mechanisms include:

- xkbcommon-based configuration on Linux, [R10]
- Windows-level remapping tools, [R11]
- and built-in modifier remapping on macOS. [R12]

The main cyberdeck risk is divergence.

If your deck sometimes boots into different operating systems, or if you use multiple computers, per-operating-system remaps can cause you to develop inconsistent habits.

This is one reason firmware remapping is often preferred for a cyberdeck that is meant to be a single, self-contained instrument.

---

## 25.5.7 A builder checklist

Before you “finalize” a remapping configuration:

1) Decide your portability goal.

Is the keyboard intended to work the same way on any host, or only on one machine?

2) Design a layer structure.

If the keyboard is compact, define layers early. [R2][R4]

3) Add macros cautiously.

Prefer safe, reversible macros.

Avoid storing secrets in firmware. [R3]

4) Validate host compatibility.

Confirm that the keyboard behaves as a standard HID keyboard on your target hosts. [R7][R8][R9]

5) Version and back up.

Use an external userspace or a dedicated repository, and keep your keymap under version control. [R13][R14][R6]

---

## 25.5.8 Practical takeaway

Firmware remapping is the portability lever.

Layers let a small keyboard behave like a complete keyboard. [R2][R4]

Macros reduce typing, but they must be treated as a safety and security surface. [R3][R5]

The most reliable cyberdeck practice is:

- implement your core layout in firmware,
- emit standard HID key events for compatibility, [R7][R8]
- and keep your keymap portable by versioning it as a reusable configuration artifact. [R13][R14][R6]

---

## Sources

- [R1] QMK docs: Keymap overview — https://docs.qmk.fm/keymap
- [R2] QMK docs: Layers — https://docs.qmk.fm/feature_layers
- [R3] QMK docs: Macros (includes security cautions) — https://docs.qmk.fm/feature_macros
- [R4] ZMK docs: Keymaps — https://zmk.dev/docs/keymaps
- [R5] ZMK docs: Macro behaviors — https://zmk.dev/docs/keymaps/behaviors/macros
- [R6] Miryoku (community portable keymap) — https://github.com/manna-harbour/miryoku
- [R7] USB-IF: Device Class Definition for HID 1.11 (PDF) — https://www.usb.org/sites/default/files/documents/hid1_11.pdf
- [R8] USB-IF: HID Usage Tables 1.5 (PDF) — https://www.usb.org/sites/default/files/hut1_5.pdf
- [R9] Microsoft: Keyboard and mouse HID client drivers — https://learn.microsoft.com/windows-hardware/drivers/hid/keyboard-and-mouse-hid-client-drivers
- [R10] xkbcommon: user configuration (Linux keymap configuration) — https://xkbcommon.org/doc/current/user-configuration.html
- [R11] Microsoft: PowerToys Keyboard Manager (Windows remapping) — https://learn.microsoft.com/windows/powertoys/keyboard-manager
- [R12] Apple: change modifier key behavior (macOS remapping) — https://support.apple.com/guide/mac-help/change-the-behavior-of-modifier-keys-mchl9756d5c1/mac
- [R13] QMK docs: external userspace (portable config workflow) — https://docs.qmk.fm/newbs_external_userspace
- [R14] `qmk_userspace` community repository template — https://github.com/qmk/qmk_userspace
