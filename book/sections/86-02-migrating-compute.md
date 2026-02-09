# 86.2 Migrating compute

A cyberdeck is often built around a compute core that is available “right now.”

In practice, compute cores change.

They change because parts become unavailable, because power and thermal constraints shift, and because new requirements emerge. The mistake is to treat the compute board as the “shape” of the deck. If the enclosure is built around one specific board, then a compute swap forces a mechanical rebuild.

This chapter explains how to design a cyberdeck so that swapping the compute core—especially a **single-board computer (SBC)**—does not require reprinting everything.

---

## 86.2.1 Vocabulary: compute core, single-board computer, and interface boundary

A **single-board computer (SBC)** is a complete computer built on a single circuit board, typically including a processor, memory, and input/output (I/O) interfaces. [S1]

A **compute core** (in this chapter) is the component that runs the operating system and applications. It may be an SBC, a small personal-computer mainboard, or a compute module on a carrier board.

An **interface boundary** is the line where you choose to keep the deck stable. Upstream of the boundary, you allow change. Downstream of the boundary, you expect stability.

In compute migration, the enclosure should be downstream of the boundary.

---

## 86.2.2 The design goal: freeze the enclosure, not the board

A durable cyberdeck treats the enclosure as a stable platform.

The enclosure should define:

- where the display sits,
- where the keyboard sits,
- where external ports appear,
- and where the battery and power components sit.

The compute core is then treated as a replaceable subassembly.

This is the same logic used by repairable products: disassembly should be practical, parts should be replaceable, and documentation should exist to support maintenance. [S4]

---

## 86.2.3 Mechanical strategy: adapter plates, datum surfaces, and compute trays

Compute migration succeeds when mechanical mounting is abstracted.

### Adapter plates as a mechanical abstraction

Different SBC families have different mounting hole patterns and connector locations.

If you mount an SBC directly to the case, those differences become case changes.

Instead, mount the case to a **stable mounting pattern**, then mount the compute board to an **adapter plate** that matches the stable pattern.

An adapter plate is a replaceable, flat interface piece (for example, a printed part, a laser-cut plate, or a small sheet-metal bracket) that translates from “case mounting points” to “board mounting points.”

When the compute board changes, you redesign only the plate.

### Datum surfaces

A **datum surface** is a reference surface you use for alignment.

In enclosure work, you should choose a small number of datum surfaces (for example, one flat plane and one edge) and align compute subassemblies to those references.

This reduces the risk that a small dimensional change forces a full redesign.

### Suggested figure

**Figure 86.2-A: Adapter plate concept.**

A top-down diagram showing four stable case standoffs and a removable plate that carries the compute board’s specific hole pattern.

---

## 86.2.4 Port strategy: replaceable I/O panels instead of board-shaped holes

The fastest way to make compute migration impossible is to cut the enclosure around a specific port layout.

Instead, treat external ports as a panel system.

A panel system has:

- a fixed aperture in the enclosure,
- a replaceable panel insert,
- and internal short extensions (cables or adapters) that connect the compute core to the panel.

If the compute board changes, you update the panel insert and internal cabling, not the enclosure.

Community builds frequently converge on this approach. For example, the “Mini Pi5 Kali Cyberdeck” uses panel-mounted extensions for Ethernet and Universal Serial Bus (USB) ports and notes real-world cable compatibility issues. This is an example of why “port exposure” should be treated as a serviceable subsystem rather than as a permanent cutout. [S7]

---

## 86.2.5 Electrical strategy: keep power distribution stable

Compute boards change power requirements.

Therefore, if the compute board is wired directly into every power consumer, compute migration becomes a full rewiring project.

A compute-migration-friendly deck instead uses a stable **power distribution layer**.

That layer typically includes:

- one input power interface (battery pack or external power),
- protection (for example, fuses or current limits),
- regulated rails if needed,
- and a small number of standardized power connectors.

The compute core becomes a consumer of the stable power layer rather than the power layer itself.

This also improves fault isolation: if the new compute board behaves badly, you can disconnect it without disturbing the rest of the deck.

---

## 86.2.6 Wiring strategy: use transition harnesses and stable connectors

Compute migration is not only mechanical. It is interconnect discipline.

A transition harness is a replaceable harness segment that adapts the compute core to the stable deck modules. (See Chapter 86.3.)

A stable-connector strategy means:

- the same connector families appear at the same physical locations across revisions,
- pin labeling is consistent,
- and the compute-specific wiring is localized to a replaceable segment.

Even without adopting a formal aerospace-style process, it is useful to know that workmanship standards exist specifically for crimping and harness assemblies and that they emphasize repeatable, inspectable interconnect quality. [S5]

---

## 86.2.7 Software strategy: separate the system image from the data

If migrating compute requires rebuilding your entire software environment, migration becomes risky and slow.

A practical strategy is to separate:

- the **system image** (operating system and base configuration), and
- the **data and working state** (documents, logs, and project artifacts).

This can be implemented in many ways. The important idea is that the compute core is replaceable and the software environment is reproducible.

The concept of abstraction also exists in software: a hardware abstraction hides device-specific details behind stable interfaces. While hardware abstraction is not a complete solution for hardware differences, it expresses the same goal: reduce the number of places that must change when hardware changes. [S3]

---

## 86.2.8 A migration checklist: plan for rollback

Compute migration should be treated as a controlled change.

A minimal checklist includes:

- define what must stay stable (enclosure, display mount, keyboard mount, port apertures),
- design or update the adapter plate,
- design or update the panel insert,
- update the transition harness,
- run power and thermal acceptance tests,
- and preserve rollback capability until the new compute core is proven.

### Suggested figure

**Figure 86.2-B: Compute migration boundary map.**

A diagram labeling what changes (compute, adapter plate, transition harness) and what should not (enclosure, panel aperture geometry, UI mounting).

---

## 86.2.9 Sources

[S1] Wikipedia, *Single-board computer* (definition of SBC). https://en.wikipedia.org/wiki/Single-board_computer

[S2] Raspberry Pi, *Raspberry Pi 5 Mechanical Drawing (PDF)* (official mechanical reference example for an SBC family). https://datasheets.raspberrypi.com/rpi5/raspberry-pi-5-mechanical-drawing.pdf

[S3] Wikipedia, *Hardware abstraction* (conceptual framing: stable interfaces hiding device-specific details). https://en.wikipedia.org/wiki/Hardware_abstraction

[S4] iFixit, *Repairability* (repair ecosystem framing: disassembly, tools, documentation, and parts availability). https://www.ifixit.com/repairability

[S5] NASA, *NASA-STD-8739.4 (Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring)* (public standard metadata and scope). https://standards.nasa.gov/standard/nasa/nasa-std-87394

[S6] Raspberry Pi, *Add-on Board / HAT Design Guide* (interface discipline example: defined header usage and boot-time probing behavior). https://raw.githubusercontent.com/raspberrypi/hats/master/designguide.md

[S7] Hackaday.io, *Mini Pi5 Kali Cyberdeck* (community build: panel-mounted extensions; real-world cable/port integration issues). https://hackaday.io/project/197232-mini-pi5-kali-cyberdeck

[S8] Hackaday, *Kali Cyberdeck Looks The Business* (secondary coverage summarizing the build’s enclosure and panelization choices). https://hackaday.com/2024/08/07/kali-cyberdeck-looks-the-business/

[S9] Raspberry Pi, *Compute Module 4 Datasheet (PDF)* (example of a compute-module approach where a stable carrier board can reduce enclosure changes). https://datasheets.raspberrypi.com/cm4/cm4-datasheet.pdf
