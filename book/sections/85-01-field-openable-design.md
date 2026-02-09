# 85.1 Field-openable design

A cyberdeck is not only a computer. It is also a piece of equipment.

Equipment must survive maintenance.

Many cyberdecks are modified, repaired, inspected, or reconfigured outside of a workshop: in a vehicle, on a table in a shared space, at an event, or during travel. A cyberdeck that cannot be opened safely and reassembled correctly under those conditions is fragile in a way that will eventually matter.

This chapter defines **field-openable design** and explains how to design for it. It focuses on three required topics:

- **Fastener strategy** (how the enclosure is held together, and how it stays held together).
- **Access panels** (how you open the minimum necessary area without dismantling everything).
- **Modular harnesses** (how internal wiring can be disconnected and reconnected without confusion).

---

## 85.1.1 Vocabulary: field-openable, field-serviceable, and maintainability

A design is **field-openable** when it can be opened and closed safely outside a workshop, with minimal tools, without damaging parts, and without requiring specialized fixtures.

A design is **field-serviceable** when it can be repaired or reconfigured in the field with parts and procedures that are realistic for the operator.

Field-openable is therefore a prerequisite for field-serviceable, but not the same thing.

**Maintainability** is the broader property that a system can be inspected, repaired, and returned to service with predictable effort and predictable outcomes.

A simple way to think about this is: field-openable design reduces the chance that maintenance itself becomes a failure event.

---

## 85.1.2 Why field-openable design matters for cyberdecks

Field-openable design is not “nice to have.” It affects reliability and security.

From a reliability perspective, many cyberdeck failures are not component failures. They are assembly failures: a loose connector, a cable that rubs through insulation, a fan path blocked by a shifted wire bundle, or a screw that backs out and becomes debris.

From a security perspective, opening is part of inspection. If you cannot open the deck predictably, you cannot verify that storage modules, radio modules, or wiring harnesses are what you expect them to be.

Repairability advocates summarize the underlying idea in practical terms: a repairable product is designed with disassembly in mind, and it requires parts, tools, and documentation to be available. [S7]

The cyberdeck version of this is: if you cannot disassemble and reassemble your own deck, then you do not fully own its operational lifecycle.

---

## 85.1.3 Access panels: open the minimum necessary area

The core idea of an access panel is **localized access**.

A cyberdeck often has multiple subsystems that may need attention at different frequencies:

- battery and power wiring (occasionally),
- storage (often),
- radio modules (sometimes),
- general wiring inspection (occasionally),
- and cooling path cleaning (occasionally).

If accessing any one subsystem requires dismantling the entire enclosure, you will either avoid maintenance or perform it hastily.

A good access-panel design has three properties.

First, it exposes the subsystem without exposing unrelated subsystems.

Second, it makes reassembly unambiguous.

Third, it preserves environmental boundaries.

### Environmental boundaries and ingress control

An enclosure is not only a shell; it is also a boundary against dust, moisture, and accidental contact.

The Ingress Protection (IP) code, defined under the international standard IEC 60529, is a widely used way to describe how well an enclosure protects against dust and water. [S1]

Most cyberdecks are not formally IP-rated, but the concept is still useful: every time you open a panel, you are temporarily degrading your boundary. A field-openable design should minimize how often boundaries are degraded, and it should make it easy to restore them.

### Suggested figure

**Figure 85.1-A: Access panel map.**

A top-down diagram of a cyberdeck showing separate panels for “battery/power,” “storage,” and “I/O,” with arrows indicating the minimum open area for each maintenance task.

---

## 85.1.4 Fastener strategy: design the “open and close” as a procedure

Fasteners are not incidental.

They determine whether opening is quick or frustrating, and whether closing is reliable.

A **fastener strategy** is the deliberate choice of:

- what fasteners are used,
- how many different fastener types exist,
- where they are placed,
- and how they are secured against vibration and repeated maintenance.

### Reduce fastener variety

Every extra fastener type increases the chance of a field mistake.

A conservative goal is to minimize the number of tool interfaces required for routine opening.

### Prefer captive and guided hardware when possible

A **captive** screw is a screw that stays attached to the panel even when loosened.

Captive hardware reduces the chance of losing parts in grass, in a vehicle, or inside the deck.

### Thread management and vibration

Transport and vibration tend to loosen fasteners over time. One mitigation is a **threadlocking compound** (threadlocker), an adhesive applied to fastener threads to resist loosening and corrosion. [S4]

Henkel’s threadlocking overview describes threadlockers as materials that fill gaps between threads and produce vibration-resistant assemblies, and it highlights that strength and removability must be chosen intentionally. [S5]

In a field-openable design, removability matters. A fastener that can only be removed with heat is rarely field-friendly.

### Torque as a design parameter

