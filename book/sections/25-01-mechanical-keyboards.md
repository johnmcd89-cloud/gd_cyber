# 25.1 Mechanical keyboards

A cyberdeck is a computer you can carry.

If you can carry it, you can type on it.

So the keyboard is not a peripheral in the usual sense.

It is part of the instrument.

A **mechanical keyboard** is a keyboard where each key uses a discrete mechanical switch mechanism.

This differs from membrane keyboards (which typically use a flexible rubber dome and a printed contact sheet).

Mechanical keyboards are popular in cyberdeck builds for practical reasons:

- they can be repaired and modified,
- they can be configured at the firmware level,
- and they can be mechanically robust when properly mounted.

This chapter explains the component choices (sizes, switches, keycaps) and the design decisions that matter specifically for cyberdecks: mounting and transport robustness.

---

## 25.1.1 A mechanical keyboard anatomy (what parts exist)

A mechanical keyboard has at least five key subsystems.

1) **Switches**

A **switch** is the mechanism under each key that turns a press into an electrical event.

Switch vendors publish specifications such as operating force, travel distance, and expected lifetime.

For example, Cherry publishes specifications for its MX2A line that include operating force and travel parameters. [K1]

2) **Keycaps**

A **keycap** is the plastic top you touch.

Keycaps are not purely aesthetic.

They determine:

- the key’s shape,
- the legend (printing),
- and whether a key is easy to identify by feel.

3) **Stabilizers**

A **stabilizer** is a mechanism used for larger keys (for example, spacebar and enter).

It prevents wobble and uneven pressing.

Keyboard University’s stabilizer overview illustrates why stabilizers are necessary and why mounting choices affect feel and stability. [K8]

4) **Plate and case (structure)**

The plate and case determine rigidity, resonance, and shock behavior.

In cyberdecks, this is also the difference between:

- “works on a desk,”
- and “survives a backpack.”

5) **Printed circuit board and controller (electronics)**

A **printed circuit board (PCB)** is the board that carries electrical traces.

A **controller** is a small computer that scans the keyboard and reports key presses to the main system.

Most mechanical keyboards are built around a scanning structure called a **matrix**.

---

## 25.1.2 Size and layout: the portability lever

For cyberdecks, keyboard size is often the first constraint.

It determines:

- enclosure footprint,
- wrist position,
- and what keys you can access without layers.

Keyboard University provides a clear taxonomy of common keyboard sizes and how they relate to missing or retained key clusters. [K5]

### Common sizes in cyberdeck contexts

- **60%** keyboards remove the function row, navigation cluster, and numpad.

- **65%** keyboards typically restore dedicated arrow keys.

- **75%** keyboards often restore a function row in a compact arrangement.

- **Tenkeyless (TKL)** keyboards remove the numpad but keep a full function row and navigation cluster.

Each step upward usually improves usability for “computer-like” work (coding, terminal usage), but increases bulk.

### Layout standards and predictability

ISO/IEC 9995 is a standards family describing keyboard layout principles (for example, placement of sections and key identification).

You do not need to read the standard to build a cyberdeck.

But the existence of this framework is useful because it explains why:

- layout differences are systematic,
- and why “the same key” can move between layouts.

The ISO page for ISO/IEC 9995-1 provides an entry point for this standards framing. [K4]

> **Figure idea:** A set of silhouettes: 60%, 65%, 75%, and TKL; label which clusters are missing and which are kept.

---

## 25.1.3 Switches: what you can choose, and what you must verify

### Switch specs you should care about

Mechanical switch selection is often discussed as personal preference.

That is true.

But cyberdecks also need engineering sanity.

Switches are specified with measurable parameters:

- **operating force** (how hard you must press),
- **pre-travel** (how far before actuation),
- **total travel** (full distance),
- and **life-cycle rating** (expected number of actuations).

Vendor product pages often list these values.

For example, Cherry MX2A Black includes a specific operating force and travel specification. [K1]

### Full-height versus low-profile

