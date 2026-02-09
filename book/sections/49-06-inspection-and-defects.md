# 49.6 Inspection and defects

Soldering is not finished when you remove the iron.

It is finished when you have evidence that the joint is mechanically sound and electrically correct.

That evidence usually starts with inspection.

This chapter explains what to look for, the defects that matter most in cyberdeck builds, and when to redo work instead of hoping it will “probably be fine.”

---

## 49.6.1 What inspection is trying to prove

Inspection is a method for reducing uncertainty.

For electronics assembly, inspection typically tries to prove three things:

1) **Electrical integrity**: the connection conducts where it should, and does not short where it should not.
2) **Mechanical integrity**: the connection can survive vibration, cable tugging, and handling.
3) **Process integrity**: the joint was formed in a way that will not corrode, crack, or drift over time.

Workmanship standards such as the National Aeronautics and Space Administration (NASA) standard for soldered electrical connections define acceptance expectations for high‑reliability assemblies, including cleanliness and rework discipline. [S1]

The IPC‑A‑610 standard (IPC, the Association Connecting Electronics Industries) is the common industry acceptability reference for what “good” solder joints look like. [S2]

---

## 49.6.2 Inspection levels (visual, electrical, functional)

### Visual inspection

Visual inspection answers: “Does the joint look like a proper joint?”

It catches the majority of solder defects quickly.

### Electrical inspection

Electrical inspection answers: “Does it behave like a conductor (or an insulator) where expected?”

The simplest tools are continuity mode and resistance measurements on a multimeter.

### Functional inspection

Functional inspection answers: “Does the assembled subsystem work under realistic conditions?”

For a cyberdeck this often means wiggling cables gently, tapping the enclosure, and observing whether power or data drops out.

Cyberdeck implication:

Many intermittent failures are mechanical.

If a failure depends on touch, vibration, or cable position, treat it as a solder or connector integrity problem until proven otherwise.

---

## 49.6.3 The defect set that matters most

### Cold joint (redo)

A **cold solder joint** is a joint that solidified without proper wetting.

It often looks dull, grainy, or lumpy.

It is strongly correlated with intermittent behavior.

NASA training materials emphasize that correct heating technique produces smooth, well‑wetted joints and illustrate common failure appearances for trainees. [S3]

### Insufficient wetting (redo)

**Insufficient wetting** means solder did not flow onto the pad or the lead.

This can happen when the joint was too cold, the surface was contaminated, or flux was exhausted.

### Solder bridge (usually redo)

A **solder bridge** is an unintended short between adjacent conductors.

It can be obvious (a visible blob) or subtle (a hairline connection hidden under flux residue).

Adafruit’s soldering “common problems” guide includes bridging and poor wetting as common failure modes and discusses practical fixes. [S4]

### Tombstoning (redo)

**Tombstoning** is a surface‑mount defect where one end of a small two‑terminal part lifts upright.

It happens when wetting forces become unbalanced during heating.

Cadence’s discussion of tombstoning emphasizes that uneven heating and asymmetrical pad design contribute to the defect. [S6]

---

## 49.6.4 Tools that make inspection easier

Inspection quality is limited by what you can see.

For most builders, the highest return tools are:

- **Magnification** (a loupe or microscope for fine pitch work).
- **Lighting** (raking light makes cracks and bridges visible).
- **A multimeter** (continuity, resistance, diode checks).

Community repair guides also emphasize cleaning and re‑tinning as part of “inspection,” because residue can hide defects and a poorly tinned tip produces repeatable bad joints. [S5]

---

## 49.6.5 When to redo (a practical rule set)

Redo work when the cost of rework is lower than the cost of uncertainty.

In cyberdeck terms, that is most of the time.

Redo immediately when:

- the joint looks cold or poorly wetted,
- the joint moves under gentle probing,
- a connector pin has any doubt about bridging,
- behavior changes when the cable is touched.

Try “clean and retest” first only when:

- you suspect flux residue is hiding the joint,
- the joint looks mechanically correct,
- the electrical test already passes.

NASA‑STD‑8739.3 treats rework as permitted but controlled: reworked joints must still meet workmanship requirements, and repeated heating cycles are not a free pass. [S1]

---

## 49.6.6 Suggested figures

1) **Defect gallery**: cold joint vs good joint, bridge vs clean pins, tombstone vs correct placement.
2) **Inspection workflow**: visual → continuity → functional (with notes on what each catches).
3) **Raking light demonstration**: same joint under overhead lighting vs side lighting.
4) **“Redo decision” flowchart**: uncertain visual geometry → redo; clean + retest only when joint is clearly formed.

---

## 49.6.7 Practical takeaway

Inspection is not pessimism.

It is how you prevent intermittent problems from escaping into the field.

If you cannot explain why a joint is acceptable, redo it while the board is still on the bench.

---

## Sources

- [S1] NASA: *NASA‑STD‑8739.3 — Soldered Electrical Connections* (workmanship expectations; rework discipline and acceptability context) — https://nepp.nasa.gov/docuploads/06AA01BA-FC7E-4094-AE829CE371A7B05D/NASA-STD-8739.3.pdf
- [S2] IPC: *IPC‑A‑610G — Acceptability of Electronic Assemblies (Table of Contents)* (industry acceptability reference for solder joints) — https://www.electronics.org/TOC/IPC-A-610G.pdf
- [S3] NASA: *Student Handbook for Hand Soldering* (training images and examples of good vs bad joints) — https://s3vi.ndc.nasa.gov/ssri-kb/static/resources/NASA%20Student%20Handbook%20for%20Hand%20Soldering.pdf
- [S4] Adafruit: *Guide to Excellent Soldering — Common Problems* (practical defect examples and fixes) — https://learn.adafruit.com/adafruit-guide-excellent-soldering/common-problems
- [S5] iFixit: *Soldering Best Practices* (community guidance on inspection mindset and avoiding common solder failures) — https://www.ifixit.com/Wiki/Soldering_Best_Practices
- [S6] Cadence: *A Look at Factors Causing Tombstoning* (discussion of tombstoning causes and mitigation) — https://resources.pcb.cadence.com/blog/a-look-at-factors-causing-tombstoning
