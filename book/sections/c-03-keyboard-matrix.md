# C.3 Keyboard matrix

A cyberdeck keyboard is a human–machine interface component: it determines how quickly a user can work, how much space the deck requires, and how much mechanical abuse the build can tolerate.

Keyboard selection is also one of the easiest places to “accidentally design yourself into a corner.” A keyboard that feels comfortable on a desk may be unusable when mounted, a compact keyboard may lack essential keys unless you remap it, and a mechanically fragile connector can turn a portable deck into a maintenance project.

This chapter provides a method for selecting keyboards using a mission-driven **keyboard matrix** (a decision matrix tailored to keyboards). It focuses on four topics that most often drive success or failure:

1) layout,
2) mounting,
3) firmware remapping,
4) and durability.

---

## C.3.1 Definitions (plain language)

A **keyboard layout** is the physical and functional arrangement of keys (physical positions, legends, and what keys mean in software). [S2]

A **human interface device (HID)** is a class of peripheral device intended for human input or output. The term commonly refers to the Universal Serial Bus human interface device specification. [S1]

A **firmware** is software that runs on a device’s microcontroller and implements how the device behaves. In keyboards, firmware typically implements key scanning, debouncing, and the mapping from physical keys to key codes.

**Key travel** is how far a key moves when pressed.

**Actuation force** is the force required to trigger a key press.

**Debouncing** is a technique for handling the fact that mechanical contacts can rapidly make and break contact when pressed, which would otherwise appear as multiple presses.

**Key rollover** is a keyboard’s ability to correctly handle multiple simultaneous key presses; “n-key rollover” means it can detect many keys at once. [S3]

A **matrix** (in the keyboard sense) is a wiring topology in which keys are arranged in rows and columns and detected by scanning; it is common in custom keyboards.

---

## C.3.2 Why keyboard choice is mission-dependent

Keyboard choice is strongly shaped by mission.

- A field cyberdeck prioritizes robustness, waterproofing tolerance, and secure mounting.
- A writerdeck prioritizes low power and comfortable long-duration typing.
- A wearable or head-mounted “cyberdeck” prioritizes minimum volume and reliable operation while moving.

A mission-driven matrix is useful because it forces you to score tradeoffs explicitly. If you cannot explain why a keyboard is “worth” the space it occupies, you will likely regret the choice.

---

## C.3.3 Layout: what you need, and what you can remap

Layout has two dimensions:

1) physical key placement and size,
2) and the logical functions produced by firmware and operating system mapping.

A compact keyboard can be fully functional if remapping is available, but remapping has a learning curve. Conversely, a full-size layout may reduce cognitive load but can dominate enclosure design.

### Suggested figure

**Figure C.3-A: Layout comparison (same-scale outlines).**

A set of same-scale top-view outlines comparing candidate layouts, annotated with keep-out zones for cables, mounting bosses, and nearby controls.

---

## C.3.4 Interfaces: wired, wireless, and why “it should work” usually does

Most cyberdeck keyboards connect over **Universal Serial Bus (USB)** and present as a **USB HID** keyboard. USB Implementers Forum documentation describes the Device Class Definition for HID and frames it as the key reference for building USB-compatible HID devices. [S4] [S5]

A practical implication is that USB HID keyboards tend to be broadly compatible across operating systems.

Wireless keyboards are often Bluetooth-based. For cyberdeck builders, the most important practical differences are:

- power consumption,
- pairing and reconnection reliability,
- and whether the keyboard remains usable in environments where radio links are unreliable.

Firmware ecosystems designed for wireless keyboards explicitly emphasize power behavior. For example, ZMK describes itself as firmware designed for power efficiency and for wired and wireless input devices, including low-power sleep states and battery reporting. [S9]

---

## C.3.5 Mounting: the part that breaks first

Mounting is where keyboard “specifications” become physical reality.

A keyboard mounted inside a cyberdeck enclosure experiences different stresses than a desktop keyboard:

- torsion and bending from carrying,
- connector strain from cable routing,
- and vibration from movement.