Full-height switches (often called MX-style) dominate the mechanical keyboard ecosystem.

Low-profile switches reduce height and sometimes reduce travel.

This is attractive in cyberdecks where:

- lid clearance is limited,
- and overall thickness matters.

Kailh’s Choc V2 specifications illustrate that low-profile switches are a distinct mechanical family with different geometry and behavior. [K2]

Practical implication:

- low-profile can make a deck physically slimmer,
- but it changes keycap compatibility and feel.

### Compatibility checks that prevent expensive mistakes

Mechanical keyboard parts are a compatibility web.

Before buying, you should verify at least:

- whether the switch is **plate-mounted** or **PCB-mounted**,
- whether it is a **3-pin** or **5-pin** form factor,
- whether the keyboard supports the switch family,
- and whether replacement parts exist.

Switch vendor pages often imply compatibility expectations.

For example, the existence of clear product families and related mechanical dimensions is part of why ecosystem compatibility matters. [K1][K3]

---

## 25.1.4 Keycaps: usability is shape, not only printing

Keycaps affect:

- comfort,
- accuracy,
- and fatigue.

### Units: what “u” means

Keycap widths are often described in **units**, written as **u**.

A 1u key is a standard alphanumeric key width.

A 2u key is twice that width.

Larger keys typically require stabilizers.

Keyboard University’s keycap overview explains keycap sizing concepts and why nonstandard layouts can make keycap coverage difficult. [K7]

### Profiles and rows

Keycaps are often sculpted:

- different rows have different shapes.

If you mix profiles or rows incorrectly, the keyboard becomes uncomfortable.

This is an important cyberdeck point because builders often try to reuse keycaps.

A reusable keyboard is a better cyberdeck keyboard.

But “reusable” is constrained by profile and row compatibility. [K7]

### Legends and field use

Legends are not only decoration.

In field-oriented decks, consider:

- high-contrast legends,
- durable printing methods,
- and tactile key identification.

A deck that is used in poor lighting benefits from keycaps that can be identified by feel.

---

## 25.1.5 Stabilizers: the robustness detail most beginners ignore

Large keys (for example, spacebar) wobble if not stabilized.

A wobbly key is not only annoying.

In a portable device, wobble can also increase mechanical wear.

Keyboard University describes stabilizer types and why mounting choices matter. [K8]

Cyberdeck implication:

- if you want transport robustness, treat stabilizer mounting as a design choice, not as an afterthought.

---

## 25.1.6 Mounting styles and transport robustness

Cyberdeck keyboards are exposed to:

- vibration,
- shock loads,
- twisting,
- and repeated packing and unpacking.

So keyboard mounting is a structural engineering problem.

Keyboard University’s mounting styles guide summarizes common case architectures (tray mount, top mount, sandwich mount, gasket mount, and integrated designs) and how structure affects feel and rigidity. [K9]

A cyberdeck-relevant interpretation is:

- **rigid** designs often survive transport better,
- **flexy** designs often feel pleasant on a desk,
- and you must decide which is more important for your deck.

### Enclosure integration strategies

A practical cyberdeck mounting strategy often includes:

- fastening the keyboard plate or case to the enclosure at multiple points,
- supporting the PCB to prevent flexing,
- and ensuring no single screw or standoff carries all load.

> **Figure idea:** A “load path” diagram: key press force → plate → mounting points → enclosure; show why PCB-only mounting concentrates stress.

---

## 25.1.7 Firmware and programmability: why mechanical keyboards adapt well

Cyberdecks often have nonstandard layouts.

That makes firmware important.

### Matrix scanning

Most keyboards use a row/column matrix.

The controller scans the matrix and infers which switch is pressed.

QMK (often expanded as “Quantum Mechanical Keyboard” firmware) documents how a keyboard matrix works and why scanning is central. [K10]

### Remapping and configuration

Remapping is often critical when you choose a smaller layout.

If you remove keys, you need layers.

VIA provides a configuration workflow for layouts and remapping for compatible keyboards, which is relevant because it can make the deck more adaptable without reflashing firmware for every change. [K11]

