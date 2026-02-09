# 86.3 Transition harnesses

Compute modules change faster than enclosures.

A cyberdeck’s case, display, keyboard, and port layout are expensive to rebuild because they are constrained by geometry, tooling, and user expectations. By contrast, the compute module is often selected from an ecosystem where boards are revised frequently and availability changes unpredictably.

A compute swap can be easy or painful depending on whether the deck was designed to **absorb change**. A **transition harness** is the part of the wiring and interconnect that makes change reversible.

This chapter explains what transition harnesses are, why they reduce rework, and how to design them so that compute swaps do not require reprinting or rebuilding the entire deck.

---

## 86.3.1 Vocabulary: harness, adapter board, and revision

A **cable harness** (also called a wire harness) is an assembly of electrical cables or wires that transmit signals or electrical power, typically bundled and secured so that it can be installed, serviced, and protected as a unit. A harness exists largely to control vibration, abrasion, moisture exposure, and installation complexity. [S2]

A **transition harness** is a harness designed to be removed, replaced, or reworked without disturbing the rest of the deck. Its purpose is to adapt one compute module to a fixed enclosure and fixed user-interface modules.

An **adapter board** is a small printed circuit board used to change a connector, pinout, or spacing so that the harness can remain stable even if the compute module changes. In a disciplined design, the adapter board is the “translation layer” between revisions.

A **revision** is a labeled, documented version of a design. In this chapter, a harness revision is a controlled change to wiring that preserves the larger system. This is consistent with the idea of orderly change control as used in configuration management and engineering change processes. [S5]

---

## 86.3.2 Why transition harnesses minimize rework

Rework is not only time. Rework is risk.

Each time you dismantle a deck to rewire it, you create opportunities for:

- reversed connectors,
- damaged conductors,
- weakened crimps,
- cracked solder joints,
- and missing or ineffective strain relief.

A transition harness reduces this risk by **localizing change**. It creates a boundary between the compute module and the rest of the deck, so the board can change while the enclosure and the external I/O (input/output) layout remain intact.

The practical goal is simple: when the compute module changes, you should rework a small, accessible assembly instead of redesigning the deck.

---

## 86.3.3 The key design idea: keep a stable interface boundary

A cyberdeck that migrates compute cleanly has a stable interface boundary.

An **interface boundary** is the line where you choose to “freeze” decisions. Upstream of the boundary, change is allowed. Downstream of the boundary, change is expensive and should be rare.

For many decks, the best boundary is between:

- the compute module (which may change), and
- the deck’s human interface and enclosure (which you want to keep).

A transition harness is the physical embodiment of that boundary.

### Suggested figure

**Figure 86.3-A: Interface boundary diagram.**

A block diagram with three columns.

- Left: “Compute module (Revision A, B, C).”
- Center: “Transition harness + adapter board (replaceable).”
- Right: “Stable deck modules: display, keyboard, storage bay, panel ports, power distribution (frozen).”

---

## 86.3.4 Patterns: adapter boards and harness revisions that survive compute swaps

Transition harness design is mostly about choosing which parts are allowed to change.

### Pattern: pin-mapping adapter boards

A pin-mapping adapter board sits between the compute module and the harness. It adapts a new pinout to the existing harness without changing the downstream wiring.

This is useful when a new board keeps the same functions (for example, the same display interface, the same power input, or the same serial console) but reorganizes pin locations.

The adapter board turns a large rewrite into a small reroute. It also makes rollback possible: if the new compute module behaves badly, you can reinstall the old adapter.

### Pattern: segmented harnesses instead of one long “octopus”

A common anti-pattern is a single harness where every wire runs from the compute module directly to its endpoint. That structure maximizes the blast radius of change.

Prefer **segments** with clear boundaries, such as:

- compute‑to‑adapter,
- adapter‑to‑panel,
- panel‑to‑peripheral.

Segmented harnesses let you replace only the segment that is compute-specific.

### Pattern: replaceable I/O panels and breakouts

When ports are exposed through a fixed panel, terminate the harness at a **replaceable breakout** near the panel.

If the compute module’s port placement changes, you replace the breakout plate or the adapter board, not the entire enclosure. This is the same design instinct that drives serviceable panel-mount connectors: wear and change should happen at a replaceable surface.