If you overtighten, you strip threads and crack plastics. If you undertighten, the panel loosens.

A practical compromise is to document “tighten until seated, then a small additional turn” for non-critical panels, and to use torque specifications only where needed.

### Suggested figure

**Figure 85.1-B: Fastener strategy table.**

A table listing each panel, the fastener type, the tool required, whether threadlocker is used, and the expected open frequency.

---

## 85.1.5 Modular harnesses: make disconnection safe and reassembly correct

A harness is a structured bundle of wires and connectors.

Cyberdecks often begin as ad-hoc wiring and then become more complex. Field-openable design requires crossing a threshold: wiring must become modular enough that you can open the deck without tearing it apart.

NASA’s workmanship standard for interconnecting cables and harness assemblies exists because cable and harness connections are a primary reliability concern in critical systems. [S2] While a cyberdeck is not a spacecraft, the lesson transfers: wiring needs mechanical support, consistent termination, and documentation.

### Principles for modular harness design

A conservative modular harness design uses:

- connectors that are keyed (they cannot be inserted the wrong way),
- service loops (a small length of extra cable that allows opening without strain),
- and labeling that persists after rework.

The goal is not to maximize modularity. The goal is to minimize reassembly ambiguity.

### Document “what goes where” inside the deck

A field-openable deck should include a wiring map.

This can be as simple as a printed diagram inside the access panel, or a QR code linking to a wiring document.

Repairability is not only mechanical. It depends on documentation being available when the deck is open. [S7]

### Suggested figure

**Figure 85.1-C: Harness modularity sketch.**

A diagram showing a main harness that stays in place, with detachable sub-harnesses for display, storage, and radio modules, each with a keyed connector and a labeled service loop.

---

## 85.1.6 Field opening workflow: a checklist mindset

A field-openable design is incomplete without an operator workflow.

A practical workflow has three phases.

### Pre-open

You should establish a safe state.

- Power down, unless live diagnosis is required.
- Isolate batteries where possible.
- Remove external cables that could lever against ports.
- Prepare a clean surface to hold fasteners and panels.

### Open and service

Move slowly and make observations.

If a cable seems tight, do not force the panel. Tightness is a signal that the harness design needs improvement.

### Close and verify

Closing is not “reverse the steps.” It is a verification phase.

A simple verification includes:

- confirm that cables are not pinched,
- confirm that fan paths are not blocked,
- confirm that seals (if present) are seated,
- and confirm that all intended fasteners are installed.

If your deck is used in dusty or wet environments, closing should include restoring ingress boundaries. IP code concepts provide a vocabulary for why this matters, even if you do not claim a formal rating. [S1]

### Suggested figure

**Figure 85.1-D: Open/close flowchart.**

A flowchart: power down → isolate battery → open panel → service action → inspect → close → torque check → functional test.

---

## 85.1.7 Community patterns: rugged cases and service-friendly panels

Community builds in rugged cases frequently demonstrate field-openable instincts: external panels, modular connectors, and switches exposed for quick access.

Hackaday’s coverage of rugged case designs often highlights front-mounted switches and modular panels that can be swapped or simplified across build versions. [S8]

Compact cyberdeck builds similarly emphasize access to connectors and internal layout choices that support rapid modification. [S9]

These are not standards, but they are useful as evidence that field-openable design is a common, practical requirement in the cyberdeck ecosystem.

---

## 85.1.8 Sources

[S1] Wikipedia, *IP code* (ingress protection concept; IEC 60529). https://en.wikipedia.org/wiki/IP_code

[S2] NASA, *NASA-STD-8739.4 (Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring)* (public standard metadata and scope). https://standards.nasa.gov/standard/nasa/nasa-std-87394

[S3] Wikipedia, *Thread-locking compound* (threadlocker purpose and removability concept). https://en.wikipedia.org/wiki/Thread-locking_fluid

[S4] Henkel (LOCTITE), *Threadlocking solutions* (vibration-proofing concept; selection factors). https://next.henkel-adhesives.com/us/en/applications/threadlockers.html

[S5] iFixit, *Repairability* (disassembly-first design; documentation/tools/parts ecosystem). https://www.ifixit.com/repairability

[S6] iFixit, *Repair Manuals for Every Thing* (repair documentation as an operational resource). https://www.ifixit.com/Guide

[S7] Hackaday, *Remixed Pi Recovery Kit V2 Offers Another Path* (rugged case build pattern; modular panels and switches). https://hackaday.com/2024/06/12/remixed-pi-recovery-kit-v2-offers-another-path/

[S8] Hackaday, *Kali Cyberdeck Looks The Business* (compact panelized cyberdeck build). https://hackaday.com/2024/08/07/kali-cyberdeck-looks-the-business/