### Wireless and low-power keyboards

Some cyberdecks want wireless keyboards.

ZMK is a keyboard firmware ecosystem that emphasizes modern configuration patterns and is commonly associated with low-power and wireless designs.

ZMK’s keymap documentation provides an entry point into its layered and declarative configuration model. [K12]

---

## 25.1.8 Confirming compatibility early (a keyboard bring-up checklist)

Before designing your enclosure around a keyboard, verify:

1) **Physical fit**
   - overall footprint,
   - thickness (including keycap profile),
   - and cable exit direction.

2) **Switch family and mounting**
   - switch type (full-height vs low-profile), [K2]
   - and mounting method. [K9]

3) **Keycap coverage**
   - does your layout have nonstandard keys that require unusual keycaps? [K7]

4) **Stabilizers**
   - do large keys have stable mounting? [K8]

5) **Firmware and remapping**
   - do you have a plan for layers and remaps? [K10][K11]

6) **Transport robustness**
   - is the keyboard protected from side loads?
   - are connectors strain-relieved?

---

## 25.1.9 Community patterns: what cyberdeck builders actually do

Two practical observations show up repeatedly in cyberdeck builds.

First, builders frequently create custom compact key matrices to fit tight enclosures.

Second, builders who care about field use often choose rugged enclosures and sealed ports.

Hackaday.io project logs show examples of cyberdeck keyboard integration, including custom approaches and rugged build choices. [K13][K14]

These sources are valuable not because they define a single best keyboard.

They are valuable because they show the constraints that matter when a keyboard becomes part of a portable system.

---

## 25.1.10 Practical takeaway

Mechanical keyboards are attractive in cyberdecks because they are modular, configurable, and repairable.

But the best cyberdeck keyboard choice is not “the nicest sounding switch.”

It is the keyboard that matches your deck’s constraints:

- layout size for portability, [K5]
- switch geometry for enclosure thickness, [K2]
- keycap and stabilizer choices for durability, [K7][K8]
- and mounting architecture for shock and transport loads. [K9]

If you treat the keyboard as a structural subsystem, your deck becomes more reliable.

If you treat it as a decorative part, it becomes a failure surface.

---

## Sources

- [K1] Cherry: MX2A Black switch specifications — https://www.cherry.de/en-us/product/mx2a-black
- [K2] Kailh: Choc V2 low-profile switch specifications — https://www.kailhswitch.com/mechanical-keyboard-switches/low-profile-key-switches/choc-v2-low-profile-silent-tactile-switch.html
- [K3] Gateron: switch product specification example — https://www.gateron.com/products/gateron-ks-3x47-milky-switch
- [K4] ISO: ISO/IEC 9995-1 (keyboard layout framework entry point) — https://www.iso.org/standard/88610.html
- [K5] Keyboard University: keyboard sizes and layouts — https://www.keyboard.university/100-courses/keyboard-sizes-layouts-gdeby
- [K6] QMK documentation: split keyboards — https://docs.qmk.fm/features/split_keyboard
- [K7] Keyboard University: keycaps (sizing, profiles, compatibility) — https://www.keyboard.university/100-courses/keycaps-101-ydy8j
- [K8] Keyboard University: stabilizers — https://www.keyboard.university/100-courses/stabilizers-lcjf2
- [K9] Keyboard University: mounting styles — https://www.keyboard.university/200-courses/keyboard-mounting-styles-4lpp7
- [K10] QMK documentation: how a matrix works — https://docs.qmk.fm/how_a_matrix_works
- [K11] VIA documentation: layouts and remapping concepts — https://caniusevia.com/docs/layouts/
- [K12] ZMK documentation: keymaps — https://zmk.dev/docs/keymaps
- [K13] Hackaday.io: cyberdeck keyboard project log — https://hackaday.io/project/196795-cyberdeck-keyboard
- [K14] Hackaday.io: cyberdeck build (enclosure/robustness patterns) — https://hackaday.io/project/183892-cyberdeck1
