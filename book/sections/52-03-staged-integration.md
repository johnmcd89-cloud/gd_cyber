# 52.3 Staged integration

Cyberdecks fail at integration boundaries.

Individually, a compute module can work.

A display can work.

A battery can work.

But when you connect them together, small mistakes compound: a harness pinout swap, a noisy power rail, a connector that loosens under motion, or a high‑speed link that becomes marginal when routed through a hinge.

**Staged integration** is the practice of assembling and testing a system in deliberate increments.

Each increment has an explicit goal and an explicit test.

Instead of asking “does the cyberdeck work,” you ask “does the power subsystem work under load,” then “does the compute subsystem boot on that power,” then “does the display link remain stable when the lid is opened and closed,” and so on.

This chapter explains how to do staged integration for a cyberdeck, including a practical ladder from breadboard prototypes through a final assembly.

---

## 52.3.1 Definitions (integration, verification, and staged integration)

**Integration** is the process of connecting subsystems together so they operate as a whole.

**Verification** is checking that the system meets defined requirements.

A **stage** is an intermediate build state that is intentionally testable.

A staged integration plan is therefore a sequence of intermediate builds, each with a defined verification step.

Systems engineering references emphasize this idea because late integration failures are expensive.

The National Aeronautics and Space Administration (NASA) Systems Engineering Handbook treats integration and verification planning as core systems engineering activities, not optional cleanup work. [S1]

Cyberdeck implication:

Your integration plan is also your debugging plan.

---

## 52.3.2 The staged integration ladder (from breadboard to final)

Staged integration is easiest to apply when you recognize that “prototype” has multiple levels.

A practical ladder is:

1) breadboard or bench prototype,
2) prototype harness and bench packaging,
3) semi-final assembly in the enclosure,
4) final assembly and regression tests.

Each stage should reduce one category of uncertainty while keeping the system observable.

### Stage A: Breadboard or bench prototype

The goal of this stage is functional validation.

You are proving that major components can work together in principle.

Typical characteristics:

- temporary wiring,
- external power supplies,
- easy access to test points,
- minimal mechanical constraints.

This is where you discover basic incompatibilities (wrong display driver board, insufficient power budget, missing software support).

### Stage B: Prototype harness

The goal of this stage is interface validation.

You turn temporary wiring into a deliberate harness with labels, connector pinouts, and strain relief.

Harness planning and testing practices matter here.

If you do not test harnesses before installing them, you will spend time debugging faults that are purely wiring issues.

NASA workmanship standards for cables and harnesses explicitly treat support, strain relief, and verification as requirements for reliable interconnects, which aligns with the harness-as-subassembly mindset. [S2]

### Stage C: Semi-final assembly

The goal of this stage is packaging validation.

You install subsystems into the enclosure, but you avoid irreversible finishing.

Examples of irreversible finishing include:

- trimming cable ties permanently,
- applying permanent adhesives,
- closing access panels that require major disassembly.

At this stage, you validate:

- cable routing through hinges,
- connector access,
- thermal behavior,
- and mechanical interference.

### Stage D: Final assembly and regression

The goal of this stage is stability.

After final assembly, you repeat a defined test suite.

If the final assembly broke something, you want to know *what* broke and *when*.

A regression test is simply re-running the same tests after changes to ensure nothing regressed.

Cyberdeck implication:

Final assembly is the worst time to discover a pinout mistake.

Staging makes that unlikely.

---

## 52.3.3 An integration ladder you can actually run

The ladder above becomes useful when it is turned into a checklist.

A practical integration ladder is:

1) power subsystem bring-up,
2) compute boot on that power,
3) storage stability,
4) display stability,
5) input stability,
6) radio integration,
7) “closure tests” (tests that only happen when the enclosure is closed and the deck is handled).

### Step 1: Power subsystem bring-up

Do not treat power as “just a battery.”

Power bring-up should verify:

- polarity at each connector,
- each voltage rail under expected load,
- and current consumption within expected bounds.

A current-limited supply is a safe bring-up tool.

### Step 2: Compute boot

The compute subsystem should boot on the deck’s power system.

This is where voltage drop and transient load problems show up.

Define the boot success criteria.

For example:

- boots to a login prompt,
- runs for ten minutes without reboot,
- survives a CPU stress test.

### Step 3: Storage

Storage failures can look like software problems.

Test basic read and write operations.

If storage is removable, test insertion and removal cycles.

### Step 4: Display

Display integration often couples mechanical and electrical constraints.

Test:

- the video link,
- backlight power and control,
- and stability across lid motion.

If the display uses high-speed links, recognize that some failures are signal-integrity issues.

Vendor high-speed layout guidance provides a useful reminder: discontinuities, stubs, and broken return paths can create intermittent behavior even when the circuit is “correct.” [S3]

### Step 5: Input

Input devices fail in two common ways: mechanical reliability and software mapping.

Test:

- key scanning or button behavior,
- latency,
- cable strain relief.

### Step 6: Radios

Radios add sensitivity to noise.

When you integrate radios, you are also integrating the electromagnetic environment.

Test:

