# 85.2 Replaceable subassemblies

A cyberdeck becomes dramatically more reliable when it stops being “one complicated object” and becomes a set of well-defined modules.

A module is a subassembly that can be removed and replaced without reworking the entire system. This is not only a convenience. It is a maintenance strategy: you trade the difficulty of repair-in-place for the simplicity of swap-and-restore.

This chapter explains how to design replaceable subassemblies for a cyberdeck, explicitly treating the **screen**, **keyboard**, **compute module**, and **battery/power system** as modules.

---

## 85.2.1 Vocabulary: modularity, field-replaceable units, and line-replaceable units

**Modularity** is the degree to which a system’s components can be separated and recombined, often to reduce complexity by hiding details behind interfaces. [S1]

A **field-replaceable unit (FRU)** is a part or assembly designed to be removed and replaced by a user or technician without sending the whole system for repair. The concept is tied to logistics: stocking spares, training, and repair time goals. [S2]

A **line-replaceable unit (LRU)** is a modular component designed to be replaced quickly at an operating location to restore a system to operational condition. LRUs are part of a multi-level maintenance model: swap at the “line,” repair elsewhere. [S3]

Cyberdecks are not aircraft. But the LRU/FRU concept transfers well: you can design your deck so that a failure becomes a quick module swap.

---

## 85.2.2 Why replaceable subassemblies matter for cyberdecks

A maker cyberdeck is often maintained over a long time horizon. Parts change, cables wear, and new requirements appear.

Replaceable subassemblies help in three ways.

First, they reduce downtime. If the compute module fails, you can swap it without rewiring the entire deck.

Second, they reduce risk during maintenance. A maintenance event is often when a deck is most vulnerable to mistakes: pinched cables, mis-seated connectors, and stripped threads.

Third, they support upgrades. A cyberdeck that can accept a new radio module or a new storage module without a full redesign is simply easier to evolve.

Repairability advocates summarize this as an ecosystem property: repairable products are designed with disassembly in mind and require parts, tools, and documentation to be available, without artificial barriers. [S4]

---

## 85.2.3 The core design requirement: interface discipline

Replaceable subassemblies only work when interfaces are disciplined.

A module boundary must specify:

- **mechanical interface** (how it is mounted and aligned),
- **electrical interface** (how power is delivered and protected),
- **data interface** (how signals move),
- **software interface** (what configuration is expected),
- and **operational interface** (how the operator swaps and verifies it).

If any one of these is ambiguous, “modular” becomes “intermittent.”

### Mechanical interface: datum surfaces and alignment

A module should have a consistent physical reference, meaning it seats the same way every time.

Practical mechanisms include:

- fixed standoffs,
- alignment pins,
- rails or slots,
- and panel cutouts that constrain position.

Avoid designs where cables provide alignment. Cables should connect after alignment, not create it.

### Electrical interface: power first, power last

A module swap is an electrical event.

A conservative discipline is:

- power is disconnected before signal connectors are disturbed,
- power is connected after the module is mechanically seated,
- and power rails are protected against shorts and transients.

### Harness interface: keyed, labeled, and strain-relieved

Wiring harnesses are common failure points.

NASA’s workmanship standard for cable and harness interconnects exists because interconnect quality is critical to system reliability in high-consequence systems. [S5] Cyberdecks do not need aerospace workmanship to benefit from the same mindset: harnesses should be mechanically supported and difficult to reconnect incorrectly.

A field-friendly harness interface has:

- keyed connectors (cannot be inserted backwards),
- labels that persist after rework,
- service loops (enough slack to open panels without strain),
- and strain relief so the connector is not loaded by cable forces.

### Suggested figure

**Figure 85.2-A: Module boundary map.**

A diagram of a cyberdeck with highlighted module boundaries: display lid module, keyboard module, compute module, battery/power module, and I/O panel module.

---

## 85.2.4 Designing the major modules

### Screen module

A screen module is usually more than a display panel. It often includes:

- a lid or mounting frame,
- a driver board,
- a cable harness,
- and mechanical protection (bezel, cover lens).

A robust screen module should allow replacement without requiring full disassembly of the compute bay.

Acceptance tests for a swapped screen module should be simple:

