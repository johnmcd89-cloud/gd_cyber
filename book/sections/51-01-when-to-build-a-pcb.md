# 51.1 When to build a printed circuit board

Cyberdeck projects often begin as a set of modules connected by wires.

At some point, many builders ask a natural question: should this be a printed circuit board?

A **printed circuit board** (PCB) is a rigid or flexible sheet of insulating material with patterned copper conductors (called traces) that electrically connect components.

A PCB is not “more advanced wiring.”

It is a different manufacturing approach with different strengths.

This section provides a decision framework for when designing a PCB reduces risk, cost, or complexity—and when it adds unnecessary effort.

---

## 51.1.1 What you use instead of a PCB (and what those options are good for)

Many cyberdecks work well without custom boards.

Before deciding to design a PCB, it helps to understand the common alternatives.

### Point-to-point wiring

**Point-to-point wiring** means you directly connect wires between parts.

It is fast for simple systems.

It becomes difficult to service as wire count grows, because every modification changes routing and strain relief.

### Breadboards

A **breadboard** is a reusable prototyping platform with spring contacts under a grid of holes.

Breadboards are excellent for quick experiments.

They are usually poor choices for a portable cyberdeck because they are sensitive to vibration and do not provide strong strain relief.

### Perforated prototyping boards (perfboard)

A **perforated prototyping board** (often called perfboard) is a rigid board with a grid of holes.

Wires and components can be soldered into the holes.

Perfboard can be robust if the assembly is mechanically supported, but it still requires manual wiring and careful workmanship.

### Breakout and adapter boards

A **breakout board** is a small PCB that adapts a difficult connector or component into a simpler interface.

Breakout boards reduce the need for delicate hand wiring, especially for small-pitch connectors.

They are a common “middle step”: you can get most of the benefits of a PCB (stable footprints and connectors) without committing to a full custom system board.

---

## 51.1.2 The core question: what problem are you trying to solve?

A PCB is justified when it solves a specific problem that wiring cannot solve efficiently.

For cyberdecks, the most common drivers are repeatability, robustness, and wiring complexity reduction.

### Repeatability

A harness built by hand is an artisan object.

That is fine for one build.

If you want to build multiple identical decks (or simply want to re-make a subsystem later), a PCB turns a wiring plan into a reproducible artifact.

The design file becomes part of your documentation.

### Robustness

A well-designed PCB can be mechanically robust because:

- connectors are anchored to a rigid substrate,
- traces do not fatigue like moving wires,
- component placement is fixed.

However, a PCB is only robust if the system design includes strain relief for cables and does not treat the PCB connector as a handle.

### Wiring complexity reduction

Wiring complexity grows faster than most beginners expect.

A design that feels simple at ten wires can become unmanageable at fifty.

A PCB can replace bundles of wires with a small number of connectors and a known pinout.

This simplifies service, reduces accidental swaps, and improves visual inspection.

Cyberdeck implication:

If you keep rewriting the harness, you may be trying to solve a PCB-shaped problem with wire.

---

## 51.1.3 Practical criteria: when a PCB is the right tool

A PCB is usually worth building when several of the following are true.

### You have many repeated connections

Examples include:

- power distribution to multiple loads,
- many identical connectors (for example, several sensors),
- a front-panel input/output cluster.

Repeated patterns are where a PCB shines.

### You need controlled geometry

Some requirements are hard to meet with free-form wiring:

- consistent connector placement for a panel,
- matching a mechanical cutout pattern,
- keeping short, predictable paths for high-speed or sensitive signals.

Standards such as IPC‑2221 (a generic PCB design standard) exist because geometric constraints and spacing rules strongly affect manufacturability and reliability. [S1]

### You want to encapsulate a subsystem

A PCB is a way to define a subsystem boundary.

Instead of “a cluster of wires,” you can have “a power board” or “an interface board.”

Subsystem boards are especially helpful in cyberdecks because they turn a hand-built enclosure into a modular system.

### You can tolerate an iteration loop

A PCB design is rarely correct on the first revision.

Even a simple board often needs at least one iteration.

If your timeline cannot tolerate waiting for fabrication and assembly, it may be better to delay the PCB decision until the design stabilizes.

Fabrication vendors publish design rule documentation that shows the constraints you must design to: minimum trace width, minimum spacing, hole sizes, and similar manufacturability limits. [S5] [S6]

---

## 51.1.4 When you should *not* build a PCB yet

A PCB can be an expensive way to freeze a design that is still changing.

Delay a custom PCB when:

- the system architecture is uncertain,
- the connector set is still in flux,
- you expect frequent changes during early experimentation,
- you do not yet understand the mechanical layout.

In early stages, breakout boards and perfboard often give you 80 percent of the value with 20 percent of the commitment.

---

## 51.1.5 Cost and lead time (why “cheap PCBs” can still be costly)

Modern PCB fabrication can be inexpensive per board.

The hidden cost is the iteration loop.

A board revision costs:

- design time,
- waiting time,
- rework time,
- and often a new set of parts.

Design for manufacturability (DFM) is the practice of designing in ways that reduce fabrication and assembly surprises.

Vendors often publish DFM-oriented guidance and explicit rule sets so you can align your design with what they can reliably build. [S6] [S7]

