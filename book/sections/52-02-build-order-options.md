# 52.2 Build order options

A cyberdeck is an integration project.

You are combining electrical subsystems, mechanical packaging, and software into one portable device.

In integration projects, *order matters*.

Build order determines when you discover problems, how expensive those problems are to fix, and whether you end up with a deck that is serviceable rather than fragile.

This section compares common build sequences—mechanical-first versus electronics-first, power-first versus compute-first, and harness-first workflows—and provides practical checkpoints so you can validate progress without gambling on a final “all at once” assembly.

---

## 52.2.1 Why build order matters

Build order is a risk-management tool.

When you build in stages, you validate assumptions early.

When you build everything and only test at the end, every failure becomes ambiguous.

You do not know whether the fault came from wiring, mechanical interference, software configuration, power integrity, or accidental damage during assembly.

Systems engineering guidance treats integration and verification as planned activities for exactly this reason.

The National Aeronautics and Space Administration (NASA) Systems Engineering Handbook emphasizes the role of progressive integration, verification planning, and managing interfaces so that problems are detected and isolated rather than discovered late as system-level surprises. [S1]

Cyberdeck implication:

A “build order” is really a sequence of testable intermediate systems.

---

## 52.2.2 The main choice: mechanical-first versus electronics-first

Many builders frame the decision as “mechanical-first” or “electronics-first.”

Both can be correct.

The deciding factor is what is most uncertain in your project.

### Mechanical-first

A **mechanical-first** build order means you establish enclosure geometry early.

You may prototype:

- the enclosure,
- hinge or lid mechanism,
- mounting points,
- connector locations,
- and cable routing paths.

Mechanical-first is usually best when:

- the enclosure is custom (three-dimensional print, modified case, or unusual form factor),
- connector alignment is critical,
- or human factors (keyboard comfort, screen angle, carry ergonomics) are primary goals.

The main risk is that you “lock in” a mechanical layout before you understand the wiring complexity.

That can force difficult harness routing later.

### Electronics-first

An **electronics-first** build order means you validate power and compute early on the bench.

You bring up:

- power rails,
- the compute module,
- storage,
- a display,
- and input devices,

before committing them to a final enclosure.

Electronics-first is usually best when:

- the enclosure is flexible (off-the-shelf case with room),
- the electrical system is uncertain (new battery chemistry, new display, many peripherals),
- or you expect significant iteration.

The main risk is that you build a working bench system that becomes hard to package.

Cyberdeck implication:

If your deck is primarily about “the object,” go mechanical-first.

If your deck is primarily about “the system,” go electronics-first.

---

## 52.2.3 Common build sequences (and when each works)

Build sequences are usually combinations of the following patterns.

### Power-first

A **power-first** sequence validates the electrical environment before anything expensive is attached.

Typical steps:

1) build the battery, charging, and regulation subsystem,
2) verify each output rail under load,
3) add distribution wiring or a power distribution board,
4) only then attach compute and peripherals.

Power-first is recommended when:

- you are using batteries,
- you have multiple voltage rails,
- or you are integrating high-current loads such as display backlights.

It reduces the risk of damaging compute hardware with reverse polarity, overvoltage, or shorts.

### Compute-first

A **compute-first** sequence validates the “brain” as early as possible.

Typical steps:

1) validate the compute module with a known-good power source,
2) validate storage and boot process,
3) validate basic input and display,
4) then integrate power subsystem and enclosure.

Compute-first is recommended when:

- software configuration is a major uncertainty,
- you are relying on specific drivers,
- or the compute module itself is unfamiliar.

### Harness-first

A **harness-first** sequence treats wiring as an engineered subassembly.

You build harnesses on the bench, test them, then install them.

This approach reduces rework because wiring errors are easier to correct before cables are constrained by the enclosure.

NASA wiring workmanship guidance emphasizes support, strain relief, and verification as part of reliable harness work, reinforcing the idea that harnesses should be treated as planned assemblies rather than improvised wiring. [S2]

### Enclosure-first

An **enclosure-first** sequence is a stricter version of mechanical-first.

You treat the enclosure as the primary artifact.

Typical steps:

1) finish the enclosure geometry and mounting features,
2) mount connectors and human-interface components,
3) route cable paths and define tie-down points,
4) then integrate electronics.

Enclosure-first is appropriate when the enclosure is the dominant constraint.

For example, a hinged lid with a tight display cable path can make cable routing the hardest part of the entire project.

---

## 52.2.4 Checkpoints: what to test after each stage

A build order is only useful if it has checkpoints.

A checkpoint is a small test that tells you whether the stage is correct before you proceed.

### Power subsystem checkpoints

After building the power subsystem, verify:

- correct polarity at connectors,
- each voltage rail is present,
- and the rails remain stable under expected load.

Bring-up is safer when you use a current-limited power source.

If the system draws too much current, current limiting reduces damage.

### Harness checkpoints

Before installing a harness into the enclosure:

- perform continuity checks,
- perform isolation (short) checks,
- and perform gentle flex (“wiggle”) tests to identify intermittent faults.

A continuity test with a multimeter is a standard first step, and vendor guidance such as Fluke’s continuity-testing reference explains what continuity mode does and how it should be used. [S3]

### Compute subsystem checkpoints

