# A.3 ESD checklist

Electrostatic discharge (ESD) is invisible until it is not.

A static discharge can be large enough to damage sensitive electronics without a visible spark. This appendix provides a practical ESD checklist for cyberdeck builders, organized around three situations: bench setup, handling rules, and packing/transport.

---

## A.3.1 Vocabulary: electrostatic discharge and ESD-safe packaging

**Electrostatic discharge (ESD)** is a sudden flow of electric current between two differently charged objects when they come close together or when the dielectric between them breaks down. [S1]

ESD is a normal physical phenomenon, but some electronic components are sensitive to it and can be degraded or destroyed by small discharges.

**ESD-protective packaging** is packaging designed to prevent static buildup and to route any discharge around the contents. Antistatic bags are a common example. [S2]

---

## A.3.2 Bench setup checklist (minimum viable)

The goal of a bench setup is to create a controlled path for charges to dissipate without passing through sensitive components.

A minimal ESD bench setup includes:

- an ESD mat or dissipative surface,
- a defined ground reference for that surface,
- a wrist strap (used when touching exposed electronics),
- and an area free of synthetic fabrics that generate charge easily.

### Practical notes

A mat without a known ground reference is only marginally better than bare wood.

You do not need laboratory-grade equipment for a cyberdeck, but you do need consistency. A consistent, grounded work surface reduces risk far more than a perfect but unused setup.

### Suggested figure

**Figure A.3-A: Minimal ESD bench layout.**

A top-down diagram showing a grounded mat, a wrist strap lead attached to the mat’s grounding point, and a designated “ESD-safe zone” for exposed boards.

---

## A.3.3 Handling rules (boards, cables, and modules)

Handling rules are simple and largely behavioral.

The key idea is to avoid discharging through sensitive pins or traces.

### Core handling rules

- Touch a grounded object before handling electronics.
- Hold circuit boards by their edges rather than by components or pins.
- Avoid synthetic clothing that generates static (especially in low humidity).
- Keep parts in ESD-safe packaging until you are ready to install them.

### Practical caution

If a part is mounted in a case, it is less exposed than a bare board. The highest risk is during assembly, testing, and rework, when boards and modules are outside their protective enclosures.

---

## A.3.4 Packing and transport checklist

Packing for transport is not the same as storage.

During transport, the enclosure and contents experience vibration, temperature changes, and handling shocks. ESD-safe packaging should be combined with mechanical protection.

### Packing steps

1) **ESD-safe packaging:** Place bare boards and modules in antistatic bags or trays. [S2]
2) **Mechanical protection:** Add cushioning or foam that does not generate excess static.
3) **Isolation:** Do not allow bare boards to rub against each other or against metal tools.
4) **Labeling:** Mark bags or containers with the part name and revision to avoid unnecessary handling later.

### Suggested figure

**Figure A.3-B: Packing cross-section.**

A cross-section diagram showing a module in an antistatic bag, placed inside a padded compartment with a label on the exterior.

---

## A.3.5 Why standards exist (context only)

ESD is a real, testable stressor.

IEC 61000-4-2 is an immunity standard that defines reproducible ESD test methods and levels for electronic products. The existence of such standards is a reminder that ESD is not folklore; it is a measurable risk. [S3]

Similarly, the EOS/ESD Association maintains standards and training around ESD control programs. [S4]

You do not need full compliance testing for a cyberdeck. You do need consistent habits.

---

## A.3.6 Community context: field builds demand pragmatic discipline

Community cyberdeck builds are often assembled in garages, studios, and temporary workspaces.

In that context, a simple, repeatable ESD checklist is more valuable than a perfect laboratory setup that is never used. Community documentation and iteration culture reinforce this: stable habits beat heroic fixes. [S5]

---

## A.3.7 Sources

[S1] Wikipedia, *Electrostatic discharge* (definition and physical description). https://en.wikipedia.org/wiki/Electrostatic_discharge

[S2] Wikipedia, *Antistatic bag* (ESD protective packaging; static-dissipative vs conductive layers). https://en.wikipedia.org/wiki/Antistatic_bag

[S3] Wikipedia, *IEC 61000-4-2* (ESD immunity standard; test method context). https://en.wikipedia.org/wiki/IEC_61000-4-2

[S4] EOS/ESD Association, *ESDA* (standards and training context for ESD control programs). https://www.esda.org/

[S5] Hackaday, *cyberdeck tag archive* (secondary source summarizing the culture of iterative cyberdeck builds). https://hackaday.com/tag/cyberdeck/
