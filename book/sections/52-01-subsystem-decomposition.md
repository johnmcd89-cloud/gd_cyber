# 52.1 Subsystem decomposition

Cyberdecks feel like small computers.

In practice, they behave like small systems.

They combine power conversion, computing, displays, input devices, radios, storage, and mechanical packaging.

If you treat the build as “one big thing,” you will debug one big thing.

That is slow.

**Subsystem decomposition** is the process of dividing a system into smaller parts with explicit interfaces.

Each part can be built, tested, and replaced independently.

This section explains how to decompose a cyberdeck into sensible subsystems and, critically, how to define the interfaces between compute, display, power, and radios so the build remains testable and serviceable.

---

## 52.1.1 Definitions (subsystems and interfaces)

A **subsystem** is a group of components that performs a coherent function.

Examples include a power subsystem, a display subsystem, or a radio subsystem.

An **interface** is the boundary where two subsystems meet.

An interface has at least three aspects:

1) **Mechanical**: the connector type, mounting, and cable routing.
2) **Electrical**: voltages, currents, grounding, shielding, and pin assignments.
3) **Protocol and behavior**: the data format, timing, and control expectations.

Systems engineering guidance emphasizes managing interfaces explicitly because interfaces are common sources of integration failure.

NASA’s Systems Engineering Handbook treats interface management as a first-class activity, not a documentation afterthought. [S1] [S2]

A common artifact is an **interface control document** (ICD), which is simply a structured description of an interface so both sides can be built consistently.

NASA even publishes templates and reference material for ICDs, reflecting how central this idea is to integration discipline. [S3]

Cyberdeck implication:

A pinout table plus a connector choice plus a current budget is already the start of an ICD.

---

## 52.1.2 Why decomposition matters (debuggability and serviceability)

Portable builds fail in portable ways.

Cables flex.

Connectors loosen.

Power rails sag under load.

If the system is not decomposed, a single symptom (for example, a display flicker) may have many possible causes.

Decomposition reduces ambiguity.

If the display subsystem has a defined power interface and a defined data interface, you can isolate whether the failure is power, data integrity, or the panel itself.

Decomposition also supports iteration.

You can redesign a subsystem (for example, a power distribution board) without rewriting the entire harness.

This is consistent with broader systems engineering practice: defining boundaries and interfaces makes change management possible without chaos. [S4]

---

## 52.1.3 Common cyberdeck subsystems (a practical baseline)

Subsystems should reflect real differences in function and constraints.

A useful baseline decomposition for many cyberdecks is:

### Compute subsystem

This includes the single-board computer or module, its storage, and any boards that exist primarily to support the processor.

The compute subsystem is usually the source of most high-speed interfaces.

### Power subsystem

This includes batteries, chargers, regulators, power switches, fuses, and power distribution wiring or boards.

The power subsystem defines the “electrical environment” that every other subsystem lives in.

### Display subsystem

This includes the display panel, its backlight power, any display driver board, and the mechanical mounting.

The display subsystem often has strict cable routing constraints.

### Input subsystem

This includes keyboard, pointing device, buttons, and any microcontroller or interface board used to translate input devices.

### Radio subsystem

This includes radios (software-defined radio front ends, Wi‑Fi, Bluetooth, Zigbee, sub‑gigahertz modules), antennas, and the cabling between them.

Radio subsystems are sensitive to noise and mechanical placement.

### Storage subsystem

This includes removable storage, solid-state drives, and any adapters.

### Sensor and expansion subsystem

This includes any modular peripherals: sensors, external ports, and expansion headers.

### Enclosure and I/O panel subsystem

This includes the mechanical structure, panel cutouts, and connector mounting.

It is a subsystem because it imposes constraints on all the others.

Community build guides implicitly reinforce this modular framing.

TechNIK’s cyberdeck instructions, for example, explicitly split wiring into multiple harnesses and treat harness completion as a prerequisite for assembly, which is a practical form of subsystem decomposition. [S5]

The Cyberdeck Cafe’s build guide similarly encourages builders to think in modular blocks and repeatable subassemblies, which naturally leads to defining interfaces instead of improvising wiring each time. [S6]

---

## 52.1.4 A method for decomposing your own deck

Decomposition is not a fixed list.

It is a method.

A practical method is:

### Step 1: Start from requirements

Write down what the deck must do.

For example:

- “two hours of battery life,”
- “external display output,”
- “software-defined radio receive capability,”
- “field-serviceable storage.”

Requirements drive which subsystems exist and what interfaces must be serviceable.

### Step 2: Draw a block diagram

A block diagram names subsystems and shows connections between them.

At this stage, do not draw pin numbers.

Draw arrows labelled “power” and “data.”

### Step 3: Define each interface explicitly

For each connection between subsystems, define:

- connector type and keying,
- pinout (signal names),
- voltage levels,
- maximum current,
- cable length and routing constraints,
- and any shielding or grounding expectations.

This is the moment where you decide whether an interface is a harness connector, a board-to-board connector, or an internal solder joint.

### Step 4: Add test points and test procedures

A subsystem boundary should be testable.

