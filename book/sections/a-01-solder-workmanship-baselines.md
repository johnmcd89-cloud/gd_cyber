# A.1 Solder workmanship baselines

A solder joint is not “good” because it conducts electricity today.

A solder joint is good because it will continue to conduct electricity after vibration, temperature changes, and repeated handling.

This appendix provides a practical baseline for solder workmanship in cyberdeck builds. It is written for builders who want a repeatable definition of “acceptable,” a set of training exercises to reach that baseline, and an inspection vocabulary for diagnosing defects.

---

## A.1.1 Vocabulary: soldering, flux, wetting, fillet, and defect

**Soldering** is a process of joining metal surfaces using a lower-melting-point filler metal called **solder**. The joint is formed by heating the surfaces and melting the solder so it flows and then solidifies. [S1]

**Solder** is a fusible metal alloy that melts and wets the surfaces being joined, then solidifies to form a bond. In electronics, solder is chosen for reliable electrical connection and reasonable mechanical integrity, not for high structural strength. [S2]

**Flux** is a chemical agent that helps soldering by reducing oxides on metal surfaces and promoting wetting. In practice, flux is what makes solder “flow” onto copper and component leads instead of forming a stubborn ball.

**Wetting** is the behavior where molten solder spreads across the surfaces being joined rather than beading up. Wetting is the difference between a joint that is bonded to metal and a joint that is merely stuck in place.

A **fillet** is the visible, shaped bead of solder that forms where two conductors meet (for example, a component lead and a printed circuit board pad). Fillet shape is one of the most useful clues for inspection.

A **defect** is a condition that makes the joint unreliable, unsafe, or out of specification. A defect is not the same thing as “looks slightly different than the last joint.”

---

## A.1.2 What “acceptable workmanship” means

A workmanship baseline should be explicit.

For cyberdeck work, an **acceptable solder joint** is one that:

- is electrically continuous (it connects what it should connect),
- is electrically isolated from what it should not connect,
- is bonded by wetting to both intended metal surfaces,
- uses an amount of solder consistent with the joint geometry,
- and has not damaged the parts being joined.

The key point is that acceptable workmanship is defined by **evidence of proper wetting and proper heat application**, not by shininess.

Shininess is a weak indicator because different alloys and fluxes produce different surface appearance. In particular, many lead-free solders can appear comparatively dull even when the joint is acceptable.

---

## A.1.3 Acceptance criteria you can apply with a loupe

### Criterion 1: evidence of wetting

Under magnification, a good joint shows solder that has flowed onto both surfaces.

For example, on a through-hole joint, solder should flow onto the pad and onto the lead. On a wire-to-terminal joint, solder should flow into the interface and appear bonded to both the wire and the terminal.

A joint that looks like a “blob sitting on top” is suspicious, because it suggests the solder did not wet the underlying metal.

### Criterion 2: fillet geometry that matches the joint type

The solder fillet should match the geometry of the joint.

On through-hole joints, you are looking for a fillet that is smooth and continuous, without gaps where the solder failed to bond.

On surface-mount joints (components soldered directly to pads on the surface of a board), the fillet should appear to join pad and termination without obvious voids or lifted edges.

A fillet is not required to be perfectly “textbook” to be functional, but gross geometry mistakes often correlate with poor wetting or overheating.

### Criterion 3: no unintended bridges or whiskers

A **solder bridge** is an unintended electrical connection between conductors caused by solder connecting them. Solder mask is commonly used on printed circuit boards to prevent solder bridges. [S3]

For hand work, the baseline rule is simple: if there is a conductive path where there should not be one, it is a reject.

### Criterion 4: no signs of thermal damage

A solder joint can be electrically correct and still be unacceptable if the process damaged the materials.

Common signs include:

- lifted pads (the copper pad separating from the board),
- melted insulation pulled back farther than intended,
- scorched board material,
- or a connector that deformed due to heat.

Thermal damage increases the chance of later mechanical failure.

### Criterion 5: cleanliness appropriate to the job

Flux residues vary.

Some residues are designed to remain; others are meant to be cleaned. A conservative baseline is that the joint should not have sticky, conductive, or corrosive contamination, and should not trap debris.

In cyberdecks, cleanliness is also a mechanical issue: residue can attract dirt and moisture and complicate future rework.

---

## A.1.4 Defect catalog (with photo references)

A defect catalog is useful only if it connects appearance to cause.

### Cold joint / insufficient wetting

A cold joint is typically caused by insufficient heat at the interface, movement during solidification, oxidation, or attempting to melt solder with the iron tip without heating the work.

Inspection cues include:

- solder that appears “stuck” rather than flowed,
- a fillet that does not blend into the pad or lead,
- or a joint that cracks when stressed.

Photo references:

- SparkFun’s through-hole soldering tutorial includes clear step-by-step photos of proper heating and solder flow, which are useful as “what good looks like.” [S5]
- Adafruit’s soldering guide discusses cold joints and the practical causes of poor joints (underpowered iron, long heating times, oxide formation). [S4]

