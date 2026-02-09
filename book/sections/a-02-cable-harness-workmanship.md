# A.2 Cable/harness workmanship

In cyberdecks, failures often cluster at interconnects.

A well-chosen compute module can be reliable, but a loose connector, an underspecified cable, or a poor crimp can make the entire system intermittent.

This appendix provides practical workmanship baselines for cables and harnesses: what to do, what to inspect, and how to verify that your work is mechanically stable.

---

## A.2.1 Vocabulary: cable, harness, connector, and crimp

A **cable** is an electrical conductor (often multiple conductors) with insulation and, sometimes, shielding.

A **cable harness** (also called a wire harness) is an assembly of wires or cables that transmit signals or electrical power, bundled and secured so it can be installed and maintained as a unit. Harnessing is used to improve mechanical stability under vibration and abrasion, reduce the risk of shorts, and simplify installation. [S2]

A **connector** is a detachable interface used to join wiring to a device or to another harness segment.

A **crimp** (in electrical interconnect) is a joining method where a metal terminal is deformed around a conductor (and often around its insulation) to create a mechanically secure and electrically reliable connection. Crimping is widely used and requires tools matched to the terminal type and wire size. [S3]

---

## A.2.2 Why workmanship matters more than you think

A harness is a mechanical object.

It is exposed to:

- vibration during transport,
- repeated plugging and unplugging,
- torsion from awkward cable routing,
- and heat cycles inside an enclosure.

If the harness is not strain-relieved, these stresses are transferred into terminals, solder joints, and printed circuit board pads.

High-reliability domains treat harness workmanship as a quality discipline. For example, NASA publishes a workmanship standard for crimping, harnesses, and wiring assemblies for critical work. [S1]

You do not need to adopt the same process overhead for a cyberdeck, but it is useful to adopt the same mindset: interconnect quality is testable and inspectable.

---

## A.2.3 Crimp workmanship: standards in the practical sense

“Crimp standards” in the practical sense means that:

- the terminal, wire, and tool are compatible,
- the crimp is repeatable,
- and inspection criteria are defined.

### What a good crimp must do

A good crimp must satisfy two jobs.

First, it must make a reliable **electrical** connection between the conductor and the terminal.

Second, it must make a reliable **mechanical** connection that prevents the conductor from moving at the electrical interface.

Many terminals therefore have two regions:

- a conductor crimp (for electrical and mechanical capture of strands), and
- an insulation crimp (for strain relief).

### Common crimp failure modes

The most common failure modes are process mistakes.

They include:

- selecting the wrong terminal for the wire size,
- using a tool die that does not match the terminal family,
- stripping too much or too little insulation,
- cutting or nicking strands during stripping,
- and crimping the insulation barrel onto bare conductor (or vice versa).

### Inspection cues you can apply in the field

A field inspection should be conservative.

At minimum, verify:

- the wire is fully inserted (strip length matches the terminal design),
- no strands are missing or cut,
- the insulation support is present and not crushing conductor,
- and the terminal is not deformed in a way that prevents proper mating.

If you have a microscope or strong magnifier, look for cracking of the terminal barrel and for evidence that the crimp is centered and symmetrical.

### Suggested figure

**Figure A.2-A: Crimp anatomy (conductor vs insulation barrels).**

A simple labeled sketch showing a terminal with two crimps: the forward crimp around conductor strands and the rear crimp around insulation.

---

## A.2.4 Pull testing: a verification method, not a ritual

A pull test is a simple idea: apply an axial tensile load and verify that the termination does not fail below an expected threshold.

Tensile testing is a general materials test in which a sample is pulled in tension until failure. [S5]

A harness pull test is a focused, practical version of that idea applied to terminations.

### What a pull test is trying to prove

A pull test does not prove that every crimp is perfect.

It does prove that:

- your tool setup is not catastrophically wrong,
- your wire preparation is consistent,
- and your failure mode is reasonable.

A desirable failure mode for a crimped termination (when you pull hard enough) is often wire breakage rather than the wire slipping out of the terminal.

### A conservative pull test procedure (high-level)

A practical procedure is:

1) **Select samples.** Test a small number of terminations from the build, especially when you change wire gauge, terminal type, or tooling.
2) **Fixture the terminal.** Clamp the terminal so that the pull force is aligned with the terminal axis.
3) **Pull steadily.** Apply tension smoothly rather than with a jerk.
4) **Record outcomes.** Note the force (if measured) and the failure mode.

In aviation maintenance practice, advisory guidance exists for acceptable wiring and interconnect workmanship, including crimping and inspection practices, which is a useful conservative reference model for “field-repairable wiring.” [S4]

### What to do if you cannot measure force

If you do not have a force gauge, you can still use pull testing qualitatively:

- compare “known good” crimps to your current batch,
- look for outliers that fail at notably lower effort,
- and treat any early pull-out as an immediate process failure.

For quantitative targets, use the terminal manufacturer’s application specification or workmanship standard for that terminal family.

### Suggested figure

**Figure A.2-B: Simple pull test rig.**

A diagram showing a terminal fixed in a vise or fixture, wire aligned axially, and a hanging-weight or spring-scale concept.

---

## A.2.5 Labeling schemes: make harnesses readable under stress

A harness is only maintainable if it is legible.

Labeling is not cosmetic. It is a reliability feature because it prevents wrong connections during repair.

### Labeling goals

A good labeling scheme answers:

- what is this connector,
- what does it mate with,
- and what is pin 1 (or the reference orientation).

It also supports revision control: “this harness is revision H3.”

### A practical scheme for cyberdecks

A common, readable convention is:

- **H#** for harness assemblies (H1, H2),
- **J#** for receptacles/jacks (J1, J2),
- **P#** for plugs (P1, P2),
- and **W#-##** for individual conductors within a harness segment.

The exact letters do not matter as much as consistency.

### Physical labeling methods

Prefer labels that survive abrasion and heat.

Two practical methods are:

- heat-shrink labels (printed or hand-marked), and
- self-laminating wrap labels.

If you cannot label every wire, label every harness segment and every connector pair, and keep a pinout table in the deck documentation.

### Suggested figure

**Figure A.2-C: Label legend example.**

A small example showing a connector labeled “J3 (Panel USB)” mating to “P3,” and a harness tag “H2 Rev B,” with wire IDs W2-01 … W2-08.

---

## A.2.6 Community reality: harness discipline enables iteration

Community cyberdeck projects often evolve quickly, which means wiring changes are common.

Hackaday’s coverage of the “Recovery Kit” designs illustrates a revision where the internal wiring and connector choices were simplified as the project iterated. That is a reminder that wiring is a revision-sensitive subsystem and benefits from modular harnessing and clear labels. [S6]

---

## A.2.7 Sources

[S1] NASA, *NASA-STD-8739.4 (Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring)* (scope and purpose metadata). https://standards.nasa.gov/standard/nasa/nasa-std-87394

[S2] Wikipedia, *Cable harness* (definition and why harnessing improves reliability and serviceability). https://en.wikipedia.org/wiki/Cable_harness

[S3] Wikipedia, *Crimp (joining)* (definition; tooling must match terminal type/size). https://en.wikipedia.org/wiki/Crimp_(joining)

[S4] Federal Aviation Administration (FAA), *Advisory Circular AC 43.13-1B (w/ Change 1): Acceptable Methods, Techniques, and Practices – Aircraft Inspection and Repair* (conservative wiring workmanship reference model). https://www.faa.gov/documentLibrary/media/Advisory_Circular/AC_43.13-1B_w-chg1.pdf

[S5] Wikipedia, *Tensile testing* (general concept underlying pull tests). https://en.wikipedia.org/wiki/Tensile_testing

[S6] Hackaday, *Remixed Pi Recovery Kit V2 Offers Another Path* (community build iteration; wiring simplification and connector choice revision). https://hackaday.com/2024/06/12/remixed-pi-recovery-kit-v2-offers-another-path/

[S7] Wiring Harness Manufacturer’s Association (WHMA), *WHMA* (industry context for harness manufacturing and training). https://www.whma.org/

[S8] Wikipedia, *American wire gauge* (wire size standard relevant to terminal/tool matching). https://en.wikipedia.org/wiki/American_wire_gauge