Add at least one test point for each power rail.

Add a debug interface for any microcontroller.

Add a simple continuity and short test for each harness.

(These ideas connect directly to harness planning and harness testing in Chapter 50.)

### Step 5: Decide what is “replaceable in the field”

If you want field serviceability, you need connectors.

If you want minimum size, you may reduce connectors.

This is a conscious trade.

---

## 52.1.5 Defining key interfaces: compute, display, power, radios

The required interfaces in a cyberdeck are often dominated by four subsystems.

### Compute ↔ Power interface

This interface defines whether the deck is stable.

Define:

- input voltage range,
- peak current (not just average),
- connector polarity protection,
- grounding strategy.

A useful artifact is a power budget table: each load, its voltage, and its peak current.

### Compute ↔ Display interface

Define:

- the display data interface type (for example, HDMI or a panel interface routed through a driver board),
- the display power rails (backlight power is often separate),
- cable constraints (bend radius and hinge routing).

If the display interface is high-speed, treat it as a special case.

As discussed in Chapter 51.5, it is often safer to use a proven driver board and a known-good cable rather than attempting custom routing.

### Compute ↔ Radio interface

Radio interfaces are not only data.

They include antennas.

Define:

- data interface (for example, USB or serial),
- power rail and noise sensitivity,
- antenna connector type and cable length,
- physical placement constraints.

Radio performance depends strongly on placement, shielding, and separation from noisy power converters.

### Display ↔ Power interface

Displays can be deceptively power-hungry.

Backlights often dominate.

Define:

- backlight voltage and current,
- whether dimming is controlled by a logic signal,
- and whether the backlight ground return is shared with sensitive electronics.

Cyberdeck implication:

Many “display issues” are actually power distribution issues.

A clear interface definition makes that visible.

---

## 52.1.6 Tradeoffs (modularity is not free)

Subsystem decomposition is not pure upside.

It introduces costs.

### Connector proliferation

Every modular boundary usually adds a connector.

Connectors cost money, take space, and can fail.

But they also make testing and replacement possible.

### Size and weight

A fully modular deck can become bulkier.

If your enclosure is extremely constrained, you may choose fewer connectors and more permanent joints.

### Failure isolation versus integration simplicity

More boundaries can make integration more complex.

But fewer boundaries can make failures harder to isolate.

A good decomposition is one where the boundaries align with real differences in function and risk.

### Revision management

If you revise one subsystem, you must keep the interface stable or update the interface documentation.

This is why revision identifiers and interface tables matter.

---

## 52.1.7 Suggested figures

1) **Baseline cyberdeck block diagram**: compute, power, display, radios, input, storage, enclosure.
2) **Interface table example**: one row per connector, with pinout, voltage, max current, cable length.
3) **Boundary test plan**: a diagram showing test points at subsystem boundaries.
4) **Tradeoff chart**: modularity versus size/weight/cost versus serviceability.

---

## 52.1.8 Practical takeaway

Subsystem decomposition is how you make a cyberdeck buildable and debuggable.

Choose subsystems that match real functions.

Define interfaces explicitly.

Add test points at boundaries.

If you can unplug the radio, test the power subsystem independently, and swap the display driver without rewiring the deck, you decomposed the system well.

---

## Sources

- [S1] NASA: *NASA Systems Engineering Handbook (NASA/SP‑2016‑6105 Rev 2)* (systems engineering guidance including interface management) — https://soma.larc.nasa.gov/lws/vfmo/pdf_files/%5bNASA-SP-2016-6105_Rev2_%5dnasa_systems_engineering_handbook_0.pdf
- [S2] NASA: *6.3 Interface Management* (NASA reference page describing interface management as a systems engineering activity) — https://www.nasa.gov/reference/6-3-interface-management/
- [S3] NASA Science: *Interface Control Document (ICD) Template* (example of formal interface documentation practice) — https://science.nasa.gov/wp-content/uploads/2024/05/hpd-icd-template-20240322.docx
- [S4] Defense Acquisition University (DAU): *Systems Engineering Fundamentals* (public systems engineering primer; discusses interface considerations as part of system integration) — https://acqnotes.com/wp-content/uploads/2017/07/DAU-Systems-Engineering-Fundamentals.pdf
- [S5] Hackaday.io: *TechNIK’s Cyberdeck — Instructions* (community build instructions that explicitly separate harness work into subassemblies) — https://hackaday.io/project/192073/instructions
- [S6] The Cyberdeck Cafe: *Cyberdeck Build Guide* (community guidance emphasizing modular build patterns) — https://cyberdeck.cafe/build
- [S7] INCOSE (presentation): Davies, *Interface Management* (practitioner-oriented view of interface management as a discipline) — https://www.incose.org/wp-content/uploads/legacy/enchantment/210609-davies-interface-management.pdf
- [S8] How-To Geek: *Building Cyberdecks Is the Geek Hobby You Need to Check Out* (secondary overview of cyberdeck motivations and build patterns) — https://www.howtogeek.com/building-cyberdecks-is-the-geek-hobby-you-need-to-check-out/
