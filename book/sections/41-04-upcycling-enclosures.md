# 41.4 Upcycling enclosures

To **upcycle an enclosure** means to reuse an existing case (the *donor case*) as the shell for a new device.

In cyberdecks, upcycling is popular because it can deliver benefits that are difficult to reproduce from scratch:

- a mature hinge and latch system,
- a durable outer shell,
- and, in rugged cases, a proven sealing surface.

Upcycling also has a trap.

The donor case’s strengths are usually concentrated in a few engineered details.

If you modify those details casually—especially around ports and hinges—you can create a device that is harder to maintain and less reliable than a purpose-built enclosure.

This chapter provides a practical rubric for evaluating donor cases, and it focuses on two recurring constraint zones: **ports** and **hinges**.

---

## 41.4.1 What kinds of donor cases are common

Cyberdeck builders upcycle a wide range of donor cases:

- rugged “Pelican-style” hard cases,
- tool cases,
- surplus instrument cases,
- laptop shells,
- and metal boxes such as ammo cans.

The right choice depends on your use case.

A rugged case may be ideal for field use.

A tool case may be adequate indoors.

A metal box may provide electromagnetic shielding advantages but can be heavier and can create grounding and corrosion considerations.

Cyberdeck culture tends to value portability and modification, which is why upcycled “found enclosures” remain a recurring pattern. [S23]

---

## 41.4.2 A donor-case evaluation rubric

A donor case is not just “big enough.”

A good evaluation considers at least seven dimensions.

### (1) External and internal envelope

Measure what you can actually use.

Internal ribs, foam, and curvature often reduce usable volume.

Plan the internal stack-up: boards, battery, cabling, and clearances.

### (2) Hinge and latch integrity

A donor case that is “almost broken” will not become reliable after you add electronics.

Check for play in hinges, cracks near hinge pins, and latch fatigue.

### (3) Material and modification realism

Plastic cases drill differently than aluminum.

Fiber-reinforced plastics can splinter.

Metal cases can create burrs and conductive debris that are dangerous around electronics.

### (4) Sealing and the ingress protection claim (if any)

If the case claims an ingress protection rating (often abbreviated as an **IP rating**), treat that as a capability you may preserve.

The IP system is standardized in IEC 60529 (Degrees of protection provided by enclosures, “IP Code”). [S1]

A drilled hole, a poorly mounted connector, or a damaged gasket changes the system.

### (5) Pressure equalization and environmental cycling

Many rugged cases include a pressure equalization feature.

This affects usability (easy opening after pressure changes) and can affect moisture behavior.

Pelican’s Protector case pages explicitly frame watertight sealing and pressure equalization as key features. [S2]

Other rugged-case vendors present a similar architecture: sealing plus pressure management. [S4][S7][S8]

### (6) Serviceability

Ask: how often will you open the case?

If it will be opened often, plan an internal mounting strategy that tolerates repeated access.

### (7) Supply and ethics

Use legitimately sourced donor cases.

Avoid questionable surplus channels.

Upcycling is about reuse, not about skirting authorization.

---

## 41.4.3 Ports: the fastest way to ruin a good case

A rugged donor case often fails at the ports.

A port modification can introduce:

- leaks,
- cable strain failures,
- and mechanical interference with opening and closing.

### Panel mounting as a system

A **panel mount** is an interface where a connector is mounted through a wall.

A good panel mount is not only “a hole and a nut.”

It is a system of:

- cutout geometry,
- connector flange,
- gasket or sealing washer,
- fasteners and torque,
- and strain relief.

Vendor panel-mount accessories exist because this is difficult to improvise reliably.

For example, Pelican’s panel frame accessory describes sealing the panel interface with a polymer O-ring. [S3]

SKB’s iSeries panel mounting ring kits similarly emphasize a gasketed panel mounting path for iSeries cases. [S6]

### Hardware geometry is not negotiable

When you choose panel connectors, you inherit their mechanical standards.

For instance, Neutrik’s D-size panel connectors are widely used because they have consistent cutout geometry and mechanical drawings. [S12]

For sealed power connectors, manufacturers such as Bulgin publish data sheets that include panel mounting and sealing-related dimensions for their product families. [S13]

Cyberdeck implication:

Select connectors early and design the port panel around their published cutouts.

Do not “make a hole and hope.”

### Strain relief and cable bend radius

A case that is carried is a case that pulls on cables.

Strain relief is therefore structural.

A simple and common strain relief pattern is a cordgrip or bushing that clamps the cable at the enclosure boundary.

Heyco’s strain relief product references show the intended use of such parts as mechanical load-bearing elements at cable entry points. [S14]

Cyberdeck implication:

Do not let your connector pins be the strain relief.

### A caution about accessory kits

Not every port or panel kit preserves waterproofness.

For example, NANUK’s panel-mount kit documentation explicitly notes limitations around waterproof gaskets for that kit. [S5]

Cyberdeck implication:

Accessory ecosystems reduce risk, but they do not eliminate the need to read constraints.

---

## 41.4.4 Hinges: the keep-out zone you must respect

A hinge is not just a pivot.

It creates a moving geometry with clearance requirements.

The **hinge keep-out zone** is the volume that must remain free so the lid can open, close, and latch without interference.

Common hinge-related mistakes in upcycled cases include:

- routing a cable across the hinge line with too small a bend radius,
- placing connectors where the lid compresses them,
- and building an internal structure that blocks full opening.

A practical way to plan hinge constraints is to select a maximum opening angle and then draw the swept volume of the lid.

Hinge selection and placement guidance from hardware vendors provides helpful conceptual framing for how hinge types map to use cases (for example, when you want free-swing hinges versus controlled-friction hinges). [S15]

Cyberdeck implication:

Treat the hinge region as a mechanical design problem.

If the hinge is stressed by cables, the case will fail at the hinge.

> **Figure idea:** “Hinge keep-out diagram.” Draw the case in side view and show the lid sweep arc. Highlight the region where cables must not bind.

---

## 41.4.5 Sealing and gasket management in upcycled builds

If a donor case is gasketed, the gasket is a component.

It has cleanliness, compression, and aging constraints.

The Parker O-Ring Handbook is a widely used reference for O-ring sealing design concepts and failure modes, and it is directly relevant to how case gaskets work. [S10]

A broader sealing reference such as *Seals and Sealing Handbook* provides additional context on seal materials and degradation. [S11]

Cyberdeck implication:

If you want a case to remain sealed, avoid modifications that create uneven compression or introduce sharp edges near the gasket.

---

## 41.4.6 Condensation: why sealed does not mean dry

Sealed cases can trap moisture.

They can also ingest humid air during pressure and temperature cycling and then condense internally.

Engineering references on protective vents emphasize the condensation mechanism: pressure differentials can draw humid air through leak paths, and moisture can condense inside the device. [S9]

Community practice reflects this.

People commonly store desiccant in cases and discuss moisture management as part of maintaining water resistance in the real world. [S22]

Cyberdeck implication:

If you are building a sealed cyberdeck, plan a moisture strategy (assembly dryness, desiccant, and inspection).

> **Figure idea:** “Condensation pathway.” A diagram showing humid air, cooling, and condensation on internal metal parts.

---

## 41.4.7 Community patterns: what builders actually do

Real upcycling builds are useful references because they expose the practical constraint zones.

Examples include:

- rugged cyberdecks that emphasize keeping the case water-tight while adding ports and internal mounting, [S16]
- project logs that document printed internal structures and panel integration, [S17][S18]
- and personal build posts that show bulkhead connectors and port-panel practices. [S20]

Upcycling is also used outside cyberdecks in portable “ground station” style builds, which makes those communities useful references for port and hinge constraints. [S19]

Cyberdeck implication:

Borrow patterns, not just aesthetics.

The most valuable information in a build log is usually where the builder changed their mind after discovering hinge or port constraints.

---

## 41.4.8 Suggested figures

1) **Donor-case checklist.** A one-page evaluation rubric with pass/fail checks.

2) **Port-panel layout.** A connector plate showing sealed bulkhead connectors, gasket interfaces, and strain relief.

3) **Hinge keep-out.** Lid sweep arc and cable routing zones.

4) **Decision matrix.** Compare donor case types (rugged case, tool case, instrument case, ammo can) across: mod difficulty, sealing potential, weight, and serviceability.

---

## 41.4.9 Practical takeaway

