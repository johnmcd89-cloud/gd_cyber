# 49.4 SMD basics

Surface‑mount devices (SMDs) are components designed to sit directly on the surface of a printed circuit board.

They are smaller, faster to assemble, and dominant in modern electronics.

This chapter covers the basic hand‑soldering techniques (tack, drag, reflow), the common defects, and how to avoid them.

---

## 49.4.1 What makes SMD different

Unlike through‑hole parts, SMDs rely on **pads** and solder fillets on the board surface.

A stable joint depends on pad geometry, solder volume, and even heating on both sides of the part.

Manufacturer assembly guidelines (for example, Melexis’ surface‑mount technology (SMT) soldering application note) emphasize pad geometry, paste volume, and controlled heating as the foundation of reliable joints. [S1]

---

## 49.4.2 Tack, drag, and reflow (the three core techniques)

There are three common hand techniques:

- **Tack‑and‑reflow**: place a small solder dot on one pad, tack one lead, then reflow the joint with additional solder.
- **Drag soldering**: flood a row of fine‑pitch leads with flux and drag a solder‑loaded tip across them.
- **Hot‑air reflow**: apply solder paste, place the part, and heat evenly until paste melts.

Weller’s drag‑soldering white paper explains why heavy flux and controlled motion prevent bridges. [S3]

Adafruit’s soldering guide illustrates tack‑and‑reflow as a reliable method for small SMD parts. [S5]

---

## 49.4.3 Common defects and how to avoid them

SMD defects are mostly **thermal and geometric**:

- **Bridging**: excess solder connects adjacent pads.
- **Tombstoning**: one end of a chip component lifts as the other end wets first.
- **Insufficient wetting**: solder does not form a proper fillet on the pad.

Melexis and onsemi assembly guides both emphasize balanced heating and correct solder volume to prevent these defects. [S1] [S2]

Practical fixes are consistent: add flux, reduce solder volume, and improve heat symmetry across the pads.

---

## 49.4.4 Inspection and rework tools

Inspection is typically visual for hand‑assembled SMDs.

The IPC‑A‑610 standard (IPC, the Association Connecting Electronics Industries) defines acceptable solder fillet shapes and wetting criteria for electronic assemblies. [S4]

Rework tools are part of the normal SMD toolkit:

- **Flux** (to improve wetting and reduce bridges).
- **Solder wick** (to remove excess solder).
- **Hot air** (for evenly heating multi‑lead packages).

---

## 49.4.5 Community patterns (how makers learn SMD)

Maker tutorials focus on repeatable, low‑risk techniques.

Adafruit’s soldering guide and Instructables’ beginner SMD guide show how tack‑and‑reflow makes tiny parts approachable. [S5] [S6]

The MIT Fab Academy SMD soldering guide provides a concise, practical checklist for hand assembly. [S7]

---

## 49.4.6 Suggested figures

1) **SMD pad anatomy**: pad, paste deposit, and fillet.
2) **Tack‑and‑reflow sequence**: dot → place → reflow → finish.
3) **Drag‑soldering pass**: fluxed leads with dragged tip path.
4) **Defect gallery**: bridge vs tombstone vs good joint.

---

## 49.4.7 Practical takeaway

SMD soldering is a controlled heat‑and‑volume problem.

Use flux, control solder volume, and heat both ends evenly.

Those habits prevent most bridges and tombstones.

---

## Sources

- [S1] Melexis: *Guidelines for Surface Mount Technology (SMT) Soldering* (pad geometry and process control for SMDs) — https://www.melexis.com/-/media/files/documents/application-notes/handling-and-assembly/guidelines-surface-mount-technology-smt-soldering-application-note-melexis.pdf
- [S2] onsemi: *Soldering and Mounting Techniques Reference Manual* (assembly guidance and defect avoidance for SMT) — https://www.onsemi.com/pub/Collateral/SOLDERRM-D.PDF
- [S3] Weller: *Drag Soldering White Paper* (technique for fine‑pitch leads and bridge avoidance) — https://www.blog.testequity.com/wp-content/uploads/2020/10/UE-Weller-WhitePaper.pdf
- [S4] IPC: *IPC‑A‑610G — Acceptability of Electronic Assemblies (Table of Contents)* (industry acceptability standard for solder joints) — https://www.electronics.org/TOC/IPC-A-610G.pdf
- [S5] Adafruit: *Adafruit Guide to Excellent Soldering* (community guide covering SMD techniques) — https://cdn-learn.adafruit.com/downloads/pdf/adafruit-guide-excellent-soldering.pdf
- [S6] Instructables: *A Complete Beginner’s Guide to SMD Soldering* (step‑by‑step community tutorial) — https://www.instructables.com/A-Complete-Beginners-Guide-to-SMD-Soldering/
- [S7] MIT Fab Academy: *Surface Mount Soldering* (concise lab guide for hand SMD assembly) — https://fab.cba.mit.edu/classes/863.15/doc/tutorials/electronics_production/smd_solder.html