For custom keyboard components, you should prefer documentation that explicitly describes the stack-up.

For example, Solder Party’s KeebDeck Keyboard documentation describes a sandwich-style design consisting of a silicone keypad and an adhesive metal dome sheet, mounted via a specific printed circuit board footprint and held by a top case. It includes a stack-up illustration and clear guidance that the keyboard is a component intended for easy integration into custom projects. [S12]

This is an unusually good model for cyberdeck work: it treats mounting as part of the product.

### Suggested figures

- **Figure C.3-B: Mounting architecture (exploded view).** Keycap or keypad → switch or dome sheet → plate → printed circuit board → standoffs → case.
- **Figure C.3-C: Cross-section thickness budget.** A side cutaway showing total thickness and keep-out volumes for connectors and underside components.

---

## C.3.6 Firmware remapping: what you can change, and what you cannot

Remapping is the practice of changing what a key does without changing the physical keyboard.

The ecosystem is broad, but three open-source families are especially relevant.

### QMK

QMK (Quantum Mechanical Keyboard) documentation describes QMK as an open-source community centered around developing input devices and maintaining firmware, tools, and documentation. [S7]

QMK supports a large number of keyboards and enables deep customization (layers, macros, and device features). [S8]

### VIA and Vial

VIA presents itself as a configuration tool that detects compatible keyboards and allows configuration with minimal friction (“plug in your keyboard”). It also notes broad compatibility across many keyboards. [S10]

Vial describes itself as an open-source cross-platform graphical user interface and a QMK fork for configuring a keyboard in real time, and it emphasizes that using it requires Vial-compatible firmware on the keyboard. [S11]

From a cyberdeck perspective, the key point is that “remapping” can mean:

- recompiling firmware (powerful, but slower to iterate),
- or live configuration tools (fast, but only if the device supports the protocol).

### ZMK

ZMK documentation describes ZMK as firmware built on the Zephyr real-time operating system and designed for power efficiency and wireless input devices. [S9]

For cyberdecks that include Bluetooth keyboards, ZMK is particularly relevant because power behavior is part of its stated design goals.

### Suggested figure

**Figure C.3-D: Firmware remap and validation flow.**

A pipeline diagram: physical matrix → firmware scan map → keymap layers → compile/flash or live remap → functional test grid → regression test.

---

## C.3.7 Durability: what fails in portable builds

Durability should be evaluated as a system property.

A keyboard that survives on a desk can fail in a portable deck because:

- connector strain is higher,
- contamination and moisture exposure are more likely,
- and mechanical fasteners loosen over time.

A practical durability checklist includes:

- **Connector strategy:** ensure cable exits have strain relief; avoid sharp bends.
- **Mounting strategy:** prefer designs where mounting points are documented and accessible.
- **Serviceability:** a keyboard that can be removed and replaced without destroying adhesive is easier to maintain.
- **Key rollover needs:** if your mission requires chord-heavy shortcuts or gaming-like simultaneous presses, verify rollover behavior. [S3]

---

## C.3.8 The keyboard matrix method

### Step 1: State the mission

Write down what you actually do.

Examples:

- “Write for two hours on battery power.”
- “Operate while standing outdoors.”
- “Use keyboard shortcuts heavily.”

### Step 2: Apply hard constraints

Hard constraints should gate options early.

Common hard constraints include:

- maximum thickness,
- minimum key count,
- maximum footprint,
- and a required interface (for example, USB HID only).

### Step 3: Choose criteria and weights

A keyboard matrix usually scores:

- layout suitability,
- mounting and serviceability,
- remapping flexibility,
- durability and connector robustness,
- power behavior (especially for wireless),
- availability and total cost.

### Step 4: Score with evidence

Use an anchored 1–5 scale:

- 5 = clearly meets mission needs,
- 3 = acceptable with compromises,
- 1 = unacceptable or requires major redesign.

### Step 5: Sensitivity check

Reweight your top two criteria by ±10 percentage points and see whether the winner changes.