Cyberdeck implication:

A PCB is rarely the fastest path to “first power-on,” but it is often the fastest path to a stable, repeatable system.

---

## 51.1.6 Cyberdeck-specific PCB patterns (what builders actually make)

The most common cyberdeck custom PCBs are not full computers.

They are helper boards that remove wiring pain.

### Power distribution boards

A power distribution board can:

- split a battery or regulator output into multiple fused outputs,
- provide test points,
- enforce polarity,
- and reduce the number of wire splices.

### Panel input/output boards

Front and side panels often include:

- USB ports,
- audio jacks,
- display connectors,
- switches and indicators.

A PCB can align these connectors precisely with the panel, turning a fragile collection of panel-mount pigtails into a single serviceable assembly.

### Adapter and interposer boards

An adapter board sits between modules.

For example, it might convert a small connector pitch into a larger one, or rearrange pinouts to reduce cable crossings.

DigiKey’s cyberdeck “expansion plate” project is a clear example of this pattern: a purpose-built board organizes connectors and expansion features to simplify a cyberdeck build. [S9]

### When this becomes “culture,” not just engineering

Cyberdecks are a maker subculture as well as a technical object.

People build them for aesthetics, personal workflow, and the joy of creating a “portable system” that feels intentional.

The Cyberdeck Cafe explicitly frames builds as modular systems with guides and shared patterns, which encourages the use of repeatable subassemblies (including custom boards) when it improves build quality. [S10]

Hackaday’s cyberdeck coverage provides a broader secondary view of the community: many builds are one-offs, but repeated themes (portable enclosures, custom input/output, wiring discipline) show up again and again. [S11]

---

## 51.1.7 Suggested figures

1) **Decision matrix**: wiring versus perfboard versus custom PCB, with criteria columns (repeatability, robustness, iteration time, serviceability).
2) **Complexity curve**: number of wires versus assembly time and error rate, showing why complexity grows non-linearly.
3) **Subsystem boundary diagram**: a cyberdeck split into modules with connector boundaries; highlight which boundaries benefit most from a PCB.
4) **Iteration loop timeline**: design → fabrication → assembly → test → revision, with typical “unknowns” at each stage.

---

## 51.1.8 Practical takeaway

Build a PCB when you want a subsystem to become a repeatable, mechanically stable artifact.

Do not build a PCB simply because wiring feels messy in the middle of experimentation.

When the design is stable and the wiring complexity is high, a PCB is often the cleanest way to reduce errors, improve robustness, and make future service less painful.

---

## Sources

- [S1] IPC (via ANSI Webstore): *IPC‑2221C — Generic Standard on Printed Board Design* (standard entry; full standard is paid) — https://webstore.ansi.org/standards/ipc/ipc2221c2023
- [S2] Lawrence Berkeley National Laboratory (LBNL): *IPC‑2221 (PDF copy)* (widely circulated reference copy; use for terminology and scope) — https://www-eng.lbl.gov/~shuman/NEXT/CURRENT_DESIGN/TP/MATERIALS/IPC_2221.pdf
- [S3] CERN: *CERN Design Guidelines* (institutional engineering guidance; includes PCB-related design practices across disciplines) — https://design-guidelines.web.cern.ch/
- [S4] CERN Open Source Program Office: *Working with the CERN PCB Design Office* (institutional process guidance emphasizing review, constraints, and manufacturability) — https://ospo.docs.cern.ch/howtos/cern-pcb-design-office/
- [S5] OSH Park Docs: *KiCad Design Rule Setup* (example of fabrication-driven rule constraints integrated into a design workflow) — https://docs.oshpark.com/design-tools/kicad/kicad-design-rules/
- [S6] PCBWay: *PCBWay Custom Design Rules* (fabricator constraints that affect whether a design is manufacturable) — https://www.pcbway.com/helpcenter/design_instruction/PCBWay_Custom_Design_Rules.html
- [S7] JLCPCB: *PCB Basics 2: Design Guidelines* (fabricator guidance on basic PCB layout constraints and common manufacturability rules) — https://jlcpcb.com/blog/pcb-basics-2-design-guidelines
- [S8] Sierra Circuits (ProtoExpress): *How to Set Up Design Rules in KiCad* (practical link between manufacturing constraints and design rule checking) — https://www.protoexpress.com/blog/how-to-set-up-design-rules-kicad/
- [S9] DigiKey Maker: *CYBERDECK Expansion Plate* (community-style project using a purpose-built board to organize cyberdeck expansion connections) — https://www.digikey.com/en/maker/projects/cyberdeck-expansion-plate/efb6a67a24f4417084d12746020a3c1e
- [S10] The Cyberdeck Cafe: *Cyberdeck Build Guide* (community guide encouraging modular build patterns and serviceability) — https://cyberdeck.cafe/build
- [S11] Hackaday: *Cyberdecks (category/tag coverage)* (secondary source summarizing the cyberdeck build culture and recurring design patterns) — https://hackaday.com/category/cyberdecks/
- [S12] How-To Geek: *Building Cyberdecks Is the Geek Hobby You Need to Check Out* (accessible secondary overview of the cyberdeck hobby and motivations) — https://www.howtogeek.com/building-cyberdecks-is-the-geek-hobby-you-need-to-check-out/