### Pattern: treat strain relief as part of the design

A harness must not use a connector as a structural member.

A transition harness should include:

- **strain relief**, meaning mechanical support that prevents the cable from transferring load into the connector,
- and a **service loop**, meaning intentional slack that prevents tension during assembly and maintenance.

Even if you do not adopt formal workmanship standards, it helps to remember that there are published workmanship requirements for cable and harness assemblies in high-reliability contexts (for example, NASA’s workmanship standard for crimping and harnesses). [S1]

---

## 86.3.5 Harness revision discipline: documentation and control

A transition harness works only if you can understand it later.

A harness revision should be treated like a design revision.

That means you produce a small documentation set that travels with the deck.

### What to document

At minimum, record:

- a pinout mapping table (signal name, source pin, destination pin),
- connector orientation notes (what “pin 1” means in the physical world),
- wire color rules (if you use color coding),
- and a revision log describing what changed and why.

The goal is not bureaucracy. The goal is to make the next compute swap predictable.

### Labeling: make it legible under stress

Label both ends of every harness segment.

If two connectors are physically compatible but electrically different, label them as if you are protecting your future self from an avoidable error.

### Suggested figure

**Figure 86.3-B: Harness revision tree.**

A small diagram showing compute revisions on one side, stable deck modules on the other, and the harness/adapter in the middle with labeled harness revisions (H1, H2, H3).

---

## 86.3.6 Validation: continuity, polarity, and staged swap

A transition harness must be tested before it is trusted.

A harness can be “almost correct” and still destroy hardware. Therefore, validation should be conservative.

### Minimal electrical validation

Before connecting a compute module:

- verify **continuity** (every pin connects to the intended destination),
- verify **polarity** (power and ground are not swapped),
- and verify that adjacent pins are not shorted.

Then connect power only and verify that the correct voltage appears at the correct locations.

### Staged swap protocol

Avoid swapping everything at once.

A staged swap isolates failures and preserves evidence.

1) Install the harness and verify power rails.
2) Connect the compute module and verify boot.
3) Enable peripherals one at a time.

This procedure is slow by design. The time you spend staging is time you do not spend debugging compounded failures.

### Suggested figure

**Figure 86.3-C: Pinout mapping table.**

A table with columns for “signal name,” “old compute pin,” “new compute pin,” “harness segment,” and “validation notes.”

---

## 86.3.7 Community pattern: compute swaps drive interconnect modularity

In community cyberdeck builds, compute swaps often trigger interconnect redesign.

For example, Hackaday’s coverage of the “Recovery Kit” designs shows an iteration where the compute board is swapped (a newer board to a prior generation) and the internal power distribution wiring is simplified. That kind of change is a real-world illustration of why modular, replaceable wiring and panels reduce rebuild effort: the deck remains a “kit,” while internal interconnect evolves. [S6]

---

## 86.3.8 Sources

[S1] NASA, *NASA-STD-8739.4 (Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring)* (standard scope and intent). https://standards.nasa.gov/standard/nasa/nasa-std-87394

[S2] Wikipedia, *Cable harness* (definition and why harnesses exist: vibration, abrasion, moisture, installation). https://en.wikipedia.org/wiki/Cable_harness

[S3] Wikipedia, *IPC (electronics)* (IPC as an electronics workmanship standards body; context for cable/harness acceptance standards). https://en.wikipedia.org/wiki/IPC_(electronics)

[S4] Wiring Harness Manufacturer’s Association (WHMA), *WHMA* (industry context for wiring harness manufacturing and training). https://www.whma.org/

[S5] Wikipedia, *Engineering change order* (change-control artifact; categories of design change). https://en.wikipedia.org/wiki/Engineering_change_order

[S6] Hackaday, *Remixed Pi Recovery Kit V2 Offers Another Path* (community build iteration; compute swap and wiring simplification as a revision trigger). https://hackaday.com/2024/06/12/remixed-pi-recovery-kit-v2-offers-another-path/

[S7] Hackaday, *cyberdeck tag archive* (secondary source summarizing the culture of iterative cyberdeck builds). https://hackaday.com/tag/cyberdeck/