If the winner changes easily, consider designing the enclosure to accept multiple keyboard options.

---

## C.3.9 Example weight sets by mission

### Mission A: Field cyberdeck

- Durability and serviceability: 25
- Mounting integration risk: 20
- Layout suitability: 20
- Remapping flexibility: 15
- Interface compatibility: 10
- Availability and total cost: 10

### Mission B: Writerdeck

- Layout suitability (typing comfort): 30
- Mounting and stability: 20
- Remapping flexibility: 15
- Power behavior: 15
- Durability and serviceability: 10
- Availability and total cost: 10

### Mission C: Wearable or head-mounted

- Footprint and thickness: 30
- Interface reliability: 20
- Power behavior: 20
- Remapping flexibility: 10
- Durability and serviceability: 10
- Availability and total cost: 10

---

## C.3.10 Scoring sheet template

| Criterion | Weight (%) | Option A | Option B | Option C | Notes (evidence) |
|---|---:|---:|---:|---:|---|
| Layout suitability |  |  |  |  |  |
| Mounting integration risk |  |  |  |  |  |
| Remapping flexibility |  |  |  |  |  |
| Durability and serviceability |  |  |  |  |  |
| Interface reliability |  |  |  |  |  |
| Power behavior |  |  |  |  |  |
| Availability and total cost |  |  |  |  |  |

---

## C.3.11 Suggested figures (choose to match your build)

- **Figure C.3-1:** Layout comparison (same-scale overlays + dimension table).
- **Figure C.3-2:** Key spacing and reach heatmap.
- **Figure C.3-3:** Keyboard matrix electrical topology (rows and columns).
- **Figure C.3-4:** Ghosting and diode placement diagram.
- **Figure C.3-5:** Mounting architecture exploded view.
- **Figure C.3-6:** Cross-section stack-up (thickness budget).
- **Figure C.3-7:** Key travel versus total device thickness (scatter plot).
- **Figure C.3-8:** Layout versus mounting compatibility matrix.
- **Figure C.3-9:** Selection decision tree.
- **Figure C.3-10:** Firmware remap and validation flow.

---

## C.3.12 Sources

[S1] Wikipedia, *Human interface device* (definition and context). https://en.wikipedia.org/wiki/Human_interface_device

[S2] Wikipedia, *Keyboard layout* (physical and functional layout concept). https://en.wikipedia.org/wiki/Keyboard_layout

[S3] Wikipedia, *Key rollover (n-key rollover)* (simultaneous keypress handling concept). https://en.wikipedia.org/wiki/N-key_rollover

[S4] USB Implementers Forum, *Human Interface Devices (HID) Specifications and Tools* (HID class overview and links). https://www.usb.org/hid

[S5] USB Implementers Forum, *Device Class Definition for HID 1.11* (HID class definition intent and goals). https://www.usb.org/document-library/device-class-definition-hid-111

[S6] QMK Firmware documentation, *What is QMK Firmware?* (QMK community and tool ecosystem description). https://docs.qmk.fm/

[S7] QMK Firmware, *qmk.fm* (project overview and compatibility framing). https://qmk.fm/

[S8] GitHub, *qmk/qmk_firmware* (firmware source repository; scope and maintenance). https://github.com/qmk/qmk_firmware

[S9] ZMK documentation, *Introduction to ZMK* (power efficiency and wired/wireless scope). https://zmk.dev/docs

[S10] VIA, *VIA* (real-time configuration tool framing and supported keyboards). https://www.caniusevia.com/

[S11] Vial, *Vial* (open-source GUI and QMK fork for real-time configuration). https://get.vial.today/

[S12] Solder Party, *KeebDeck Keyboard* (handheld keypad component; documented stack-up and integration approach). https://www.solder.party/docs/keebdeck/keyboard/

[S13] GitHub, *solderparty/keebdeck_keyboard_hw* (hardware repository including footprint and integration notes). https://github.com/solderparty/keebdeck_keyboard_hw