Validate compute in increasing complexity:

1) power on,
2) boot to a known state,
3) verify storage is readable and stable,
4) verify a basic user interface (local display or serial console).

This staged approach makes it easier to separate hardware faults from software configuration errors.

### Display subsystem checkpoints

Display integration often fails at the boundary between power and high-speed signaling.

Validate:

- backlight power and control,
- a stable video link using known-good cables and adapters,
- and correct mechanical routing through hinges or lids.

If the display interface uses high-speed links (for example, HDMI), it is often safer to avoid custom routing and use proven modules and cables, as discussed in the high-speed caveats section. [S4]

### Radio subsystem checkpoints

For radios, test both function and interference.

Validate:

- the radio enumerates on its data interface,
- the antenna is connected and mechanically strain-relieved,
- and performance does not collapse when the power subsystem is active.

Radio subsystems are often sensitive to noise from power converters.

This is another reason staged integration is valuable.

---

## 52.2.5 Iteration strategy: how to avoid rebuilding everything

Build order interacts with revision control.

A good strategy is to identify what you will freeze early and what you expect to change.

### Freeze mechanical decisions at stable boundaries

You rarely need to freeze the entire enclosure early.

Instead, freeze:

- mounting hole patterns,
- connector cutouts,
- and cable routing corridors.

Leave internal volume flexible if possible.

### Freeze electrical interfaces before internal details

If you define stable interfaces (connector type, pinout, voltage rail assignments), you can revise subsystems behind those interfaces without rewriting the entire build.

This is the same reason interface control documents exist in large systems: stable interfaces allow independent iteration. [S1]

### Use prototypes deliberately

The first build is a prototype.

Treat it as a learning tool.

If you must choose between a “perfect-looking final enclosure” and “a layout that is easy to probe,” choose probe-ability on early revisions.

Cyberdeck implication:

A prototype that is easy to debug is more valuable than a prototype that is easy to photograph.

---

## 52.2.6 Community patterns (how builders actually sequence builds)

Cyberdeck builders often converge on staged integration because the projects are complex and highly customized.

The Cyberdeck Cafe build guide encourages a modular approach and emphasizes planning around subsystems and mechanical packaging, which naturally supports both mechanical-first and harness-first sequences. [S5]

TechNIK’s detailed cyberdeck instructions similarly treat wiring harnesses as separate milestones, which is consistent with a harness-first strategy. [S6]

Secondary summaries of the cyberdeck hobby (for example, mainstream technology press) highlight that many cyberdecks are one-off builds driven by personal constraints and aesthetics, which further increases the value of early mechanical prototyping and staged integration. [S7]

---

## 52.2.7 Suggested figures

1) **Build order flowchart**: a decision tree that selects mechanical-first versus electronics-first based on the dominant uncertainty.
2) **Stage-and-checkpoint timeline**: power → harness → compute → display → radios, with a test checkbox at each stage.
3) **Risk heat map**: components ranked by “cost to replace” and “risk of damage during bring-up,” motivating power-first testing.
4) **Integration boundary diagram**: subsystem blocks with labeled interfaces and test points.

---

## 52.2.8 Practical takeaway

There is no universal “best” build order.

But there is a universal principle: integrate in stages, and validate each stage with a checkpoint before you proceed.

Mechanical-first reduces packaging surprises.

Electronics-first reduces bring-up uncertainty.

Power-first reduces the risk of destroying expensive modules.

Harness-first reduces intermittent faults and rework.

Pick the sequence that attacks your project’s biggest uncertainty first.

---

## Sources

- [S1] NASA: *NASA Systems Engineering Handbook (NASA/SP‑2016‑6105 Rev 2)* (integration, verification planning, and interface management concepts) — https://soma.larc.nasa.gov/lws/vfmo/pdf_files/%5bNASA-SP-2016-6105_Rev2_%5dnasa_systems_engineering_handbook_0.pdf
- [S2] NASA: *NASA‑STD‑8739.4A — Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring* (treats harness work as engineered with support/strain relief and verification) — https://standards.nasa.gov/sites/default/files/standards/NASA/A/4/nasa-std-87394a_w_change_4_0.pdf
- [S3] Fluke: *How to Test for Continuity* (vendor reference for continuity testing practice and limitations) — https://www.fluke.com/en-us/learn/blog/digital-multimeters/how-to-test-for-continuity
- [S4] Texas Instruments: *High-Speed Interface Layout Guidelines (SPRAAR7)* (practical reminder that high-speed interfaces require controlled constraints; motivates using known-good modules/cables when constraints cannot be met) — https://www.ti.com/lit/pdf/spraar7
- [S5] The Cyberdeck Cafe: *Cyberdeck Build Guide* (community guide emphasizing modular build patterns and packaging constraints) — https://cyberdeck.cafe/build
- [S6] Hackaday.io: *TechNIK’s Cyberdeck — Instructions* (community instructions that stage harness work as a milestone) — https://hackaday.io/project/192073/instructions
- [S7] How-To Geek: *Building Cyberdecks Is the Geek Hobby You Need to Check Out* (secondary summary of the cyberdeck hobby and one-off build drivers) — https://www.howtogeek.com/building-cyberdecks-is-the-geek-hobby-you-need-to-check-out/
