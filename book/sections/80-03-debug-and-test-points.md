# 80.3 Debug and test points

A **test point** is a deliberate, accessible location on a circuit where you can measure voltage or signal behavior. In a cyberdeck, test points are the difference between “I think it’s broken” and “I know what is wrong.”

This chapter explains why test points are essential, where to place them, and how to balance access with safety and board space.

---

## 80.3.1 Why test points matter

Every cyberdeck eventually needs diagnosis.

A system that cannot be probed is a system that cannot be repaired.

Test points reduce diagnostic time because they let you answer simple questions quickly:

- Is the power rail at the correct voltage?
- Did the processor reset line assert?
- Is the serial console alive?

Practical design-for-test resources emphasize that test points are not an afterthought. Cadence’s guidance on designing for test stresses that test access must be planned early, and that test point placement competes with board space and component clearance. [S1]

---

## 80.3.2 What a test point actually is

A test point can be a labeled pad, a through‑hole pin, or a dedicated connector. It does not need to be complicated.

The key property is **access**: a probe can reliably contact it without shorting nearby components.

In manufacturing contexts, test points also support automated test methods such as **in‑circuit test** (ICT) and **flying probe** testing. These methods require consistent spacing and clear access to multiple points across the board. [S2]

Even if your cyberdeck is a one‑off, the same logic applies: the easier it is to probe the board, the faster you can debug it.

---

## 80.3.3 The essential test points in a cyberdeck

A minimal, practical set of test points includes:

- **Ground reference.** A clearly marked ground point that you can clip to without guessing.
- **Primary power rails.** For example, the input rail, the regulator output rails, and any battery rail.
- **Reset and enable signals.** These tell you whether the system is being held in reset or is failing to start.
- **Serial console.** If your compute platform supports a console, a clearly labeled access point is invaluable.
- **Critical interfaces.** If a single cable or connector causes most failures (for example, a display or storage link), provide a test point near that interface.

The goal is not to expose every signal. It is to expose the signals that answer the most common questions.

---

## 80.3.4 Placement guidance: access without damage

Test points create their own risks. They can be shorted by a probe, they can be accidentally bridged, and they can inject noise if placed poorly.

Good placement follows three rules.

First, **make them probe‑friendly.** Keep them out of dense component clusters so a probe can land without contacting a neighbor.

Second, **keep ground close.** A nearby ground reference reduces measurement error and makes probing safer.

Third, **avoid high‑speed traces** unless the test point is necessary. Probing a high‑speed line can change its behavior.

Design‑for‑test guidance emphasizes that test access is a tradeoff and should be planned before routing. [S1]

---

## 80.3.5 Labeling and documentation

A test point with no label is a missed opportunity.

Labels should be legible and meaningful. “TP1” is less useful than “TP_5V” or “TP_RESET.”

A short test‑point map in your build notes can save hours later.

Many professional workflows treat documentation as part of the test strategy. Design‑for‑test guidelines often include recommendations on labeling and consistent access patterns. [S2]

---

## 80.3.6 Test points and field service

Cyberdecks are moved, repaired, and modified. Test points are what make that feasible without specialized fixtures.

In the field, you may only have a multimeter. A clearly labeled ground and power rail test point can be the difference between “works” and “dead.”

If you are building for reliability, treat test points as a service interface, not just a development tool.

---

## 80.3.7 Suggested figures (for a build log or book layout)

**Figure 80.3-A: Minimal test‑point set.** A diagram of a small board showing ground, input rail, regulator outputs, reset, and serial console test points.

**Figure 80.3-B: Probe clearance example.** A layout snippet showing good versus bad spacing around a test point.

**Figure 80.3-C: Test‑point map.** A simplified table‑like diagram (not a literal table) that maps test points to signals and expected voltages.

---

## Sources

[S1] Cadence, “Section 12 – PCB Design: How to Design For Test.” https://resources.pcb.cadence.com/jbj-pcb-design-from-start-to-finish/jbj-section-12-pcb-design-how-to-design-for-test

[S2] Genesys Industries, “Design for Testability Guidelines” (DFT guidelines PDF). http://www.genesysind.com/doc/Genesys-DFT-Guidelines-v2_0.pdf

[S3] PCBSync, “How to Design PCB Test Points for ICT & Flying Probe Testing.” https://pcbsync.com/pcb-test-points/

[S4] IPC‑2221 (Generic Standard on Printed Board Design), referenced for general layout and testability principles.