- enumeration (the radio appears on the bus),
- baseline functionality,
- performance with the rest of the system powered.

### Step 7: Closure tests

A closure test is a test that only becomes meaningful when the deck is assembled.

Examples include:

- opening and closing the lid repeatedly,
- carrying the deck by its handle,
- plugging and unplugging external ports,
- running the system while gently flexing harnesses.

The goal is not to abuse the deck.

The goal is to reproduce normal use.

---

## 52.3.4 Logging and checklists (making debugging cumulative)

Integration failures repeat when you do not log what changed.

### Bring-up logs

A bring-up log is a short record of:

- what was tested,
- what passed,
- what failed,
- and what was changed.

Even a simple text file is enough.

The benefit is that you can correlate failures with changes.

### Pass/fail criteria

Define objective criteria.

For example:

- “5 V rail is 4.9–5.1 V under a 2 ampere load,”
- “USB device enumerates within ten seconds,”
- “display shows stable image for thirty minutes with lid motion.”

Objective criteria reduce the temptation to accept “seems fine.”

### Regression tests

Regression tests are a small, repeatable set of checks that you run after changes.

For cyberdecks, a good regression suite includes:

- power rail measurement,
- a compute boot test,
- a display test,
- and a harness wiggle test.

---

## 52.3.5 Intermittents and mechanical coupling

The hardest cyberdeck failures are intermittent.

Intermittents are often mechanical:

- a connector backs out,
- a wire fatigues near a stiff soldered region,
- a harness rubs on an edge,
- or a cable moves and stresses a delicate port.

Harness testing methods such as continuity checks and controlled wiggle tests are practical ways to provoke and identify intermittent faults.

Automotive diagnostic practice formalizes the wiggle test as a way to locate intermittent wiring faults by monitoring resistance changes while the harness is flexed. [S4]

Cyberdeck implication:

If a fault only appears when you touch a cable, treat it as an interconnect problem until proven otherwise.

Mechanical mitigation is often the correct fix: strain relief, rerouting, grommets, and proper support.

---

## 52.3.6 Community patterns (how builders stage integration)

Staged integration is not unique to formal engineering.

It shows up naturally in successful hobby builds.

TechNIK’s cyberdeck instructions explicitly break wiring into multiple harness milestones, which is a practical staged-integration strategy: build and finish a harness, then integrate it. [S5]

The Cyberdeck Cafe build guide encourages modular, repeatable subassemblies and emphasizes planning around enclosure constraints, which aligns with staged integration and checkpoint testing. [S6]

Secondary summaries of cyberdeck culture emphasize that many builds are one-offs, which makes staged integration even more valuable: you do not have a production line to catch mistakes, so you must catch them with process. [S7]

---

## 52.3.7 Suggested figures

1) **Integration ladder diagram**: breadboard → prototype harness → semi-final assembly → final regression.
2) **Checkpoint table**: one row per integration step, with “test,” “pass criteria,” and “common failure modes.”
3) **Bring-up log example**: a one-page example log with a change note and a test result.
4) **Intermittent fault map**: connector exits, hinge regions, and edge pass-throughs highlighted as high-risk zones.

---

## 52.3.8 Practical takeaway

Staged integration is how you turn a cyberdeck from a risky one-shot assembly into a controlled build.

Define stages that keep the system observable.

Attach a checkpoint test to every stage.

Log results.

Then, when something fails, you know what changed—and you know where to look.

---

## Sources

- [S1] NASA: *NASA Systems Engineering Handbook (NASA/SP‑2016‑6105 Rev 2)* (integration and verification planning as part of systems engineering) — https://soma.larc.nasa.gov/lws/vfmo/pdf_files/%5bNASA-SP-2016-6105_Rev2_%5dnasa_systems_engineering_handbook_0.pdf
- [S2] NASA: *NASA‑STD‑8739.4A — Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring* (treats harness work as engineered with support and verification) — https://standards.nasa.gov/sites/default/files/standards/NASA/A/4/nasa-std-87394a_w_change_4_0.pdf
- [S3] Texas Instruments: *High-Speed Interface Layout Guidelines (SPRAAR7)* (illustrates why high-speed integration can fail due to discontinuities and return path issues) — https://www.ti.com/lit/pdf/spraar7
- [S4] Pico Technology (PicoAuto): *Wiggle test (resistance)* (practical intermittent-fault test method via controlled harness flexing) — https://www.picoauto.com/library/automotive-guided-tests/system-tests/wiggle-test-(resistance)/AGT-899-wiggle-test/
- [S5] Hackaday.io: *TechNIK’s Cyberdeck — Instructions* (community build that stages harness integration as milestones) — https://hackaday.io/project/192073/instructions
- [S6] The Cyberdeck Cafe: *Cyberdeck Build Guide* (community guide emphasizing modular subassemblies and staged build discipline) — https://cyberdeck.cafe/build
- [S7] How-To Geek: *Building Cyberdecks Is the Geek Hobby You Need to Check Out* (secondary overview of cyberdeck culture and one-off build drivers) — https://www.howtogeek.com/building-cyberdecks-is-the-geek-hobby-you-need-to-check-out/
