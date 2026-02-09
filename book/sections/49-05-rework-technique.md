# 49.5 Rework technique

Rework is the process of correcting an assembly after the first soldering pass.

In a cyberdeck build, rework is normal.

The goal is not to “never rework.” The goal is to rework **without damaging the printed circuit board (PCB)** or degrading the component.

This chapter covers safe part removal, the tools that matter, and the habits that prevent lifted pads.

---

## 49.5.1 When rework is appropriate

Rework is appropriate when it restores electrical and mechanical integrity.

It is not appropriate when repeated heating cycles are likely to destroy the board (for example, when pads are already lifting).

High‑reliability workmanship standards treat rework as a controlled process: materials, cleanliness, and inspection still apply. [S1]

Cyberdeck implication:

If you find yourself “touching up” the same joint repeatedly, stop and change the process (tip size, flux strategy, heat delivery) instead of adding more thermal cycles.

---

## 49.5.2 The core rework tools

A small rework kit can fix most problems:

- **Flux** (to restore wetting and reduce bridges).
- **Desoldering braid** (also called solder wick) to remove excess solder.
- **Solder sucker** (vacuum pump) for through‑hole joints.
- **Hot‑air rework** for multi‑lead surface‑mount components.
- **Tweezers and gentle mechanical support** to lift parts only when solder is fully molten.

Chemtronics’ “Soder‑Wick” guide describes the basic technique for using braid effectively (including the importance of contact, dwell, and fresh braid). [S4]

---

## 49.5.3 Step‑by‑step patterns for common rework jobs

### Removing a solder bridge (fine‑pitch leads)

1) Add flux to the bridged pins.
2) Use drag soldering or wick to pull excess solder away.
3) Inspect under magnification.

Weller’s drag‑soldering guidance explains why heavy flux and controlled motion often remove bridges faster than repeated poking. [S2]

### Cleaning a pad (preparing for a retry)

1) Add flux.
2) Use braid to remove old solder.
3) Re‑tin lightly (a thin layer is enough).

### Replacing a two‑terminal part (resistor/capacitor)

1) Add flux.
2) Heat one end and lift slightly.
3) Heat the other end and remove.
4) Clean pads with braid.
5) Place new part, tack one end, then finish the other.

The key safety rule:

Do not pry. If the part does not lift easily, the solder is not fully molten.

---

## 49.5.4 Preventing lifted pads (the failure you cannot undo)

A pad lifts when the copper adhesive bond to the PCB fails.

The usual causes are:

- excessive heat or dwell time,
- pulling a lead before solder is fully molten,
- repeated thermal cycling,
- using force as a substitute for heat.

Repair manuals for circuit board rework emphasize controlled heating and mechanical gentleness because pad damage is often permanent. [S6]

Cyberdeck implication:

If you suspect a pad is fragile, switch to the least‑aggressive method (more flux, more even heat, less mechanical force) and accept that “good enough” is sometimes safer than “perfect.”

---

## 49.5.5 Community patterns (how hobbyists do rework)

Community repair guides tend to converge on the same practical advice:

- add flux early,
- use braid for cleanup,
- use hot air for multi‑lead parts,
- inspect constantly.

iFixit’s soldering and desoldering guide is a representative example, showing both soldering and removal workflows and emphasizing that adding flux often fixes poor wetting during rework. [S5]

---

## 49.5.6 Suggested figures

1) **Rework toolkit**: flux, braid, pump, hot‑air station, tweezers.
2) **Pad lift mechanism**: pad bonded to substrate, force arrow showing peel.
3) **Bridge removal**: fluxed pins → drag pass → wick cleanup.
4) **Two‑pad replacement**: lift one end → lift other → clean → tack → finish.

---

## 49.5.7 Practical takeaway

Rework is a heat‑and‑force problem.

Use more flux than you think you need, apply heat evenly, and never pull until solder is molten.

Those habits prevent lifted pads, which is the most expensive rework failure.

---

## Sources

- [S1] NASA: *NASA‑STD‑8739.3 — Soldered Electrical Connections* (workmanship baseline; process expectations that still apply during rework) — https://nepp.nasa.gov/docuploads/06AA01BA-FC7E-4094-AE829CE371A7B05D/NASA-STD-8739.3.pdf
- [S2] Weller: *Drag Soldering White Paper* (rework method for bridges and fine‑pitch leads) — https://www.blog.testequity.com/wp-content/uploads/2020/10/UE-Weller-WhitePaper.pdf
- [S3] IPC: *IPC‑7711/21 — Table of Contents* (industry reference for rework, modification, and repair procedures; document itself is paywalled) — https://www.electronics.org/TOC/IPC-TOC-7711-21C.pdf
- [S4] Chemtronics: *How To Use Soder‑Wick* (practical guidance on desoldering braid technique) — https://www.chemtronics.com/how-to-use-soder-wick
- [S5] iFixit: *How To Solder and Desolder Connections* (community repair guide; rework workflows and flux use) — https://ifixit-guide-pdfs.s3.amazonaws.com/pdf/ifixit/guide_750_en.pdf
- [S6] PACE: *CirKit Repair Manual* (repair manual covering board‑level rework concepts and damage prevention) — https://dn721601.ca.archive.org/0/items/manual_Pace_CirKit_Repair_Manual/Pace_CirKit_Repair_Manual.pdf