### Solder bridge

A solder bridge is caused by excess solder, insufficient spacing, poor solder mask, or uncontrolled tip technique.

Inspection cues include visible solder connecting adjacent pads or pins.

Photo reference:

- SparkFun’s tutorial includes photos of joints and rework discussions that are useful for understanding how bridges form during through-hole work. [S5]

### Lifted pad

A lifted pad is caused by overheating, prolonged dwell time, repeated rework without temperature control, or mechanical prying.

Inspection cues include a pad that visibly separates from the board or rotates.

In a repair context, a lifted pad is a structural defect. You must treat the connection as mechanically fragile and consider alternative anchoring or repair methods.

### Insulation damage and lack of strain relief

In wires, the defect is often not the solder itself but the way the wire is supported.

If a soldered wire is allowed to flex at the joint, the conductor can fatigue.

Workmanship standards for mission-critical hardware explicitly treat interconnect and harness workmanship as a quality domain, which is a reminder that wiring quality is not “secondary” to electronics. [S7]

---

## A.1.5 Training exercises (progressive)

The purpose of training exercises is not to build a thing. The purpose is to build a motor skill.

### Exercise 1: tinning and heat discipline (wires)

Cut and strip wires of several sizes.

Practice applying heat to the wire and feeding solder so that solder wets the strands without creating a large, stiff, brittle mass.

The pass condition is that the wire remains flexible away from the joint and the solder does not wick excessively under insulation.

### Exercise 2: through-hole “volume control” on scrap boards

Use a scrap through-hole practice board (or perfboard).

Solder a row of identical components (for example, resistors) and focus on consistent results.

The pass condition is consistent wetting and no bridges.

### Exercise 3: rework practice (desolder and resolder)

Deliberately remove and reinstall a component.

This trains restraint and teaches you how much heat a board tolerates.

The pass condition is that pads are not lifted and the joint quality is not degraded after rework.

### Exercise 4: basic surface-mount components

Choose a large surface-mount package size for practice and gradually move smaller.

The pass condition is correct alignment, no tombstoning (one end lifted), and evidence of wetting on both ends.

### Exercise 5: inspection and documentation

For each session, take close-up photos of a few joints and annotate them.

Your goal is to learn what defects look like before they become failures.

---

## A.1.6 A simple scoring rubric

A rubric reduces self-deception.

For each joint, score the following items as pass/fail.

- Wetting to both surfaces.
- No unintended bridges.
- No thermal damage.
- Geometry consistent with joint type.
- Wire support and strain relief (for wires).

If a joint fails, rework it immediately and write down the most likely cause.

Over time, the rubric becomes a training log.

### Suggested figure

**Figure A.1-A: Joint inspection score sheet.**

A one-page form with columns for joint ID, joint type (through-hole, wire, surface mount), pass/fail on each rubric line, and notes.

### Suggested figure

**Figure A.1-B: Defect photo gallery (reference set).**

A two-column gallery layout with “acceptable” vs “reject,” populated by links to photo-heavy references and your own labeled macro photos.

---

## A.1.7 Where formal standards fit

If you need a formal acceptance standard—because you are working with high consequences or producing work for others—there are established workmanship standards for soldered electrical connections.

For example, NASA publishes a soldered electrical connections workmanship standard intended as a baseline for process procedures used in mission-critical contexts. [S6]

Even if you do not adopt such a standard as-is, its existence is a useful reminder: soldering is a process discipline, and acceptance criteria can be explicit.

---

## A.1.8 Sources

[S1] Wikipedia, *Soldering* (high-level definition of the process). https://en.wikipedia.org/wiki/Soldering

[S2] Wikipedia, *Solder* (definition and why solder alloys are used). https://en.wikipedia.org/wiki/Solder

[S3] Wikipedia, *Solder mask* (explains the role of solder mask in preventing solder bridges). https://en.wikipedia.org/wiki/Solder_mask

[S4] Adafruit, *Adafruit Guide To Excellent Soldering* (practical causes of cold joints; tool selection; includes photos). https://learn.adafruit.com/adafruit-guide-excellent-soldering

[S5] SparkFun, *How to Solder: Through-Hole Soldering* (step-by-step technique with clear photos; includes rework discussion). https://learn.sparkfun.com/tutorials/how-to-solder-through-hole-soldering/all

[S6] NASA, *NASA-STD-8739.3: Soldered Electrical Connections* (standard metadata and scope; workmanship baseline concept). https://standards.nasa.gov/standard/nasa/nasa-std-87393

[S7] NASA, *NASA-STD-8739.4: Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring* (interconnect workmanship context and scope). https://standards.nasa.gov/standard/nasa/nasa-std-87394

[S8] Adafruit, *Collin’s Lab: Soldering* (introductory training resource; cautions about using the correct flux and solder type). https://learn.adafruit.com/collins-lab-soldering