Upcycling enclosures is a valid cyberdeck strategy.

The outer shell is often the easy part.

The hard part is preserving what made the donor case good while adding ports and internal mounting.

If you evaluate donor cases with a rubric, design ports as systems (connector cutout, gasket, torque, strain relief), and respect hinge keep-out zones, you can build a cyberdeck that is both durable and serviceable.

---

## Sources

- [S1] IEC 60529 listing (Ingress protection / IP code) — https://webstore.iec.ch/en/publication/2452
- [S2] Pelican: 1450 Protector case (watertight sealing and pressure equalization features) — https://www.pelican.com/us/en/product/cases/protector/1450
- [S3] Pelican: 1520PF panel frame accessory (panel sealing with polymer O-ring) — https://www.pelican.com/us/en/accessory/cases/special-application-panel-frame/1520pf
- [S4] NANUK: NANUK 935 product page (IP67 case architecture and pressure valve context) — https://nanuk.com/products/nanuk-935
- [S5] NANUK: panel mount kit (panel integration limitations) — https://nanuk.com/products/panel-mount-kit
- [S6] SKB: iSeries panel mounting ring kit (gasketed panel mounting) — https://www.skbcases.com/products/i-series-1309-panel-mounting-ring-kit
- [S7] SKB Europe: iSeries case example (IP67 and pressure equalization context) — https://skb-europe.com/products/industry-military/single-lid-cases/iseries/3i-2222-12b-c/
- [S8] Seahorse: SE520 protective case (IP67 and interior foam options) — https://seahorse.net/products/520-protective-case
- [S9] Gore: condensation and vented sealed enclosure guidance — https://www.gore.com/resources/gore-protective-vents-reduce-condensation-sealed-enclosures
- [S10] Parker: O-Ring Handbook landing page (O-ring sealing engineering reference) — https://discover.parker.com/Parker-ORing-Handbook-ORD-5700
- [S11] Flitney: *Seals and Sealing Handbook* (engineering reference) — https://www.sciencedirect.com/book/9780080994161/seals-and-sealing-handbook
- [S12] Neutrik: D-size panel connector mechanical drawing/cutout reference (example product page) — https://www.neutrik.us/en-us/product/ne8fdp-b
- [S13] Bulgin: panel-mount power connector data sheet (standard series) — https://www.bulgin.com/products/pub/media/bulgin/data/Standard_power.pdf
- [S14] Heyco: snap-in cordgrip strain relief reference — https://heyco.com/products/strain-relief-bushings/snap-in-cordgrip-strain-reliefs/heyco-snap-in-cordgrip-strain-reliefs-straight-thru/
- [S15] Southco: hinge types and selection context — https://southco.com/en_us_int/resources/Types-of-hinges-and-where-to-use-them
- [S16] Hackaday: rugged cyberdeck in a Pelican-style case (ports and watertightness) — https://hackaday.com/2022/03/12/rugged-cyberdeck-makes-the-case-for-keeping-things-water-tight/
- [S17] Hackaday.io: Cyberdeck1 project (case upcycling and waterproof ports) — https://hackaday.io/project/183892-cyberdeck1
- [S18] Hackaday.io: Cyberdeck1 instructions (internal printed structures and port panels) — https://hackaday.io/project/183892/instructions
- [S19] ArduPilot Discourse: Raspberry Pi in Pelican case ground station build (port and hinge realities in practice) — https://discuss.ardupilot.org/t/building-a-raspberry-pi-4-pelican-case-ground-station-computer-for-mission-planner/64391
- [S20] r/cyberDeck: Pelican-case build example (port panel practice) — https://www.reddit.com/r/cyberDeck/comments/15af8l5/wip_cyberdeck_build_in_pelican_case/
- [S21] r/pelicancase: moisture/condensation discussion (community practical) — https://www.reddit.com/r/pelicancase/comments/1gfyfab
- [S22] r/pelicancase: moisture-tightness and desiccant discussion (community practical) — https://www.reddit.com/r/pelicancase/comments/1ojsu0y
- [S23] Hackaday: cyberdeck contest culture framing (portability and modification) — https://hackaday.com/2022/08/08/load-your-icebreakers-the-2022-cyberdeck-contest-is-here/