- confirm brightness control,
- confirm no flicker under movement,
- confirm that cable routing does not interfere with hinges or fans.

### Keyboard module

Keyboards are high-touch wear items.

A keyboard module should make replacement possible without rewiring the whole deck. This usually means:

- a dedicated connector path,
- strain relief at the keyboard exit,
- and an interface that is tolerant of repeated open/close cycles.

Acceptance tests:

- confirm full key matrix function,
- confirm that the keyboard does not brown out USB during high-load events,
- and confirm physical stability (no flex that stresses connectors).

### Compute module

The compute module (single-board computer, mini PC, or motherboard) is often the most failure-sensitive part.

To make it replaceable:

- decouple it from the I/O panel using a controlled harness,
- ensure cooling interfaces (heat sink, fan duct) can be reassembled consistently,
- and avoid burying the compute module under non-related wiring.

Acceptance tests:

- boot test,
- storage enumeration,
- basic peripheral check,
- and a short thermal check to confirm cooling contact.

### Battery and power module

A battery module is both an energy source and a safety hazard.

A replaceable battery/power module should include:

- a clear isolation point (switch or removable link),
- protective packaging to prevent puncture,
- and connectors chosen for current and retention.

Acceptance tests:

- verify stable supply at idle and peak load,
- verify that plug/unplug events do not reset the deck,
- and verify that the module can be removed without stressing adjacent wiring.

---

## 85.2.5 Spares and swap logistics: treat modules as controlled items

Replaceable subassemblies only help if you can actually replace them.

A practical spares strategy includes:

- a short list of critical spares (the modules that would stop the deck from working),
- packaging that prevents damage (anti-static bags for electronics; padding for displays),
- labels with module version and date,
- and a minimal acceptance test procedure.

After a swap, you should record what changed. This can be a simple log entry: which module serial or version was installed, when, and why.

---

## 85.2.6 Community context: rugged builds tend to become modular

Cyberdeck builds in rugged cases often evolve toward modularity simply because space and access are constrained.

Hackaday’s coverage of rugged “kit in a case” builds highlights panelized layouts and modular front panels, which are natural module boundaries. [S6]

Similarly, community builds such as Pelican-style decks document how builders organize power, screen, and I/O into separable physical regions, even when the design began as a one-off prototype. [S7]

---

## 85.2.7 Safety and handling: electrostatic discharge and batteries

Module swaps expose electronics.

Electrostatic discharge (ESD) is a sudden flow of current between differently charged objects and can damage sensitive components even without a visible spark. [S8]

Field-friendly handling includes:

- avoiding unnecessary contact with exposed connector pins,
- using anti-static bags for spare boards,
- and establishing a “clean surface” habit for maintenance.

Battery modules require additional care: avoid puncture, avoid short circuits, and ensure the module is mechanically constrained so it cannot move and abrade.

---

## 85.2.8 Sources

[S1] Wikipedia, *Modularity* (system decomposition and interface abstraction concept). https://en.wikipedia.org/wiki/Modularity

[S2] Wikipedia, *Field-replaceable unit* (FRU definition; logistics and support implications). https://en.wikipedia.org/wiki/Field-replaceable_unit

[S3] Wikipedia, *Line-replaceable unit* (LRU definition; swap-to-restore maintenance model). https://en.wikipedia.org/wiki/Line-replaceable_unit

[S4] iFixit, *Repairability* (repair ecosystem framing: disassembly, tools, documentation, parts availability). https://www.ifixit.com/repairability

[S5] NASA, *NASA-STD-8739.4 (Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring)* (public standard metadata and scope). https://standards.nasa.gov/standard/nasa/nasa-std-87394

[S6] Hackaday, *Remixed Pi Recovery Kit V2 Offers Another Path* (community rugged build pattern with modular panels/switches). https://hackaday.com/2024/06/12/remixed-pi-recovery-kit-v2-offers-another-path/

[S7] Jake-Simek, *Pelican-Deck* (community cyberdeck build reference; modular physical layout). https://github.com/Jake-Simek/Pelican-Deck

[S8] Wikipedia, *Electrostatic discharge* (definition and risk framing for exposed electronics). https://en.wikipedia.org/wiki/Electrostatic_discharge
